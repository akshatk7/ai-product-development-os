# SQL Templates

Parameterized SQL queries for the Call Transcript Scan skill. Replace placeholders with actual values at runtime.

**Configure your table:** Replace `[YOUR_TRANSCRIPT_TABLE]` with your actual transcript table. Map columns as needed.

**Expected columns:**

| Expected Field | Your Column | Description |
|----------------|-------------|-------------|
| `TRANSCRIPT_FULL` | [your column] | Full verbatim transcript with speaker labels |
| `CALLER` | [your column] | The rep/agent who placed the call (for speaker identification) |
| `START_DATE` | [your column] | Call date |
| `CALL_ID` | [your column] | Unique call identifier |
| `ACCOUNT_ID` | [your column] | Customer/account identifier |
| `ACCOUNT_NAME` | [your column] | Customer/account name |

---

## Query 1 — Identify Relevant Calls

```sql
SELECT
  COUNT(*) as total_calls,
  COUNT(DISTINCT CALL_ID) as distinct_calls,
  SUM(CASE WHEN TRANSCRIPT_FULL IS NOT NULL AND TRANSCRIPT_FULL != '' THEN 1 ELSE 0 END) as with_transcript,
  COUNT(DISTINCT ACCOUNT_ID) as distinct_accounts,
  MIN(START_DATE) as min_date,
  MAX(START_DATE) as max_date
FROM [YOUR_TRANSCRIPT_TABLE]
WHERE START_DATE >= '{start_date}'
  AND START_DATE <= '{end_date}'
  AND TRANSCRIPT_FULL IS NOT NULL
  AND TRANSCRIPT_FULL != ''
  AND ({keyword_filter})
  {cohort_filter}
```

**`{keyword_filter}`** expands to: `LOWER(TRANSCRIPT_FULL) LIKE '%keyword1%' OR LOWER(TRANSCRIPT_FULL) LIKE '%keyword2%' OR ...`

---

## Query 2 — Speaker-Aware Signal Detection

The core methodology query. Splits transcripts into speaker turns, builds context windows around topic mentions, and applies signal patterns to customer speech only.

```sql
WITH relevant_calls AS (
  SELECT
    CALL_ID,
    TRANSCRIPT_FULL,
    CALLER,
    START_DATE,
    ACCOUNT_NAME,
    ACCOUNT_ID
  FROM [YOUR_TRANSCRIPT_TABLE]
  WHERE START_DATE >= '{start_date}'
    AND START_DATE <= '{end_date}'
    AND TRANSCRIPT_FULL IS NOT NULL
    AND TRANSCRIPT_FULL != ''
    AND ({keyword_filter})
    {cohort_filter}
),

-- Split transcripts into speaker turns
parsed_turns AS (
  SELECT
    rc.CALL_ID,
    rc.CALLER,
    rc.START_DATE,
    rc.ACCOUNT_NAME,
    rc.ACCOUNT_ID,
    f.INDEX as turn_order,
    TRIM(f.VALUE) as turn_text,
    TRIM(REGEXP_SUBSTR(f.VALUE, '^([^(]+)', 1, 1, 'e')) as speaker_name
  FROM relevant_calls rc,
  LATERAL FLATTEN(
    input => SPLIT(
      REGEXP_REPLACE(
        rc.TRANSCRIPT_FULL,
        '((?:^|\\n)\\s*[A-Z][A-Za-z \\-\\.]+\\s*\\(\\d{1,2}:\\d{2}:\\d{2}\\)\\s*:)',
        '§§§\\1'
      ),
      '§§§'
    )
  ) f
  WHERE TRIM(f.VALUE) != ''
    AND LENGTH(TRIM(f.VALUE)) > 5
),

-- Tag turns that mention topic keywords
topic_turns AS (
  SELECT *,
    CASE WHEN {turn_keyword_filter} THEN 1 ELSE 0 END as mentions_topic
  FROM parsed_turns
),

-- Build context windows and filter to customer speech only
customer_context_speech AS (
  SELECT DISTINCT
    anchor.CALL_ID,
    anchor.CALLER,
    anchor.START_DATE,
    anchor.ACCOUNT_NAME,
    anchor.ACCOUNT_ID,
    ctx.turn_order as context_turn_order,
    ctx.turn_text as customer_text,
    ctx.speaker_name as customer_speaker
  FROM topic_turns anchor
  JOIN topic_turns ctx
    ON anchor.CALL_ID = ctx.CALL_ID
    AND ctx.turn_order BETWEEN anchor.turn_order AND anchor.turn_order + 3
  WHERE anchor.mentions_topic = 1
    AND ctx.speaker_name IS NOT NULL
    AND TRIM(LOWER(ctx.speaker_name)) != TRIM(LOWER(anchor.CALLER))
    AND LENGTH(ctx.turn_text) > 10
),

-- Apply signal patterns to customer context speech
signals_raw AS (
  SELECT
    CALL_ID,
    START_DATE,
    ACCOUNT_NAME,
    ACCOUNT_ID,
    customer_text,
    {signal_case_expression}
  FROM customer_context_speech
)

SELECT
  signal_type,
  COUNT(DISTINCT CALL_ID) as calls,
  COUNT(DISTINCT ACCOUNT_ID) as accounts
FROM signals_raw
WHERE signal_type IS NOT NULL
GROUP BY signal_type
ORDER BY calls DESC
```

---

## Query 3 — Co-Occurring Signals

```sql
-- Uses the same CTE chain as Query 2
WITH call_signals AS (
  SELECT DISTINCT CALL_ID, signal_type
  FROM signals_raw
  WHERE signal_type IS NOT NULL
),

signal_pairs AS (
  SELECT
    a.CALL_ID,
    a.signal_type as signal_1,
    b.signal_type as signal_2
  FROM call_signals a
  JOIN call_signals b
    ON a.CALL_ID = b.CALL_ID
    AND a.signal_type < b.signal_type
)

SELECT
  signal_1,
  signal_2,
  COUNT(DISTINCT CALL_ID) as calls
FROM signal_pairs
GROUP BY signal_1, signal_2
HAVING calls >= 2
ORDER BY calls DESC
LIMIT 15
```

---

## Query 4 — Tenure Breakdown (Optional)

Cross-reference calls with a product activity table to compute customer tenure at time of call. Configure the tenure table and join key for your product.

```sql
-- Configure:
-- [YOUR_ACTIVITY_TABLE]: table with customer activity dates
-- [YOUR_JOIN_KEY]: column to join on (e.g., account_id, store_id)
-- [YOUR_PRODUCT_FILTER]: filter for your product (e.g., product_type = 'premium')

WITH store_tenure AS (
  SELECT
    [YOUR_JOIN_KEY],
    MIN(date_column) as first_active,
    DATEDIFF('day', MIN(date_column), MAX(date_column)) as tenure_days
  FROM [YOUR_ACTIVITY_TABLE]
  WHERE [YOUR_PRODUCT_FILTER]
  GROUP BY [YOUR_JOIN_KEY]
),

call_with_tenure AS (
  SELECT
    cs.*,
    st.tenure_days,
    CASE
      WHEN st.tenure_days IS NULL THEN 'unknown'
      WHEN st.tenure_days = 0 THEN '0 days'
      WHEN st.tenure_days BETWEEN 1 AND 7 THEN '1-7 days'
      WHEN st.tenure_days BETWEEN 8 AND 14 THEN '8-14 days'
      WHEN st.tenure_days BETWEEN 15 AND 30 THEN '15-30 days'
      WHEN st.tenure_days BETWEEN 31 AND 60 THEN '31-60 days'
      WHEN st.tenure_days BETWEEN 61 AND 90 THEN '61-90 days'
      ELSE '90+ days'
    END as tenure_bucket
  FROM call_signals cs
  LEFT JOIN store_tenure st
    ON cs.ACCOUNT_ID = st.[YOUR_JOIN_KEY]
)

SELECT
  tenure_bucket,
  COUNT(DISTINCT CALL_ID) as total_calls,
  {signal_pivot_columns}
FROM call_with_tenure
GROUP BY tenure_bucket
ORDER BY
  CASE tenure_bucket
    WHEN '0 days' THEN 1 WHEN '1-7 days' THEN 2 WHEN '8-14 days' THEN 3
    WHEN '15-30 days' THEN 4 WHEN '31-60 days' THEN 5 WHEN '61-90 days' THEN 6
    WHEN '90+ days' THEN 7 ELSE 8
  END
```

---

## Query 5 — Sample Transcripts

Pull 5 sample transcripts per signal type for qualitative analysis.

```sql
SELECT
  rc.CALL_ID,
  rc.START_DATE,
  rc.ACCOUNT_NAME,
  rc.CALLER,
  rc.ACCOUNT_ID,
  rc.TRANSCRIPT_FULL
FROM signal_calls sc
JOIN relevant_calls rc ON sc.CALL_ID = rc.CALL_ID
ORDER BY rc.START_DATE DESC
LIMIT 5
```

---

## Performance Notes

- **If Query 2 times out:** Break the date range into weekly batches.
- **LATERAL FLATTEN on long transcripts is expensive.** For >5K calls, use `SAMPLE (2000 ROWS)` for initial testing.
- **The speaker label regex** handles `Name (H:MM:SS):` format. Adjust if your transcripts use a different speaker label pattern.

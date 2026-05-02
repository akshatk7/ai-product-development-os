# Signal Patterns

Universal signal dictionary for call transcript analysis. These patterns are applied to **customer speech only** within topic context windows.

Patterns use Snowflake `RLIKE` (POSIX regex) syntax. They are case-insensitive (applied to `LOWER()` text).

## Universal Signals

| Signal | ID | Regex Pattern | Description |
|--------|----|---------------|-------------|
| Cancel intent | `cancel_intent` | `(cancel\|end (it\|this\|the)\|stop (it\|this\|the)\|turn (it\|this) off\|remove it\|get rid of\|don.t want\|take (it\|this) off\|shut (it\|this) off)` | Customer expressing desire to end/stop the product |
| Confusion | `confused` | `(don.t understand\|what does (this\|it)\|confused\|how does\|what is this\|explain.*to me\|i don.t know what\|what.s this)` | Customer doesn't understand what the product does |
| Not working | `not_working` | `(not working\|doesn.t work\|no results\|no customer\|no sales\|no orders\|no one.s (using\|ordering\|buying)\|didn.t get any)` | Customer perceives the product isn't delivering results |
| Spend/cost concern | `spend_concern` | `(too much\|too expensive\|can.t afford\|spending too\|cost too\|afford to\|my out of pocket\|costing me)` | Customer concerned about cost or spend levels |
| Low return | `low_return` | `(not worth\|losing money\|waste\|doesn.t add up\|lose money\|not making.*money\|not profitable)` | Customer doesn't see sufficient ROI |
| Positive/satisfied | `positive` | `(love it\|working great\|happy with\|good results\|going well\|really like\|it.s great\|it works\|been good\|doing well)` | Customer expresses satisfaction with the product |
| Want control | `want_control` | `(want to choose\|pick my own\|more control\|customize\|let me choose\|i want to pick\|choose (which\|what))` | Customer wants more control over the product's behavior |
| Discount concern | `discount_concern` | `(too much discount\|discount too\|giving away\|free is too)` | Customer thinks discounts/offers are too aggressive |

## Adding Topic-Specific Patterns

To add patterns for a specific product area, append a new section below. Keep the same format (signal ID, regex, description). Topic-specific patterns are applied **in addition to** universal patterns.

## Notes

- Patterns are intentionally conservative. They use first-person phrasing to reduce false positives from rep speech that leaked through speaker parsing.
- Signal rates should be treated as **lower bounds** — indirect objections (metaphors, implications, tone) are not captured by regex.
- If a new signal type emerges consistently across multiple runs, add it here with the date it was added.

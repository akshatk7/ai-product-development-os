# Customer Feedback Scan — Gotchas

_Updated as issues are encountered. Read before every run._

1. **Don't scan the same window twice.** Check `runs.log` for the last scan date and start from there.
2. **Filter bot messages and automated alerts.** They add noise, not signal.
3. **Use the current year (check today's date).** Don't pull messages from prior years unless explicitly asked for historical data.
4. **Themes over quotes.** The output should be organized by theme, not by channel. Cross-channel patterns are the valuable insight.

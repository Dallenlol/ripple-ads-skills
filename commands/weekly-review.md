---
description: A structured review of the last N days: spend, results, cost per result per platform and per campaign, what to keep, what to pause, what to test.
---

Run my weekly advertising review for the last 7 days.
1. get_workspace, then recall (so you know what we decided before), then get_performance_summary(days: 7) and list_campaigns(days: 7).
2. For the two campaigns with the most spend, call get_campaign and get_breakdown(dimension: "device") and, on Google, get_breakdown(dimension: "search_term").
3. Report in this shape: Totals (spend, results, cost per result, vs the prior period if the data covers it) → Per campaign table (name, platform, spend, results, cost per result, status, "Ripple-managed?") → Keep / Watch / Act, three bullets each, every "Act" bullet naming the exact tool call you would make and the guardrail that will check it.
4. Do not execute anything. Ask me which "Act" items to apply; apply only those, one at a time, and read the tool result back to me including any changeId.
5. Finish with remember(kind: "session") — what we reviewed, what we changed (with changeIds), what to check next week.

If `$ARGUMENTS` is set, apply it: days may be given as `name=value`.

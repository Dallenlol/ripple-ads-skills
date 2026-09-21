---
description: Find wasted Google search terms in the last N days and propose negative keywords, one campaign at a time.
---

Clean up my Google search terms over the last 30 days.
1. list_campaigns(platform: "google", days: 30) and pick the campaign with the most spend if I did not name one.
2. get_breakdown(dimension: "search_term", days: 30) on that campaign.
3. Show a table of terms with spend but no conversions, sorted by spend, and a second short table of terms that look off-intent even with a conversion. Say what each would cost us to keep per month at the current rate.
4. Propose negatives as exact add_negative_keyword calls (campaign id, term, match type). Do not add any until I say which. Add only what I confirm, one call per term, and read back each result — a refused term means it is protected, so tell me that and move on.
5. remember(kind: "note", title: "Search terms reviewed <date>") listing the terms we excluded, so you do not propose them again.

If `$ARGUMENTS` is set, apply it: days, campaign may be given as `name=value`.

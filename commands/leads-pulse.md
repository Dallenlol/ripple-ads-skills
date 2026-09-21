---
description: How many leads came in, what they cost, and where they came from — counts only, never contact details.
---

Give me a leads pulse for the last 7 days.
Call get_leads_summary and get_performance_summary(days: 7). Report leads by status, cost per lead per platform and per campaign (list_campaigns gives per-campaign leads and spend), and the change versus the previous period if you have it. Never ask for or repeat a lead’s name, phone or email — the tools do not return them and you must not store them with remember.

If `$ARGUMENTS` is set, apply it: days may be given as `name=value`.

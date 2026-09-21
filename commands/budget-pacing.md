---
description: Are we on track against the daily and monthly ceilings? Where is money going unspent, and where is it being spent with no results?
---

Check my budget pacing.
1. get_workspace for the ceilings (daily, monthly, per account) and the currency of each account.
2. get_performance_summary(days: 30) and list_campaigns(days: 30).
3. For each ad account: month-to-date spend versus the monthly ceiling, projected month-end at the current daily run-rate, and whether the projection lands under, at, or over the ceiling.
4. Name any campaign that spent more than 20% of the account total with zero results in the window, and any active campaign whose daily budget is more than twice its average daily spend (money sitting idle).
5. Suggest at most three budget moves, each as a concrete set_campaign_daily_budget call with the amount in cents and the reason. Do not execute until I confirm each one.

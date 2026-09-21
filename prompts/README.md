# Prompt library

The same prompts the Ripple Ads MCP server serves through `prompts/list`. Paste one into any assistant that has the Ripple Ads connector; in Claude Code they are slash commands once this plugin is installed.

## Catch me up (`/ripple-ads:catch-me-up`)

Start of a conversation: what we discussed last time, what has changed in the accounts since, and what needs a decision today.

```text
Catch me up on my advertising. Do it in this order and keep it short:
1. Call get_workspace, then recall. If recall returns a last-session summary, read it first and use suggestedChangesWindowDays for the next step.
2. Call get_changes for that window and list what was executed, what was blocked and by which guardrail, and anything still awaiting approval (get_pending_approvals).
3. Call get_performance_summary for the same window and compare it to what the last session expected.
4. Tell me: what you promised to check last time and what you found; the three numbers that moved most; and the decisions waiting on me. No more than 200 words before the decisions list.
Do not propose a change until I ask. When we finish, call remember with kind "session" summarising what we did and what to look at next time.
```

## Weekly review (`/ripple-ads:weekly-review`)

A structured review of the last N days: spend, results, cost per result per platform and per campaign, what to keep, what to pause, what to test.

```text
Run my weekly advertising review for the last 7 days.
1. get_workspace, then recall (so you know what we decided before), then get_performance_summary(days: 7) and list_campaigns(days: 7).
2. For the two campaigns with the most spend, call get_campaign and get_breakdown(dimension: "device") and, on Google, get_breakdown(dimension: "search_term").
3. Report in this shape: Totals (spend, results, cost per result, vs the prior period if the data covers it) → Per campaign table (name, platform, spend, results, cost per result, status, "Ripple-managed?") → Keep / Watch / Act, three bullets each, every "Act" bullet naming the exact tool call you would make and the guardrail that will check it.
4. Do not execute anything. Ask me which "Act" items to apply; apply only those, one at a time, and read the tool result back to me including any changeId.
5. Finish with remember(kind: "session") — what we reviewed, what we changed (with changeIds), what to check next week.
```

## Budget pacing check (`/ripple-ads:budget-pacing`)

Are we on track against the daily and monthly ceilings? Where is money going unspent, and where is it being spent with no results?

```text
Check my budget pacing.
1. get_workspace for the ceilings (daily, monthly, per account) and the currency of each account.
2. get_performance_summary(days: 30) and list_campaigns(days: 30).
3. For each ad account: month-to-date spend versus the monthly ceiling, projected month-end at the current daily run-rate, and whether the projection lands under, at, or over the ceiling.
4. Name any campaign that spent more than 20% of the account total with zero results in the window, and any active campaign whose daily budget is more than twice its average daily spend (money sitting idle).
5. Suggest at most three budget moves, each as a concrete set_campaign_daily_budget call with the amount in cents and the reason. Do not execute until I confirm each one.
```

## Search terms clean-up (Google) (`/ripple-ads:search-terms-cleanup`)

Find wasted Google search terms in the last N days and propose negative keywords, one campaign at a time.

```text
Clean up my Google search terms over the last 30 days.
1. list_campaigns(platform: "google", days: 30) and pick the campaign with the most spend if I did not name one.
2. get_breakdown(dimension: "search_term", days: 30) on that campaign.
3. Show a table of terms with spend but no conversions, sorted by spend, and a second short table of terms that look off-intent even with a conversion. Say what each would cost us to keep per month at the current rate.
4. Propose negatives as exact add_negative_keyword calls (campaign id, term, match type). Do not add any until I say which. Add only what I confirm, one call per term, and read back each result — a refused term means it is protected, so tell me that and move on.
5. remember(kind: "note", title: "Search terms reviewed <date>") listing the terms we excluded, so you do not propose them again.
```

## What changed? (`/ripple-ads:what-changed`)

Plain-language log of every change Ripple made or refused in the last N days, with who asked and what blocked it.

```text
Tell me what changed in my ad accounts in the last 14 days.
Call get_changes(days: 14, status: "any"). Group by campaign. For each change say what it was (from → to), who asked (me via assistant, the dashboard, or a Ripple agent), whether it ran, and if it was blocked, which guardrail and what that guardrail is for. Flag any executed change that still has rollbackAvailable: true and ask whether I want any of them undone — do not roll anything back on your own.
```

## Leads pulse (`/ripple-ads:leads-pulse`)

How many leads came in, what they cost, and where they came from — counts only, never contact details.

```text
Give me a leads pulse for the last 7 days.
Call get_leads_summary and get_performance_summary(days: 7). Report leads by status, cost per lead per platform and per campaign (list_campaigns gives per-campaign leads and spend), and the change versus the previous period if you have it. Never ask for or repeat a lead’s name, phone or email — the tools do not return them and you must not store them with remember.
```

## Save this session (`/ripple-ads:end-session`)

Before you go: write down what we did, what we decided and what to check next time, so the next conversation starts from here.

```text
We are done for now. Call remember with kind "session", a title like "Session <today’s date>", and content that covers: what we looked at, every change we made with its changeId, what we decided not to do and why, and exactly what to check first next time (with the tool call). Then confirm it saved.
```


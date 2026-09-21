---
name: ripple-ads
description: Use whenever the Ripple Ads MCP connector is available and the user asks about their Meta or Google Ads accounts — performance, spend, leads, what changed, budgets, pausing, negative keywords, bid targets. Teaches the tool order, what a guardrail refusal means, and how to use the per-user memory so the next session starts where this one stopped.
---

# Ripple Ads

Ripple Ads is a hosted MCP server (`https://ads.ripplestrategies.ca/api/mcp`) in front of the
Meta and Google Ads accounts a customer connected to their Ripple workspace. Reads answer
from data Ripple has already synced. Writes never touch a platform directly: each one becomes
a proposed change that Ripple executes only if every guardrail the customer set passes, logs,
and can undo.

## Start every conversation the same way

1. `get_workspace` — plan, accounts, currencies, whether Ripple may change each account
   (`changesAllowed`), data freshness, and the spend ceilings every change is checked against.
2. `recall` — the private notebook for this person in this workspace. Read the last session
   summary first. Use `suggestedChangesWindowDays` as the window for step 3.
3. `get_changes(days)` — what ran, what was blocked and by which guardrail, since last time.

Then answer the question. Money is in minor units (cents) unless a field says otherwise.

## Before the conversation ends

Call `remember` with `kind: "session"`: what was looked at, every change with its `changeId`,
what was decided against and why, and the exact first thing to check next time. Without it
the next session starts blank. Use `kind: "note"` for standing facts ("never pause Brand
Search", "client reviews budgets on Mondays"). Never store a lead's name, phone or email.

## Reads

| Tool | Use it for |
| --- | --- |
| `list_campaigns` | budget, status, managed-by-Ripple flag, spend and results per campaign |
| `get_campaign` | ad sets / ad groups and ads with bids or targets and performance |
| `get_performance_summary` | totals per platform and account with cost-per-result figures |
| `get_breakdown` | one campaign by device, placement, age_gender, region, hour, conversion_category, or `search_term` (Google) |
| `get_pending_approvals` | changes Ripple proposed and is waiting on |
| `get_leads_summary` | lead counts and statuses — never contact details |
| `get_seo_audit` | the latest on-page SEO audit of the customer's website (score, findings with fixes, worst pages); audits are started under Dashboard → SEO, not from chat |

## Writes (need a read-and-change token and a live account)

`set_campaign_status`, `set_campaign_daily_budget` (cents), `set_ad_set_status`,
`set_ad_status`, `set_bid_target`, `add_negative_keyword`, `rollback_change`.

Rules that keep the customer's money safe, and that you follow without being asked:

- Propose first, execute only what the user confirms, one call at a time, and read each
  result back including the `changeId`.
- A refusal is not an error to retry. The result names the guardrail (daily ceiling, monthly
  ceiling, per-change limit, kill switch, protected keyword, account not live). Tell the user
  which limit refused it and where it is set (Dashboard → Guardrails, or Connections for the
  account's live mode).
- `changesAllowed: false` on an account means every write will be refused until the customer
  switches that account to live under Connections. Say so once; do not keep trying.
- Increases are checked harder than decreases. Read the current value with `list_campaigns`
  or `get_campaign` immediately before proposing a change to it.
- Anything executed can be undone with `rollback_change(changeId)` — offer it, never do it
  unasked.

## Slash commands in this plugin

`/ripple-ads:catch-me-up`, `/ripple-ads:weekly-review`, `/ripple-ads:budget-pacing`,
`/ripple-ads:search-terms-cleanup`, `/ripple-ads:what-changed`, `/ripple-ads:leads-pulse`,
`/ripple-ads:end-session`. They are the same workflows the server offers through
`prompts/list`, so a client with prompt support shows them without this plugin.

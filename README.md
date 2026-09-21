# Ripple Ads — skills, prompts and the connector

Everything an AI assistant needs to work your Meta and Google Ads accounts through
[Ripple Ads](https://ads.ripplestrategies.ca): the hosted MCP server, a skill that knows its
tools and guardrails, seven workflow prompts, and a per-user memory so the next conversation
starts where the last one stopped.

## Install

**Claude Code**

```bash
claude plugin marketplace add Dallenlol/ripple-ads-skills
claude plugin install ripple-ads@ripple-ads
```

That adds the `ripple-ads` skill, the slash commands below, and the `ripple-ads` MCP server.
The first call opens Ripple's sign-in; approve read-only or read-and-change access there.

**claude.ai / Claude Desktop** — Settings → Connectors → Add custom connector, name
`Ripple Ads`, URL `https://ads.ripplestrategies.ca/api/mcp`, OAuth fields empty, Connect.
The prompts appear in the prompt picker; paste any from [`prompts/`](./prompts/README.md)
into a client without one.

**Cursor, n8n, anything else** — see Dashboard → Connect inside Ripple for the exact
snippet with a token.

## Slash commands

| Command | What it does |
| --- | --- |
| `/ripple-ads:catch-me-up` | Last session's summary, what changed since, what needs a decision |
| `/ripple-ads:weekly-review` | Totals, per-campaign table, keep / watch / act |
| `/ripple-ads:budget-pacing` | Month-to-date vs ceilings, idle budget, wasted spend |
| `/ripple-ads:search-terms-cleanup` | Google search terms → proposed negatives, one campaign at a time |
| `/ripple-ads:what-changed` | Every change made or refused, who asked, what blocked it |
| `/ripple-ads:leads-pulse` | Leads by status and cost per lead — counts only |
| `/ripple-ads:end-session` | Save what was done and decided for next time |

## Memory

The server keeps a private notebook per person per workspace (`recall`, `remember`,
`forget`). The skill tells the assistant to read it first and write a session summary last.
You can see and delete every note under Dashboard → Connect → Assistant memory.

## SEO tools the assistant can read

Dashboard → SEO gives every workspace a site audit (one a week free, one a day
paid) and paid plans keyword research and daily rank tracking. The assistant
reads them with `get_seo_audit` and `get_rankings`; audits and tracking are
started from the dashboard, not from chat.

## Goes well with

- [claude-ads](https://github.com/Dallenlol/claude-ads) (fork of
  [AgriciDaniel/claude-ads](https://github.com/AgriciDaniel/claude-ads), MIT) — audits,
  scoring and planning across twelve ad platforms. Ripple Ads is the hands; claude-ads is
  the method.
- [marketing-skills](https://github.com/irinabuht12-oss/marketing-skills) — 48 marketing
  skills including SEO and AI visibility. Linked rather than copied: the repository carries
  no licence.

## Development

`commands/` and `prompts/README.md` are generated from the server's prompt list
(`packages/mcp/src/prompts.ts` in the Ripple monorepo); edit them there.

MIT.

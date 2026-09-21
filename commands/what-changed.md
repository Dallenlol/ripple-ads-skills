---
description: Plain-language log of every change Ripple made or refused in the last N days, with who asked and what blocked it.
---

Tell me what changed in my ad accounts in the last 14 days.
Call get_changes(days: 14, status: "any"). Group by campaign. For each change say what it was (from → to), who asked (me via assistant, the dashboard, or a Ripple agent), whether it ran, and if it was blocked, which guardrail and what that guardrail is for. Flag any executed change that still has rollbackAvailable: true and ask whether I want any of them undone — do not roll anything back on your own.

If `$ARGUMENTS` is set, apply it: days may be given as `name=value`.

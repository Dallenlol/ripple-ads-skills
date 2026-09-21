---
description: Start of a conversation: what we discussed last time, what has changed in the accounts since, and what needs a decision today.
---

Catch me up on my advertising. Do it in this order and keep it short:
1. Call get_workspace, then recall. If recall returns a last-session summary, read it first and use suggestedChangesWindowDays for the next step.
2. Call get_changes for that window and list what was executed, what was blocked and by which guardrail, and anything still awaiting approval (get_pending_approvals).
3. Call get_performance_summary for the same window and compare it to what the last session expected.
4. Tell me: what you promised to check last time and what you found; the three numbers that moved most; and the decisions waiting on me. No more than 200 words before the decisions list.
Do not propose a change until I ask. When we finish, call remember with kind "session" summarising what we did and what to look at next time.

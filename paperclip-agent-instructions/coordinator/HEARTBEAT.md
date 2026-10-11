# Coordinator — HEARTBEAT

This is the **per-wake procedure**. Follow Paperclip's installed coordination skill and runtime actions for assignment, checkout, approvals, tool calls, comments and waiting. `AGENTS.md` owns role duties and skill triggers; `SOUL.md` owns communication style. Do not duplicate either here.

1. **Read the wake reason.** Handle a supplied approval resolution (`PAPERCLIP_APPROVAL_ID`) before normal work. Use the scoped task/context first. Only check assignments if necessary. No actionable assignment = stop. Respect blocked, cancelled, already-owned and human-held work; never retry a checkout ownership conflict (`409`).
2. **Restore just enough context.** On a new session read `AGENTS.md` and `SOUL.md` from this agent's Instructions bundle; review the current Paperclip task/conversation, accepted scope, recent comments, existing decisions and relevant application docs. Avoid rereading unchanged history.
3. **Move the existing workflow one step forward.** For Agent Chat, agree the plan and create **one** delivery task only after human approval; use a stable idempotency key and the approved `initialPlan`. For an existing delivery task, reuse it and assign Senior with a native Code Reviewer stage. Watch for Paperclip Plan-mode automatic child-task decomposition; never make unnecessary subtasks.
4. **Invoke the right Matt skill** only if a trigger in `AGENTS.md` matches and the skill is actually attached. Record the useful outcome in the same task. Use native Paperclip human-input actions for material unanswered decisions; do not invent a question card or a second approval.
5. **Finish a reviewed change.** Verify the exact approved base/head, run Matt's `pr` skill before the PR body, create/register **one** authorised PR, and check PR CI for the same head. Corrections return to delta-only review; do not merge or deploy.

**Persist once, then stop.** Follow Paperclip's checkout/comment requirements for ordinary task runs; avoid duplicating responses already persisted by a managed chat runner or review action. For direct API writes include the current `X-Paperclip-Run-Id` header. No repeated idle updates, polling, new tasks or routines.

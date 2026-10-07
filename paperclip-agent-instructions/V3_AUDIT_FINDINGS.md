# v3: cross-check of the three agents

Checked against Paperclip's current published default CEO/default-agent heartbeat; `paperclip-create-agent` Coder and QA templates; Superpowers/RedOak example agents; the official coordination skill; execution-policy, workspace, Agent Chat, plan-decomposition, and GitHub PR workflow docs. These are public upstream references, **not a reading of the live host**.

## Most important corrections

1. **Coordinator — Agent Chat handoff (critical).** A persistent conversation is not the implementation issue. Keep chat in place, create only one ordinary assigned execution task after acceptance, copy the approved plan as `initialPlan` at creation, use a stable `idempotencyKey`, and do not poll for completion. Do not reuse the conversation issue as the developer's task.
2. **Coordinator — avoid automatic task explosion (critical).** Plan-mode acceptance can generate child tasks. Human scope approval and task decomposition are not the same thing. Review proposed task structure before approving and inspect any decomposition the platform has already performed. Prefer a small confirmed scope and one delivery task.
3. **Developer + Coordinator — visible Git attribution (critical for user's privacy preference).** The published Paperclip skill currently mandates `Co-Authored-By: Paperclip <noreply@paperclip.ing>` on agent-authored Git commits. The Coordinator must disclose this **before the first work commit** and obtain a decision rather than promising invisible use. Developer must not quietly omit a required attribution trailer. Host-specific skill/policy may differ.
4. **Privacy of Paperclip Chat (important).** Paperclip Agent Chat conversations can be read by others in the same Paperclip company. Treat a shared Paperclip company as visible to teammates, even if repo changes are clean.
5. **Coordinator — PR CI and artifact record (important).** Reviewer may approve before a PR exists, so PR CI runs later. Coordinator must verify required CI on the exact reviewed SHA before claiming ready; later code edits require fresh independent review. Register `pull_request` work product, not only a text comment.
6. **Reviewer — native stage vs separate review task (important).** When configured as an execution-policy stage, reviewer approves/requests changes on the same issue. When assigned a separate review task, post verdict on **that review task** and complete it even with adverse findings. The reviewer may not have write access to the owner's issue.
7. **Senior UI Developer — Git and privacy checks (important).** Inspect dirty worktrees, stage only in-scope changes, do not commit secrets/customer data, honour hooks and signing, use documented app authentication for UI testing, and record exact tested SHA.
8. **All three — keep existing Paperclip runtime skill and operational safeguards.** Follow scoped wake contexts, no duplicate checkout/comment, 409 ownership stop, human holds, bounded review, precise next action, no speculative tasks. The agent files supplement, not replace, the platform coordination skill.

## Deliberately not imported

- The default CEO's P&L, hiring, roadmap, broad delegation, memory extraction and periodic status rituals: unnecessary and likely to create work/chatter.
- The Superpowers template's mandatory brainstorm for *every* request, 2–5-minute subtask plans, or parallel subagents: conflicts with the user's small one-task delivery preference.
- A blanket TDD-only rule, full app QA matrix, and extra agents on every UI change: add friction without clear benefit for this setup. Preserve any *repository-required* tests and security gates.
- A second PR-opening agent: Coordinator already owns that step.

## Main official sources

- Paperclip skill: https://github.com/paperclipai/paperclip/blob/master/skills/paperclip/SKILL.md
- Agent Chat: https://docs.paperclip.ing/experimental/agent-chat/
- Plan decomposition: https://docs.paperclip.ing/experimental/plan-decomposition-panel/
- Execution policy: https://docs.paperclip.ing/guides/power/execution-policy/
- Code agent template: https://github.com/paperclipai/paperclip/blob/master/skills/paperclip-create-agent/references/agents/coder.md
- Superpowers template: https://github.com/paperclipai/companies/blob/main/superpowers/agents/lead-engineer/AGENTS.md
- PR workflow: https://docs.paperclip.ing/reference/skills/bundled/software-development/github-pr-workflow/
- Workspaces: https://docs.paperclip.ing/guides/projects-workflow/workspaces/

## Check on the host

Do not apply blindly: inspect the installed Paperclip version, actual `skills/paperclip/SKILL.md`, instruction loading, execution policy, Agent Chat usage, plan approval flow, workspaces, credentials, and git settings. Preserve the repo-specific rules and required attribution, and do not modify the working repositories without explicit authority.

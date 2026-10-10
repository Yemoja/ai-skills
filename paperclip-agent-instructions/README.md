# Paperclip: three-agent instruction pack

Prepared 7 October 2026 for **Coordinator**, **Senior UI Developer**, and **Code Reviewer**.

**Start with the nine files in the three agent folders.** They are ready-to-paste instructions, not templates full of unknown paths or commands. The remaining files are guidance for you, not extra material to load into every agent.

This is a researched starting configuration tailored to your reported problems. It has not been tested on your Paperclip host. No agents, host settings, repositories, plugins, or permissions have been changed.

The pack can use [Matt Pocock's upstream skills](https://github.com/mattpocock/skills) without vendoring them. [MATT_POCOCK_SKILLS.md](MATT_POCOCK_SKILLS.md) is the integration contract: it recommends importing the source through Paperclip's Skills Store, maps compatible skills to each role, explains the native Paperclip question-card bridge, and prevents upstream flows from bypassing Paperclip's native task and review controls. The role `AGENTS.md` files contain the operative trigger branches: they call the matching Skill tool at scope, implementation, documentation, and PR-writing moments, then carry the skill's evidence into the existing handoff. The integration file is the shared trigger map and precedence reference, not a replacement for those branches.

## What this is designed to fix

You reported more than 20 long messages for a simple UI change, and more than 10 unapproved subtasks for another small request. You want one conversation with a Coordinator, then implementation, testing, independent review, and a PR. You also want clear English, useful Markdown, obvious decisions, and personal orchestration kept out of work repositories.

The proposed workflow is:

**Agree scope → Senior UI Developer implements and tests → Code Reviewer reviews → Coordinator prepares the PR.**

All stages normally belong to one delivery task. Only Coordinator creates the PR. You keep the merge decision.

**v4 addition:** **Incremental reviews.** Re-requested reviews of the same task/PR use the previous Paperclip decision and stamped base/head SHAs as the baseline. Check earlier findings, the new patch delta and fix regressions; no fresh line-by-line audit of unchanged code. Rebases use Git's `range-diff` where possible. See [V4_INCREMENTAL_REVIEW.md](V4_INCREMENTAL_REVIEW.md) for examples and rebase handling.

**v3 cross-check retained:** Exact base-ref/SHA review, Agent Chat handoff, approved-plan task creation, attribution privacy, native-versus-delegated review, and post-PR CI. See [V3_AUDIT_FINDINGS.md](V3_AUDIT_FINDINGS.md).

## Install in Paperclip

1. **Back up the current instructions and settings.** Save the existing agent files in a private location. Keep necessary host-specific tool notes. Review changes before replacing anything.
2. **Open each agent's Instructions page.** Copy the matching folder's `AGENTS.md`, `HEARTBEAT.md`, and `SOUL.md` into files with those exact names. Use `AGENTS.md` as the entry file. Paste raw Markdown, without adding an outer code fence. Do not paste this README into an agent. If using the Matt integration, also copy `MATT_POCOCK_SKILLS.md` into the common parent directory so each role's `../MATT_POCOCK_SKILLS.md` pointer resolves; preserve that relative layout. The inline trigger branches in each `AGENTS.md` are required: do not replace them with only a link to the integration file. Do not copy upstream `SKILL.md` files into the bundle.
3. **Check a fresh run before real work.** Confirm the correct entry file and application working directory in the invocation details. Verify file-read traces for the supporting files, the shared Matt integration file when used, and repository guidance; do not rely solely on the agent saying it read them.

Prefer managed agent storage or a private external directory outside the work repository. Paperclip's entry file is supplied automatically; the supporting files need explicit loading. Each supplied `AGENTS.md` therefore tells the agent to read its `SOUL.md` and `HEARTBEAT.md`. [S1]

Do not erase a useful existing `TOOLS.md`, remove required coordination skills, or append these files below an old contradictory CEO persona. Merge necessary environment details; remove conflicting personal-workflow instructions deliberately.

If you use Matt Pocock's skills, read [MATT_POCOCK_SKILLS.md](MATT_POCOCK_SKILLS.md) before attaching or invoking them. Import the upstream source once through Paperclip's Skills Store, inspect the pinned revision, and attach only the role-compatible skills. Skill import, attachment, plugin installation, and permission changes are operator-approved setup actions; the agent instructions do not authorise an agent to perform them. Do not copy upstream skill files into this repository.

## Files for each agent

| Agent | Entry and operating rules | Run procedure | Judgement and writing style |
| --- | --- | --- | --- |
| Coordinator | [AGENTS.md](coordinator/AGENTS.md) | [HEARTBEAT.md](coordinator/HEARTBEAT.md) | [SOUL.md](coordinator/SOUL.md) |
| Senior UI Developer | [AGENTS.md](senior-ui-developer/AGENTS.md) | [HEARTBEAT.md](senior-ui-developer/HEARTBEAT.md) | [SOUL.md](senior-ui-developer/SOUL.md) |
| Code Reviewer | [AGENTS.md](code-reviewer/AGENTS.md) | [HEARTBEAT.md](code-reviewer/HEARTBEAT.md) | [SOUL.md](code-reviewer/SOUL.md) |

## Settings to pair with the instructions

These are proposed pilot settings, not changes already applied:

| Setting | Proposed choice |
| --- | --- |
| Heartbeat on interval | Off for all three agents |
| Wake on demand | On |
| Max concurrent runs | One per agent initially |
| Proactive improvement routines | Off while calibrating |
| Task reviewer | Code Reviewer |
| Final delivery stage | Coordinator, where supported |
| Responsible human | You, explicitly configured |
| `executionPolicy.maxReviewRounds` | `2` initially |
| Scope approval | Accept the current bounded plan once before implementation |
| Additional tasks or agents | Ask you first |

Paperclip documents timer-off/event-driven operation as the normal starting point. Turning the timer off is not the same as pausing an agent. [S2]

The review cap counts consecutive agent change requests: with `2`, the second such decision escalates instead of returning automatically to the developer. It is not two guaranteed completed fix cycles. Configure `responsibleUserId` or a suitable human creator; otherwise the documented escalation cannot hand the review to you. Native review and approval stages can operate on the same task. [S3]

Accept the current plan revision through a supported, human-resolvable confirmation. Changing work mode alone is not plan acceptance. Do not allow an agent to approve its own proposed scope. **Important:** Agent Chat handoff creates an ordinary development task from the accepted plan; accepting a Plan-mode task can create child tasks automatically, so inspect that path before using it for a small one-task request. [S4, S10, S11]

## What instructions cannot enforce by themselves

These files guide behaviour; they are not a security boundary or a universal subtask-limit setting. Use actual permissions and runtime controls for hard limits. Verify that controls cover the real route used, including shell commands and not just a connector. Do not weaken your organisation's controls to reduce prompts. [S5]

Some task runs require a persisted comment. Some verified chat flows have the runtime persist the response and manage lifecycle actions. The heartbeat files preserve the installed platform's rules while avoiding a second duplicate report. They do not promise an empty task history. [S3, S6]

**Visibility warning:** Paperclip Agent Chat conversations are visible to other members of the same Paperclip company. If you use a shared company, the messages themselves are not personal. Use a separate personal company or instance if appropriate and permitted. [S10]

**Privacy warning — decide before first agent-authored commit:** The currently published Paperclip coordination skill requires `Co-Authored-By: Paperclip <noreply@paperclip.ing>` on Git commits made by agents. This makes Paperclip use visible in commit history. The instructions explicitly require disclosing that conflict and following mandatory attribution; they cannot guarantee the workflow remains undisclosed. Review the installed host skill and company rules first. [S6]

If a completed task keeps waking, inspect the wake reason and installed version. Do not keep adding prose to `SOUL.md` to treat a runtime loop.

## How repo instructions are handled

Each agent must read the assigned application's root `README.md` and applicable `AGENTS.md`, `CLAUDE.md`, and related development rules at a new thread/session or repo change. Relevant nested instructions must be read before that area is changed or reviewed. Unchanged context can be reused within a session.

The files do not assume Next.js, Tailwind, a package manager, a test runner, or a branch name. Agents must discover those from the application. Missing repo guidance does not authorise writing new instruction files into your work repo.

## Git comparison and review handoff (v2 retained in v3)

The Coordinator confirms the intended PR target (`baseRef`) once from the application/task settings. The Senior UI Developer works in the issue's real execution workspace and passes the reviewer the repository, workspace path, branch, base ref, **exact commit SHA**, working-tree status, and test results. The Code Reviewer compares the *whole change* with the merge-base/three-dot diff (`git diff BASE...HEAD_SHA`), not just the latest commit, and checks uncommitted changes separately. The reviewer records which SHA was approved. The Coordinator must confirm the final PR targets that base and contains that exact SHA, or re-submit for review.

Why this matters: Paperclip can provision different worktrees with a recorded base ref, while GitHub PRs may target `main`, `develop`, release branches, or stacked branches. The wrong base can produce a misleading review; an approval of an earlier SHA does not approve later code. Paperclip's optional workspace-diff viewer distinguishes uncommitted working-tree changes from changes against the base ref. [S7, S8, S9]

## Check the first five small tasks

Suggested trials: a responsive alignment fix; a keyboard interaction bug; a small React state fix; a UI change with unavailable browser access; and a small fix near an unrelated outdated dependency.

For each trial, check:

- One delivery task, no unauthorised expansion, and no duplicate scope approval.
- A direct first sentence, readable Markdown, and a specific user action only when needed.
- Real test/review evidence, with missing checks stated honestly.
- A concise review and one Coordinator handoff; no repeated idle or waiting discussion.
- A PR for the reviewed code, with private orchestration material excluded.

Treat these as acceptance tests for the setup. The word targets are soft limits, not grounds to omit a serious defect or failed check.

## Other files

- [Matt Pocock skills integration](MATT_POCOCK_SKILLS.md): managed installation, Paperclip Skills Store setup, role compatibility, and conflict rules.
- [v4 incremental review safeguard](V4_INCREMENTAL_REVIEW.md): how repeat reviews reuse prior SHA stamps and findings without reopening unchanged work.
- [v3 cross-check and important differences](V3_AUDIT_FINDINGS.md): earlier audit still applies.
- [Reviewer ref correction](REVIEW_REF_UPDATE.md): v2's branch and SHA selection fix, preserved in v3.
- [Host review prompt](HOST_REVIEW_PROMPT.md): a read-only audit brief for the Claude session on your host.
- [Portable clarity rules](portable/PLAIN_LANGUAGE.md): independent of Paperclip.
- [Claude output style](portable/claude-output-style-plain-language.md): optional reusable writing style.

## Sources

[S1] [Paperclip agent instructions](https://docs.paperclip.ing/guides/org/agents/)

[S2] [Paperclip heartbeats and routines](https://docs.paperclip.ing/guides/projects-workflow/routines/)

[S3] [Paperclip execution policy](https://docs.paperclip.ing/guides/power/execution-policy/)

[S4] [Paperclip work modes](https://docs.paperclip.ing/guides/day-to-day/work-modes/)

[S5] [Claude Code: instructions versus enforced settings](https://code.claude.com/docs/en/memory)

[S6] [Paperclip coordination skill](https://github.com/paperclipai/paperclip/blob/master/skills/paperclip/SKILL.md)

[S7] [Paperclip execution workspace base ref / worktrees](https://docs.paperclip.ing/guides/projects-workflow/workspaces/)

[S8] [Paperclip workspace-diff plugin: working-tree and against-ref modes](https://docs.paperclip.ing/reference/plugins/workspace-diff/)

[S9] [GitHub: three-dot comparison of PRs](https://docs.github.com/en/pull-requests/reference/branches)

[S10] [Paperclip Agent Chat](https://docs.paperclip.ing/experimental/agent-chat/)

[S11] [Paperclip plan decomposition](https://docs.paperclip.ing/experimental/plan-decomposition-panel/)

[S12] [Paperclip Coder template](https://github.com/paperclipai/paperclip/blob/master/skills/paperclip-create-agent/references/agents/coder.md)

[S13] [Paperclip GitHub PR workflow skill](https://docs.paperclip.ing/reference/skills/bundled/software-development/github-pr-workflow/)

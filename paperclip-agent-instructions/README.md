# Paperclip: three-agent instruction pack

Prepared 7 October 2026 for **Coordinator**, **Senior UI Developer**, and **Code Reviewer**.

**Start with the nine files in the three agent folders.** They are ready-to-paste instructions, not templates full of unknown paths or commands. The remaining files are guidance for you, not extra material to load into every agent.

This is a researched starting configuration tailored to your reported problems. It has not been tested on your Paperclip host. No agents, host settings, repositories, plugins, or permissions have been changed.

## What this is designed to fix

You reported more than 20 long messages for a simple UI change, and more than 10 unapproved subtasks for another small request. You want one conversation with a Coordinator, then implementation, testing, independent review, and a PR. You also want clear English, useful Markdown, obvious decisions, and personal orchestration kept out of work repositories.

The proposed workflow is:

**Agree scope → Senior UI Developer implements and tests → Code Reviewer reviews → Coordinator prepares the PR.**

All stages normally belong to one delivery task. Only Coordinator creates the PR. You keep the merge decision.

## Install in Paperclip

1. **Back up the current instructions and settings.** Save the existing agent files in a private location. Keep necessary host-specific tool notes. Review changes before replacing anything.
2. **Open each agent's Instructions page.** Copy the matching folder's `AGENTS.md`, `HEARTBEAT.md`, and `SOUL.md` into files with those exact names. Use `AGENTS.md` as the entry file. Paste raw Markdown, without adding an outer code fence. Do not paste this README into an agent.
3. **Check a fresh run before real work.** Confirm the correct entry file and application working directory in the invocation details. Verify file-read traces for the supporting files and repository guidance; do not rely solely on the agent saying it read them.

Prefer managed agent storage or a private external directory outside the work repository. Paperclip's entry file is supplied automatically; the supporting files need explicit loading. Each supplied `AGENTS.md` therefore tells the agent to read its `SOUL.md` and `HEARTBEAT.md`. [S1]

Do not erase a useful existing `TOOLS.md`, remove required coordination skills, or append these files below an old contradictory CEO persona. Merge necessary environment details; remove conflicting personal-workflow instructions deliberately.

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

Accept the plan through the supported confirmation flow. Changing work mode alone is not plan acceptance. Do not allow an agent to approve its own proposed scope. [S4]

## What instructions cannot enforce by themselves

These files guide behaviour; they are not a security boundary or a universal subtask-limit setting. Use actual permissions and runtime controls for hard limits. Verify that controls cover the real route used, including shell commands and not just a connector. Do not weaken your organisation's controls to reduce prompts. [S5]

Some task runs require a persisted comment. Some verified chat flows have the runtime persist the response and manage lifecycle actions. The heartbeat files preserve the installed platform's rules while avoiding a second duplicate report. They do not promise an empty task history. [S3, S6]

If a completed task keeps waking, inspect the wake reason and installed version. Do not keep adding prose to `SOUL.md` to treat a runtime loop.

## How repo instructions are handled

Each agent must read the assigned application's root `README.md` and applicable `AGENTS.md`, `CLAUDE.md`, and related development rules at a new thread/session or repo change. Relevant nested instructions must be read before that area is changed or reviewed. Unchanged context can be reused within a session.

The files do not assume Next.js, Tailwind, a package manager, a test runner, or a branch name. Agents must discover those from the application. Missing repo guidance does not authorise writing new instruction files into your work repo.

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

- [Research and choices](RESEARCH_AND_CHOICES.md): current options, popularity signals, evidence, and trade-offs.
- [Use with Codex and Claude](USING_WITH_CODEX_AND_CLAUDE.md): personal instructions and an optional Claude output style.
- [Host review prompt](HOST_REVIEW_PROMPT.md): a read-only audit brief for the Claude session on your host.
- [Portable clarity rules](portable/PLAIN_LANGUAGE.md): independent of Paperclip.

## Sources

[S1] [Paperclip agent instructions](https://docs.paperclip.ing/guides/org/agents/)

[S2] [Paperclip heartbeats and routines](https://docs.paperclip.ing/guides/projects-workflow/routines/)

[S3] [Paperclip execution policy](https://docs.paperclip.ing/guides/power/execution-policy/)

[S4] [Paperclip work modes](https://docs.paperclip.ing/guides/day-to-day/work-modes/)

[S5] [Claude Code: instructions versus enforced settings](https://code.claude.com/docs/en/memory)

[S6] [Paperclip coordination skill](https://github.com/paperclipai/paperclip/blob/master/skills/paperclip/SKILL.md)

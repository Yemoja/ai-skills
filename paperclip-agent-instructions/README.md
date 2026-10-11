# Paperclip agent instruction pack

**Nine Markdown files: three per agent.** Keep Paperclip's normal instruction structure:

| Paperclip agent | Instructions files |
| --- | --- |
| Coordinator | [AGENTS.md](coordinator/AGENTS.md) · [HEARTBEAT.md](coordinator/HEARTBEAT.md) · [SOUL.md](coordinator/SOUL.md) |
| Senior UI Developer | [AGENTS.md](senior-ui-developer/AGENTS.md) · [HEARTBEAT.md](senior-ui-developer/HEARTBEAT.md) · [SOUL.md](senior-ui-developer/SOUL.md) |
| Code Reviewer | [AGENTS.md](code-reviewer/AGENTS.md) · [HEARTBEAT.md](code-reviewer/HEARTBEAT.md) · [SOUL.md](code-reviewer/SOUL.md) |

## Install

1. In each Paperclip agent's **Instructions** page, add its three matching files with their exact filenames. Set `AGENTS.md` as the entry instructions. Supporting files are explicitly loaded on a new session. Keep them in the agent's own instruction bundle, **not** the application repository.
2. In **Company → Skills → Sources**, import `https://github.com/mattpocock/skills` once. Attach only the appropriate imported skills through each agent's **Skills** tab:

| Agent | Attach |
| --- | --- |
| Coordinator | `grilling`, `research`, `codebase-design`, `domain-modeling`, `wizard`, `writing-for-agents`, `pr` |
| Senior UI Developer | `diagnosing-bugs`, `tdd`, `research`, `prototype`, `codebase-design`, `domain-modeling`, `writing-for-agents` |
| Code Reviewer | `code-review`, `codebase-design`, `writing-for-agents` |

3. Keep Paperclip's own coordination skill and necessary host-specific `TOOLS.md` configuration. Remove or replace any older contradictory role instruction files. You do **not** need `MATT_POCOCK_SKILLS.md`: the role-specific skill index and exact triggers now live inside each `AGENTS.md`.

## What goes where

- **AGENTS.md** — the role, permissions, handoffs and **when to call each Matt skill**.
- **HEARTBEAT.md** — what to do on each Paperclip wake/task run, including waits, review and persistence.
- **SOUL.md** — judgement, tone and concise communication.

An `AGENTS.md` mention does not install a skill. Import and attach actual upstream `SKILL.md` packages as above. The role must genuinely invoke/load its attached skill and never claim it ran otherwise.

**Quick check:** In Paperclip Skill Studio, run safe smoke tests for Coordinator/`writing-for-agents`, Senior/`diagnosing-bugs`, and Code Reviewer/`code-review`. Also try one ordinary harmless task to check that the matching skill is invoked automatically. Matt supplies review analysis; Paperclip owns the native review stage and its approval transition.

The extra historical audit/reference documents in this repo are optional reading, not Paperclip instruction files.

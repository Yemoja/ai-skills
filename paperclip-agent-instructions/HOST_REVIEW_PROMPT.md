# Read-only review request for Claude on the Paperclip host

Review this instruction pack against the actual Paperclip installation. **Inspect and propose changes first; do not apply changes, restart services, install plugins, change permissions, or publish anything yet.**

The three agents are Coordinator, Senior UI Developer, and Code Reviewer. The intended workflow is one bounded task, developer implementation/testing, independent same-task review, and a PR created by Coordinator. The user needs simple English, brief useful comments, obvious decisions, and private orchestration outside work repositories.

## Inspect

1. Identify the installed Paperclip and coding-runtime versions, the agents' actual instruction bundles, entry files, application working directories, active global instructions, and loaded skills. Do not expose credentials.
2. Compare the existing `AGENTS.md`, `HEARTBEAT.md`, and `SOUL.md` against the matching supplied files. Preserve necessary host-specific tool details. Find conflicting CEO/roadmap instructions, auto-delegation rules, verbose update policies, and repeated imported text.
3. Verify repo-startup reading, task ownership, native review/approval routing, responsible-human settings, round counting, timer/demand wakeups, and whether the harness already persists comments.
4. Check actual shell/connector permissions and PR ownership. Verify the real issue execution workspace, its Git branch, recorded `baseRef`, parent project workspace `repoRef`/`defaultRef`, and whether the reviewer sees the developer's exact commit. Confirm the reviewer checks a merge-base/three-dot diff plus uncommitted changes, and that the final PR's base and head SHA match the review approval. Confirm no automatic attribution or private orchestration material surprises the user. Honour mandatory workplace policies.
5. If examples are available, inspect one chatty run and one scope-creep task. Separate prompt problems from duplicate wakeups or configuration errors. Do not infer causes merely from agent names.

## Return

Give a short conclusion first. Then show a compact table: location, observed problem, proposed change. Supply exact proposed diffs and a five-task validation plan. Identify unsupported settings or missing evidence explicitly. Ask only for access or a decision that genuinely blocks the next step.

The pack's README and research notes are for this audit, not additional instructions to concatenate into every agent. Back up existing files before any later approved edit. Do not modify an employer's repository guidance merely to enforce this personal workflow.

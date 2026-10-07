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


## v3: verify platform-specific gaps before applying

- If Coordinator is used through Agent Chat, confirm a persistent conversation remains separate from its implementation task. Validate that the approved plan is included as `initialPlan` at task creation with a stable `idempotencyKey` and no accidental parent/child relationship.
- If Plan-mode approval is used, inspect whether the host auto-decomposes accepted plans into child tasks. Do not replace a one-task delivery model with ten generated subtasks.
- Inspect the installed `paperclip` skill's commit attribution requirement. It may require `Co-Authored-By: Paperclip <noreply@paperclip.ing>`. Report the impact on the user's wish not to disclose Paperclip use. Do not quietly remove required attribution.
- Verify git credential preflight for both developer push and Coordinator PR creation, and required CI after PR creation. Does the configured review/approval path allow an updated commit to receive a fresh independent review?
- Check whether review uses a native executionPolicy stage or a separate delegated review issue. Use the correct completion and comment locations for that mode; do not assume cross-issue writes are permitted.
- Confirm that PRs are registered as `pull_request` work products on delivery tasks, not only mentioned in comments, and that the execution workspace is not prematurely archived.
- Verify that code agents respect existing dirty working trees, commit hooks/signing, sensitive data protections, and do not start follow-up work or unnecessary tests.
- Preserve `TOOLS.md`, explicit installed skills, host-specific credentials and project rules. These files are not a substitute for the runtime Paperclip skill.


## Additional v4 check: repeat reviews

Verify the Code Reviewer uses the last Paperclip review decision/comment as its baseline, records the exact base/head/merge-base SHAs with stable blocking finding IDs, and uses focused delta/rebase review on later requests for the same scope. Ensure Senior UI Developer hands back old and new SHAs with fix IDs, and Coordinator does not restart a full review or evade `maxReviewRounds`. Check that the installed reviewer has access to previous verdicts; if using separate delegated review issues, include earlier verdicts in their self-contained briefs. Do not create extra issues to store review metadata.

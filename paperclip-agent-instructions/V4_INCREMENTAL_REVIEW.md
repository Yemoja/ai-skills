# v4 — Incremental review safeguard

## Why

For the same accepted task/PR, a second or third review must **not restart the entire review from scratch**. Repeat reviews should converge, not generate new optional work.

## Policy

- First review: evaluate the whole change; give all blocking findings together with stable IDs.
- Every review: record **base ref + base SHA + merge-base SHA + reviewed head SHA** inside the existing Paperclip review decision/comment. Do not create another issue just to track the baseline.
- Repeat review: compare **last reviewed revision → current candidate**. Include relevant GitHub PR comments being addressed, verify earlier blockers, inspect corrected/rewritten patches, check fix regressions, and repeat only relevant required checks.
- Rebase: `git range-diff OLD_MERGE_BASE..OLD_HEAD NEW_MERGE_BASE..NEW_HEAD` when both commit ranges exist. Compare patch changes rather than mistakenly treating all upstream changes as new task work.
- Do not reopen accepted requirements, review previously unchanged code for style preferences, or create new follow-up tasks just to continue the conversation.
- A serious, evidence-backed new security/data-loss/correctness problem can still be flagged; a material scope change goes to Coordinator.
- Respect Paperclip's `maxReviewRounds` and human escalation. Do not reset the issue or create fresh review tasks to evade the limit.

## Example: first review decision

```text
Review: changes requested
Scope: same accepted task / PR #42
Base ref: origin/develop
Base SHA: <base_commit_sha>
Merge-base SHA: <merge_base_sha>
Reviewed head SHA: <reviewed_head_sha>
Blocking findings:
- REV-1: Save button remains clickable while submitting — SettingsForm.tsx:82
- REV-2: Focus moves behind modal — SettingsDialog.tsx:35
Checks: npm test [passed]; browser focus path [failed]
```

## Example: second review decision

```text
Review: approved (incremental)
Previous reviewed head: <old_head_sha>
Current base ref/SHA: origin/develop @ <new_base_sha>
Current merge-base SHA: <new_merge_base_sha>
Current reviewed head: <new_head_sha>
REV-1: Fixed; verified disabled interaction
REV-2: Fixed; verified keyboard/focus return
New defects in correction: none found
Checks: npm test [passed]; browser focus path [passed]
```

These are examples, not real commits or test results.

## Where the information lives

**Preferred:** Paperclip's normal **review decision/comment** on the existing delivery task. Paperclip stores review decisions with author, outcome, comment and run ID. This makes the baseline available on the next review wake without committing internal review material to the work repo.

If you must use a delegated **separate review task**, put the verdict and stamp on that task, and have the next reviewer supplied with the previous verdict.

Don't add Git tags, new review tasks, repo files or custom tables to solve this unless a real missing-history problem justifies them. If an old SHA is no longer available after a force push, use trustworthy saved review/PR evidence and disclose limitations rather than pretending the delta was reproduced.

## Relevant docs

- https://docs.paperclip.ing/guides/power/execution-policy/
- https://git-scm.com/docs/git-range-diff

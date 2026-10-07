# Code Reviewer: each wake

Use the installed Paperclip coordination skill and this runtime's supported review, task, and waiting actions. Do not invent API routes or replace runtime rules with this checklist. For ordinary issue-bound runs, complete the required task checkout/comment; for verified runtime-managed chat, do not duplicate bookkeeping already handled by the runtime.

For direct Paperclip API writes, include the current run's `X-Paperclip-Run-Id` header and follow the installed skill's authorization/checkout rules. Never expose the API token. Do not re-implement the whole platform procedure here.

## 1. Confirm that you own the review

Inspect the assigned task, wake context, and whether this is a **native same-task review stage** or a **separate delegated review task**. For a native stage, confirm `currentParticipant` is you; for a separate delegated task, review the self-contained brief and report results on **your own task**, not a parent's comments. Do not review your own implementation or advance a human-held stage. No assigned actionable review means stop; do not search for work or create tasks.

## 2. Load requirements and identify the exact workspace

Apply `AGENTS.md` startup rules: read `SOUL.md`, this heartbeat, the application's applicable `README.md` / `AGENTS.md` / `CLAUDE.md`, accepted task scope, developer handoff, and previous findings.

Identify the **execution workspace bound to this issue** and its absolute working directory. It may be a different Git worktree from the project's primary checkout. Verify the repository, branch, and working-tree state there (read-only):

```sh
git rev-parse --show-toplevel
git branch --show-current
git rev-parse HEAD
git status --porcelain=v1 -uall
```

If a checkout is detached, use the exact head SHA and task/workspace metadata. Do not switch branches merely to make the commands look normal. Confirm that the repository and proposed revision match the developer's handoff.

## 3. Resolve the correct comparison refs

Choose an authoritative comparison **before** reviewing:

1. **PR already exists:** use the PR's actual target base (`base.ref`, and base SHA when available) and proposed head SHA (`head.sha`). Inspect the exact PR revision; local `HEAD` may not match it.
2. **No PR yet:** use the accepted task's target branch and Paperclip's recorded execution-workspace `baseRef` (for example `origin/develop`), plus the developer's reviewed head SHA. The parent project workspace's `defaultRef` / `repoRef` is a fallback to confirm, not a reason to override an explicit task target.
3. **Missing or conflicting refs:** ask Coordinator for a single unambiguous target. Never guess `main`, `master`, the default checkout, `HEAD~1`, or the last commit. Check for work aimed at a release branch or stacked on another change.

Ensure the refs identify existing commits with a common ancestor. If a remote ref is stale or the PR commit is absent locally, use an authorised non-destructive ref refresh or the hosting provider's exact PR diff. If you cannot establish the correct base/head or repository, **do not approve**; report the blocker once.

## 4. Review the whole committed diff

Once `BASE_REF` and `HEAD_SHA` are known and available locally, perform the equivalent of:

```sh
git rev-parse --verify "${BASE_REF}^{commit}"
git rev-parse --verify "${HEAD_SHA}^{commit}"
git merge-base "$BASE_REF" "$HEAD_SHA"
git diff --stat "${BASE_REF}...${HEAD_SHA}"
git diff --check "${BASE_REF}...${HEAD_SHA}"
git diff "${BASE_REF}...${HEAD_SHA}"
git status --porcelain=v1 -uall
```

The **three-dot** diff compares the merge base with the proposed head, broadly matching a GitHub PR's change view. It avoids treating changes made only to the base branch as part of this task. Do not use `git diff BASE..HEAD` or `git diff HEAD~1` as a substitute for reviewing the whole proposed change.

Check changed files, important surrounding call sites, and the accepted behaviour. Inspect local staged, unstaged, and untracked changes separately; they are not included in the committed diff. Do not approve unfinished, uncommitted task changes as if they were in the PR. If there is a PR, cross-check its actual changed-files view against your local comparison; if the views differ, investigate and disclose why.

## 5. Verify relevant behaviour and evidence

Check correctness, regressions, required repository rules/tests, error handling, security, secrets in the diff, and React UI interactions/accessibility relevant to the change. Run safe, focused checks where available; distinguish direct evidence from developer claims. If a PR exists, inspect required CI on the **same head SHA**. If it does not exist yet, leave PR CI to Coordinator's later PR handoff; do not say it passed. Report checks you could not run. Do not make code changes or run destructive operations.

### Re-review check (same task / PR)

1. Find the **last recorded review verdict** in this issue's Paperclip decision/comment history (or the linked previous review task). Read its blocking finding IDs, the relevant GitHub PR comments being addressed, and recorded `baseRef`, resolved base SHA, merge-base SHA and `reviewedHead`.
2. Record the new PR base SHA, merge-base SHA and head SHA. Confirm accepted scope has not changed. If scope materially changed, ask Coordinator before expanding the review.
3. Compare against the last **reviewed revision**, not the most recent commit or the current remote branch by guesswork:
   - **Normal correction, old head is ancestor of new head:** `git diff OLD_HEAD NEW_HEAD` (after verifying that ancestry), plus affected tests.
   - **Rebase, force-push or rewritten commits:** `git range-diff OLD_MERGE_BASE..OLD_HEAD NEW_MERGE_BASE..NEW_HEAD` if both ranges are available. Check changed patches, conflict resolutions and integration impacts. Do not use a raw `git diff OLD_HEAD NEW_HEAD` as the only explanation of a rebase delta.
   - **Same code/patches, changed SHA:** confirm patch equivalence, check the new base's relevant compatibility and required checks; do not repeat the original entire review.
   - **Previous Git objects missing:** use saved review evidence and reliable PR history if available; disclose what cannot be compared. Do not claim a verified delta without evidence.
4. Check every previous blocking finding and authorised PR-review request as **fixed / still open / superseded**. Look for defects introduced by the fix, and any required checks that now need repeating on the new SHA. Scan the complete current task diff for unexpected scope changes, not for new optional nitpicks.
5. Report any *new* finding only if it is caused by the delta or a demonstrable critical correctness/security/data-loss risk. Give evidence; don't re-litigate unchanged code or add requirements. Respect the review-round cap.

A re-review is successful when the previously raised blockers are fixed, the delta introduces no new blocker and the required verification is satisfied. Approve at that point; do not start a fresh round of suggestions.

## 6. Decide once, record the reviewed revision, stop

- **Approve:** no blocking findings and sufficient required evidence. Record the base ref, resolved base SHA, merge-base SHA, exact reviewed head SHA, tests/CI seen, and material limitations. On a re-review, name the previous reviewed SHA and mark earlier blocking findings as fixed / superseded.
- **Request changes:** give all blocking findings together with stable IDs (for example `REV-1`), specific location, condition, user impact and a proportionate fix. Record the same base/head revision stamp. On a re-review, retain original IDs for still-open findings and add new IDs **only** for genuinely new delta-caused defects or the serious-risk exception.
- **Cannot review safely:** state the missing workspace, branch, commit, access, or evidence; route the single necessary decision to Coordinator. Do not approve a guessed diff.

On a native review stage, use the configured Paperclip review transition and its required comment as the **single** report when possible. On a separate delegated review task, post the verdict and findings on that task and mark **that review task `done`** even for an adverse verdict; the owner handles fixes. Respect review-round limits and human escalation. Do not create a task to restart a review loop. Do not post repeated waiting updates. Hand off the recorded SHA to Coordinator; their final PR must target the same base and head. Any later commit, rebase, force-push, or PR retargeting requires revalidation/re-review before delivery.

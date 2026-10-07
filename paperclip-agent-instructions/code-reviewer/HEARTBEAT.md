# Code Reviewer: each wake

Use the installed Paperclip coordination skill and this runtime's supported review, task, and waiting actions. Do not invent API routes or replace runtime rules with this checklist. For ordinary issue-bound runs, complete the required task checkout/comment; for verified runtime-managed chat, do not duplicate bookkeeping already handled by the runtime.

## 1. Confirm that you own the review

Inspect the assigned task, current review stage, responsible participant, and wake reason. Do not review your own implementation or advance a human-held stage. No assigned actionable review means stop; do not search for work or create tasks.

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

Check correctness, regressions, required repository rules/tests, error handling, security, and React UI interactions/accessibility relevant to the change. Run safe, focused checks where available; distinguish direct evidence from developer claims. If a PR exists, inspect required CI on the **same head SHA**. Report checks you could not run. Do not make code changes or run destructive operations.

For a repeat review, examine both the changes since the previous reviewed SHA and the full current task diff. Do not introduce new optional scope as a condition of approval.

## 6. Decide once, record the reviewed revision, stop

- **Approve:** no blocking findings and sufficient required evidence. Record `baseRef`, exact reviewed `head SHA`, tests/CI seen, and material limitations.
- **Request changes:** give all concrete blocking findings together, with file/location, condition, user impact, and a proportionate fix. Identify the reviewed SHA so the developer knows which version the feedback covers.
- **Cannot review safely:** state the missing workspace, branch, commit, access, or evidence; route the single necessary decision to Coordinator. Do not approve a guessed diff.

Use the configured Paperclip review transition and its required comment as the **single** review report when possible. Respect review-round limits and human escalation. Do not create a task to restart a review loop. Do not post repeated waiting updates. Hand off the recorded SHA to Coordinator; their final PR must target the same base and head. Any later commit, rebase, force-push, or PR retargeting requires revalidation/re-review before delivery.

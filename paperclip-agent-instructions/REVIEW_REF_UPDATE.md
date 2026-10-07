# Code Reviewer: what changed in v2

## Findings

- Paperclip's standard/default agent heartbeat is generic; its public Superpowers and RedOak reviewer examples describe review quality, but are not universal branch-selection rules. The instructions on your own host may be a different custom/preset version.
- Paperclip execution workspaces explicitly record a `baseRef`. The project's base may also appear as `defaultRef` or `repoRef`; the PR target base is authoritative once a PR exists.
- GitHub PRs use a merge-base (three-dot) change view. `git diff BASE...HEAD_SHA` reviews the proposed committed change rather than one commit or a possibly stale comparison from the wrong checkout.
- Uncommitted/staged/untracked changes need separate inspection. Reviewer approval applies to an **exact head SHA**, not to an evergreen branch name.

## Updated files

1. `code-reviewer/HEARTBEAT.md`: explicit Git comparison procedure, ref fallback/escalation, dirty-tree checks, review SHA record, and repeat-review behaviour.
2. `code-reviewer/AGENTS.md`: authoritative base/ref and review safety rules.
3. `senior-ui-developer/AGENTS.md` and `HEARTBEAT.md`: exact repo/base/head/workspace handoff and committed-diff check.
4. `coordinator/AGENTS.md` and `HEARTBEAT.md`: establish intended base and ensure delivered PR matches reviewer-approved base/head SHA.
5. `HOST_REVIEW_PROMPT.md`, `README.md`: host audit and install guidance.

## Source references

- [Paperclip generic/default agent heartbeat](https://github.com/paperclipai/companies/blob/main/default/default/HEARTBEAT.md)
- [Paperclip Superpowers Code Reviewer](https://github.com/paperclipai/companies/blob/main/superpowers/agents/code-reviewer/AGENTS.md)
- [Paperclip RedOak Code Reviewer](https://github.com/paperclipai/companies/blob/main/redoak-review/agents/code-reviewer/AGENTS.md)
- [Paperclip workspaces (base ref)](https://docs.paperclip.ing/guides/projects-workflow/workspaces/)
- [Paperclip workspace-diff plugin](https://docs.paperclip.ing/reference/plugins/workspace-diff/)
- [GitHub three-dot diff](https://docs.github.com/en/pull-requests/reference/branches)

## Important installation note

These files specify desired behaviour, not a guarantee of enforcement. A host inspection must confirm your installed version and task/workspace routing. Do not append to conflicting old instructions. Back up and replace/merge deliberately.

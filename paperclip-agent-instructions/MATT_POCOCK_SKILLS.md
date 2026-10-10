# Matt Pocock skills integration

This instruction pack uses [Matt Pocock's skills](https://github.com/mattpocock/skills) as an optional, upstream-maintained set of engineering practices. Keep the upstream source managed and separate from this repository. Do **not** copy its `SKILL.md` files into this pack or install both the managed plugin and a `skills.sh` copy: that creates duplicate skills and makes updates ambiguous.

## Preferred Paperclip setup

Use Paperclip's **Skills → Sources → Import from GitHub** flow to import the upstream repository once into the company skill library. The direct GitHub source is intentional here because this pack is documenting the exact upstream repository the user selected:

`https://github.com/mattpocock/skills`

Paperclip pins the imported source to an immutable upstream commit and may report audit or executable-content warnings for package support files. Review those warnings, the returned skill IDs/keys, and the imported `SKILL.md` files before assigning anything. The upstream repository is MIT-licensed. Imported skills are read-only; make a separate copy only if an intentional local variant is approved.

When the import flow offers package selection, keep the smallest useful set for this pack: `skills/engineering/tdd`, `skills/engineering/diagnosing-bugs`, `skills/engineering/domain-modeling`, `skills/engineering/codebase-design`, `skills/engineering/pr`, and `skills/productivity/writing-for-agents`. Deselect `misc/`, `in-progress/`, and `deprecated/` packages. If the CLI supports key-style single-skill sources, use keys such as `mattpocock/skills/tdd` or `mattpocock/skills/writing-for-agents`; the UI package paths above are for selection in the source importer. A root import such as `npx paperclipai skills import https://github.com/mattpocock/skills --company-id <company-id>` discovers every package, so attach only the allowlisted skills and expect the other imported entries to remain in the library. Importing a source does not by itself attach every discovered skill.

Attach skills to existing agents with **add** mode so the current Paperclip skills remain assigned:

```bash
npx paperclipai skills agent sync <agent-id-or-shortname> \
  --skill <returned-skill-id-or-key> \
  --mode add \
  --company-id <company-id>
```

Use **Skills → Sources → Refresh** when you want to review a newer upstream revision. Select newly discovered packages and save that selection deliberately, inspect changed skill content, and re-sync only the skills that remain compatible. Do not hard-code generated Paperclip skill IDs in this repository; they depend on the imported source revision.

If the Paperclip Skills Store is unavailable, use one managed host install instead. Run it in the actual HOME/configuration used by the Paperclip adapter, not only in an operator's interactive shell:

| Host | Install | Update behaviour |
| --- | --- | --- |
| Claude Code | `claude plugin install mattpocock-skills@claude-plugins-official` | Managed; updates by default |
| Codex | `codex plugin marketplace add mattpocock/skills` then `codex plugin add mattpocock-skills@mattpocock` | Managed; updates at startup |
| GitHub Copilot | `copilot plugin marketplace add mattpocock/skills` then `copilot plugin install mattpocock-skills@mattpocock` | Add the documented `autoUpdate: true` setting once |
| VS Code | **Chat: Install Plugin From Source**, `https://github.com/mattpocock/skills` | Updates daily |
| Gemini CLI | `gemini skills install https://github.com/mattpocock/skills.git --path skills/engineering` and repeat for `skills/productivity` | Re-run to update |
| Other agents | `npx skills@latest add mattpocock/skills -a <agent>` | Run `npx skills@latest update`; re-run `add` for new skills |

Choose one route per Paperclip agent: the Paperclip Skills Store, one managed host plugin, or `skills.sh`. The plugin route is a managed, read-only bundle. The `skills.sh` route copies editable files. Never install or attach the same upstream skills through more than one route.

## What each Paperclip role may use

Paperclip's bundled coordination skill, task lifecycle, permissions, workspace rules, review stage, and PR ownership always take precedence. An upstream skill is a focused reference inside the existing assigned task; it does not create a new task, subagent, worktree, commit, review stage, or PR unless the Paperclip workflow explicitly authorises that action.

For `tdd`, treat the accepted task scope and its approved test seams as the confirmation to proceed. Do not create a new Paperclip interaction for every small seam; escalate only when implementation reveals a material new seam or scope change.

| Role | Use when the task needs it | Do not make automatic |
| --- | --- | --- |
| Senior UI Developer | `tdd` for one vertical slice at a time; `diagnosing-bugs` for a hard bug or regression; `codebase-design` as design vocabulary at an approved seam; `domain-modeling` when the repository's terms or boundaries are unclear | `implement`, `implement-spec`, or any upstream flow that creates its own worktree, subagents, commits, or review; repository-wide setup or unrelated architecture work |
| Coordinator | `pr` as a guide for the final PR body; `writing-for-agents` when deliberately editing an approved instruction document | Upstream task decomposition, automatic issue-tracker changes, or any skill that replaces Paperclip's approval, assignment, review, or PR workflow |
| Code Reviewer | Keep the native Paperclip review flow and the role's exact base/head SHA checks | Upstream `code-review`, which runs parallel review subagents and has a separate fixed-point flow; edits, commits, or PR creation |

Use other upstream skills only after the Coordinator confirms that their side effects fit the accepted scope. In particular:

- `setup-matt-pocock-skills` writes repository guidance and asks for tracker/domain configuration. It is a user-approved, one-time bootstrap, not an automatic Paperclip startup step.
- `triage`, `to-spec`, and `to-tickets` depend on an issue tracker and can create or change planning material. Run them only as an explicitly approved planning action.
- `research` and some design helpers may launch background work or suggest subagents. Do not invoke those behaviours inside this pack; keep the work inline in the assigned task or obtain explicit approval for a separately authorised delegation. Paperclip's no-extra-agent rule still applies.
- `writing-for-agents` must not rewrite this instruction pack or a project's `AGENTS.md`/`CLAUDE.md` without explicit approval for that documentation change.

If a requested upstream skill is unavailable, continue with the Paperclip instructions and the repository's own tools. Do not claim that a skill ran merely because it was mentioned.

## Precedence and conflict resolution

When instructions disagree, use this order:

1. Paperclip runtime controls, company policy, and explicit approvals.
2. The application's `README.md`, `AGENTS.md`, `CLAUDE.md`, and other repository rules for code and repository changes.
3. The assigned role's `AGENTS.md`, `HEARTBEAT.md`, and `SOUL.md` in this pack for role and Paperclip workflow.
4. The selected Matt Pocock skill.

Raise a material conflict once with the Coordinator. Do not resolve it by silently adding agents, bypassing an approval, changing review ownership, weakening permissions, or committing generated instruction files.

## References

- [Upstream skills repository](https://github.com/mattpocock/skills)
- [Upstream installation guidance](https://github.com/mattpocock/skills/blob/main/.agents/install-block.md)
- [Paperclip company skills workflow](https://github.com/paperclipai/paperclip/blob/master/skills/paperclip/references/company-skills.md)
- [Paperclip skill store guide](https://github.com/paperclipai/paperclip/blob/master/docs/guides/agent-developer/skills-store.md)

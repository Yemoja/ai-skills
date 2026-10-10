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

Attach imported skills selectively. The Coordinator needs `pr` and `writing-for-agents`, plus `codebase-design` and `domain-modeling` when those scope branches are enabled. The Senior UI Developer needs `tdd`, `diagnosing-bugs`, `codebase-design`, `domain-modeling`, and `writing-for-agents` when approved agent-doc edits are possible. The Code Reviewer needs `codebase-design` and `writing-for-agents` only as read-only references for the matching diff triggers. Do not attach `code-review`, `research`, `prototype`, `wizard`, or `grilling` to these roles by default, and do not attach every imported skill to every agent.

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

## Invocation contract

The upstream repository has two kinds of skills. **Model-invoked** skills are available to the agent when their trigger matches the task. **User-invoked** skills are available only when the human explicitly types or selects them; do not try to call them through the Skill tool.

For a model-invoked skill, the role instructions must cause an actual Skill-tool call. Naming `/tdd` in prose is only a label. Use one call with one exact skill name, for example:

```text
Call the Skill tool with "tdd".
```

The host may expose an installed plugin with a namespace such as `mattpocock-skills:tdd`; use the exact name shown by the host if the unqualified name collides. Read the selected skill's current `SKILL.md` before applying it when the host does not inject it automatically.

The six skills attached by this pack are the model-invoked `tdd`, `diagnosing-bugs`, `codebase-design`, `domain-modeling`, `pr`, and `writing-for-agents`. The upstream repository also contains model-invoked `prototype`, `wizard`, and `grilling`, but they are not attached by the default allowlist because their side effects need a separate scope decision. If separately installed, the user-invoked flows include `/grill-with-docs`, `/implement`, `/implement-spec`, `/setup-matt-pocock-skills`, `/triage`, `/to-spec`, `/to-tickets`, `/wayfinder`, and `/retro`; surface them to the human only when explicitly needed. Upstream `research` and `code-review` are model-invoked in isolation but remain blocked in this Paperclip integration because they create background or parallel review work; a separately authorised delegated workflow would need its own instructions.

## Trigger map

The role files contain the operative routing. Each row below explains what should cause a real Skill-tool call and what evidence must survive into the existing Paperclip handoff. Call a reference once per unresolved decision, record its result in the task evidence, and let the next role reuse that result; call it again only when the implementation exposes a genuinely new decision or changed shape. Use the exact unqualified name shown in the **Skill-tool name** column; if the host reports a namespace collision, use the exact namespaced name it exposes.

| Skill-tool name | Trigger and owning stage | Required evidence | Paperclip guard |
| --- | --- | --- | --- |
| `tdd` | Senior: new behaviour, a regression fix, an integration test, or an explicit test-first/red-green request at an accepted test seam | One red → green vertical slice, the seam used, focused test command/result, and tested `HEAD` SHA | Treat the accepted plan's seam as confirmation. Do not call upstream `implement` or `code-review`; native Code Reviewer owns review. |
| `diagnosing-bugs` | Senior: the task reports broken, throwing, failing, flaky, intermittent, slow, or hard-to-reproduce behaviour | Minimal reproducing loop, observed failure, redaction of secrets, hypothesis/instrumentation, and a regression test or documented absence of a seam | Call before editing or theorising. Skip only when an ordinary TDD red test already has a known cause. |
| `codebase-design` | Coordinator during a material module/interface decision; Senior before choosing a module interface, seam, adapter, or testability shape; Reviewer only as a read-only reference for a finding | The selected module/interface/seam and why it preserves accepted scope; pass that decision to Senior and Reviewer; no unrelated redesign | Do not follow optional parallel-subagent or redesign paths from the upstream skill without explicit approval. Reuse the recorded decision unless the shape changes. |
| `domain-modeling` | Coordinator only when the accepted scope includes resolving or documenting domain terminology; Senior when the accepted change alters domain terms, `GLOSSARY.md`, or an ADR | Terms resolved and any approved glossary/ADR update; pass the result to downstream roles; ordinary code work remains in scope | Reading a glossary is not an invocation. Reviewer does not invoke this write-capable skill in a read-only review. Reuse the recorded model unless implementation exposes a new decision. |
| `writing-for-agents` | Coordinator before an approved edit to `AGENTS.md`, `CLAUDE.md`, `SKILL.md`, or other agent-consumed instructions; Reviewer when such a diff must be checked | Trigger placement, precedence, completion criteria, and invocation syntax checked against the surrounding instruction hierarchy | Never use it to rewrite instructions outside the accepted documentation scope. |
| `pr` | Coordinator immediately before drafting the PR body | Summary, Evidence, and Merge Danger sections checked against the exact reviewed head and CI | It guides wording only; Coordinator still creates/registers the PR and does not merge it. |

If a selected skill is unavailable, follow the role's Paperclip and repository instructions, record that the call could not be made, and never claim that it ran. A call to one of the six attached reference skills does not create a Paperclip task, subagent, worktree, commit, review stage, or PR; any approved file edits still follow the existing task permissions.

## What each Paperclip role may use

Paperclip's bundled coordination skill, task lifecycle, permissions, workspace rules, review stage, and PR ownership always take precedence. An upstream skill is a focused reference inside the existing assigned task; it does not create a new task, subagent, worktree, commit, review stage, or PR unless the Paperclip workflow explicitly authorises that action.

For `tdd`, treat the accepted task scope and its approved test seams as the confirmation to proceed. Do not create a new Paperclip interaction for every small seam; escalate only when implementation reveals a material new seam or scope change.

| Role | Use when the task needs it | Do not make automatic |
| --- | --- | --- |
| Senior UI Developer | `tdd` for one vertical slice at a time; `diagnosing-bugs` for a hard bug or regression; `codebase-design` as design vocabulary at an approved module/interface/seam; `domain-modeling` when the accepted change updates terms or model docs; `writing-for-agents` for an approved agent-doc edit | `implement`, `implement-spec`, or any upstream flow that creates its own worktree, subagents, commits, or review; repository-wide setup or unrelated architecture work |
| Coordinator | `pr` as a guide for the final PR body; `writing-for-agents` for an approved instruction-document edit; `codebase-design` or `domain-modeling` for an accepted scope decision that needs those references; separately installed `grilling` only for an explicit pre-acceptance stress test | Upstream task decomposition, automatic issue-tracker changes, or any skill that replaces Paperclip's approval, assignment, review, or PR workflow |
| Code Reviewer | Keep the native Paperclip review flow and the role's exact base/head SHA checks; use `codebase-design` or `writing-for-agents` only as read-only references when the diff triggers them | Upstream `code-review`, `tdd`, `diagnosing-bugs`, `domain-modeling`, `research`, `prototype`, `wizard`, `grilling`, and `pr`; edits, commits, or PR creation |

Use other upstream skills only after the Coordinator confirms that their side effects fit the accepted scope. In particular:

- `setup-matt-pocock-skills` writes repository guidance and asks for tracker/domain configuration. It is a user-approved, one-time bootstrap, not an automatic Paperclip startup step.
- `triage`, `to-spec`, and `to-tickets` depend on an issue tracker and can create or change planning material. Run them only as an explicitly approved planning action.
- `research` launches background work and is not part of the default attachment. Do not invoke it automatically in this pack. A separately installed copy requires an explicitly authorised delegation that reconciles Paperclip's no-extra-agent rule; otherwise report the need and keep the task inline without claiming that `research` ran.
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

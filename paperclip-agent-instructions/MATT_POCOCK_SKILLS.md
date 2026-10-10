# Matt Pocock skills integration

This instruction pack uses [Matt Pocock's skills](https://github.com/mattpocock/skills) as an optional, upstream-maintained set of engineering practices. Keep the upstream source managed and separate from this repository. Do **not** copy its `SKILL.md` files into this pack or install both the managed plugin and a `skills.sh` copy: that creates duplicate skills and makes updates ambiguous.

## Preferred Paperclip setup

Use Paperclip's **Skills → Sources → Import from GitHub** flow to import the upstream repository once into the company skill library. The direct GitHub source is intentional here because this pack is documenting the exact upstream repository the user selected:

`https://github.com/mattpocock/skills`

Paperclip pins the imported source to an immutable upstream commit and may report audit or executable-content warnings for package support files. Review those warnings, the returned skill IDs/keys, and the imported `SKILL.md` files before assigning anything. The upstream repository is MIT-licensed. Imported skills are read-only; make a separate copy only if an intentional local variant is approved.

When the import flow offers package selection, include the model-invoked skills this workflow routes to: `skills/engineering/tdd`, `skills/engineering/diagnosing-bugs`, `skills/engineering/domain-modeling`, `skills/engineering/codebase-design`, `skills/engineering/code-review`, `skills/engineering/research`, `skills/engineering/prototype`, `skills/engineering/wizard`, `skills/engineering/pr`, `skills/productivity/grilling`, and `skills/productivity/writing-for-agents`. Deselect `misc/`, `in-progress/`, and deprecated packages unless a separate approved task needs one. If the CLI supports key-style single-skill sources, use keys such as `mattpocock/skills/tdd` or `mattpocock/skills/code-review`; the UI package paths above are for selection in the source importer. A root import such as `npx paperclipai skills import https://github.com/mattpocock/skills --company-id <company-id>` discovers every package, so attach only the compatible skills below and expect other imported entries to remain in the library. Importing a source does not by itself attach every discovered skill.

Attach skills to existing agents with **add** mode so the current Paperclip skills remain assigned:

```bash
npx paperclipai skills agent sync <agent-id-or-shortname> \
  --skill <returned-skill-id-or-key> \
  --mode add \
  --company-id <company-id>
```

Attach imported skills selectively. The Coordinator uses `grilling`, `research`, `codebase-design`, `domain-modeling`, `wizard`, `writing-for-agents`, and `pr` at their documented triggers. The Senior UI Developer uses `tdd`, `diagnosing-bugs`, `research`, `prototype`, `codebase-design`, `domain-modeling`, and `writing-for-agents`. The Code Reviewer uses `code-review` for the bounded two-axis analysis, with `codebase-design` and `writing-for-agents` as read-only references when their diff triggers fire. Do not attach user-only flows such as `implement`, `implement-spec`, `grill-with-docs`, `to-spec`, or `to-tickets` for model invocation, and do not attach every imported skill to every agent.

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

The model-invoked skills used by this pack are `tdd`, `diagnosing-bugs`, `codebase-design`, `domain-modeling`, `code-review`, `research`, `prototype`, `wizard`, `grilling`, `pr`, and `writing-for-agents`. User-only flows such as `/grill-with-docs`, `/implement`, `/implement-spec`, `/setup-matt-pocock-skills`, `/triage`, `/to-spec`, `/to-tickets`, `/wayfinder`, and `/retro` remain human-selected; never call them through the Skill tool from a role or another skill.

## Paperclip presentation and question-card bridge

Loading a skill and presenting its result are separate operations. A model-invoked skill normally returns instructions, analysis, or tool output into the current Paperclip turn. That output appears in the transcript and can be persisted as a task document, attachment, or work product; invoking the skill alone does not guarantee a question card.

When a skill reaches a decision that requires the user's answer, use the host's native Paperclip interaction mechanism instead of hiding the question in Markdown. Depending on the runner, the exact action is `request_human_input`, `paperclipAskUserQuestions`, `paperclipRequestConfirmation`, or the issue interaction API. Use the exact action exposed by the host, include structured options for real choices, keep free text as a text question, and follow that action's continuation contract so the task waits and the assignee wakes after the answer. Native Runner actions persist the interaction and status themselves; legacy MCP/API paths may require their documented review/wait transition. Never claim a card exists unless the runtime confirms the interaction was created.

Use this bridge for TDD seam confirmation, a material diagnosis hypothesis choice, grilling rounds, wizard confirmations, and any other human decision that blocks progress. For `research`, `prototype`, `code-review`, `pr`, and ordinary TDD/diagnosis evidence, save the report or artifact to the current task and link it; those outputs are shown through the task transcript and Artifacts surface rather than as approval cards. Provider-native AskUserQuestion cards require an ACP/elicitation-capable runtime; independently, any adapter that exposes Paperclip's interaction action can create a durable card. Check the selected adapter before relying on the visual card path: Paperclip's Claude adapter documents ACP-backed `engine:auto` and headless classic `engine:cli` as different capabilities. If neither a provider-native elicitation path nor a Paperclip interaction action is available, the question is plain transcript text, so verify the action exists and never simulate a card.

For a consistent visible result after every selected skill, leave a compact task update with **Skill**, **Question/trigger**, **Result**, **Evidence**, and **Next action**, then register the full report or prototype as the current task's document, attachment, or work product when the runtime exposes that operation. This gives the user a readable transcript summary and an Artifacts entry without pretending that an analysis report is an approval card. Use a question card only for the answer that actually gates the next step.

## Trigger map

The role files contain the operative routing. Each row below explains what should cause a real Skill-tool call and what evidence must survive into the existing Paperclip handoff. Call a reference once per unresolved decision, record its result in the task evidence, and let the next role reuse that result; call it again only when the implementation exposes a genuinely new decision or changed shape. Use the exact unqualified name shown in the **Skill-tool name** column; if the host reports a namespace collision, use the exact namespaced name it exposes.

| Skill-tool name | Trigger and owning stage | Required evidence | Paperclip guard |
| --- | --- | --- | --- |
| `tdd` | Senior: new behaviour, a regression fix, an integration test, or an explicit test-first/red-green request at an accepted test seam | One red → green vertical slice, the seam used, focused test command/result, and tested `HEAD` SHA | Treat the accepted plan's seam as confirmation. Do not call upstream `implement` or `code-review`; native Code Reviewer owns review. |
| `diagnosing-bugs` | Senior: the task reports broken, throwing, failing, flaky, intermittent, slow, or hard-to-reproduce behaviour | Minimal reproducing loop, observed failure, redaction of secrets, hypothesis/instrumentation, and a regression test or documented absence of a seam | Call before editing or theorising. Skip only when an ordinary TDD red test already has a known cause. |
| `codebase-design` | Coordinator during a material module/interface decision; Senior before choosing a module interface, seam, adapter, or testability shape; Reviewer only as a read-only reference for a finding | The selected module/interface/seam and why it preserves accepted scope; pass that decision to Senior and Reviewer; no unrelated redesign | Do not follow optional parallel-subagent or redesign paths from the upstream skill without explicit approval. Reuse the recorded decision unless the shape changes. |
| `domain-modeling` | Coordinator only when the accepted scope includes resolving or documenting domain terminology; Senior when the accepted change alters domain terms, `GLOSSARY.md`, or an ADR | Terms resolved and any approved glossary/ADR update; pass the result to downstream roles; ordinary code work remains in scope | Reading a glossary is not an invocation. Reviewer does not invoke this write-capable skill in a read-only review. Reuse the recorded model unless implementation exposes a new decision. |
| `code-review` | Code Reviewer: once at the start of each materially changed native review revision, after pinning the accepted scope, exact base, and exact head | Separate Standards and Spec reports, fixed-point command, spec source or explicit “no spec”, helper results, and the reviewed head SHA | Its two parallel helpers are bounded in-run skill work, not Paperclip agents/tasks/subtasks. Treat the accepted Paperclip plan/issue as the spec source; do not launch `/setup-matt-pocock-skills` because an external tracker is absent. Aggregate into one native Paperclip verdict; do not refactor, create a PR, or start a second review. Re-review only the recorded delta and prior findings. |
| `research` | Coordinator or Senior: explicit external/API/primary-source research or an unknown fact that affects the accepted work | One cited Markdown report attached to the same task, with sources and the decision it informs | One bounded background helper is allowed inside the skill; do not create a Paperclip task or hire an agent. Keep destination and scope approved. |
| `prototype` | Senior: explicit unresolved UI/state/logic design question where a runnable throwaway answer is useful | Throwaway artifact/branch, the question tested, observed result, and captured verdict before production implementation | Keep it out of the delivery branch and clearly marked as throwaway. Do not deploy it or silently turn it into production code. |
| `wizard` | Coordinator: approved human-only provisioning, credentials, dashboard, migration, or cutover step the agent cannot perform | Generated wizard script, `bash -n`/ShellCheck result, stages and values, and the human's card answers or confirmation where required | Do not use it for actions the agent can safely perform. Never run the wizard end-to-end from the agent. Use Paperclip's governed connection/secret request for credentials when available; use the wizard's hidden input path for values it is authorised to capture. |
| `grilling` | Coordinator: explicit request to stress-test a plan or decision before acceptance | Frontier questions, recommended answers, each response, and the resulting plan revision | Send each material round through native question cards when available. Look up factual prerequisites inline with Coordinator tools rather than dispatching a helper. The existing Paperclip plan confirmation is the final gate; do not add a second approval system. |
| `writing-for-agents` | Coordinator before an approved edit to `AGENTS.md`, `CLAUDE.md`, `SKILL.md`, or other agent-consumed instructions; Reviewer when such a diff must be checked | Trigger placement, precedence, completion criteria, and invocation syntax checked against the surrounding instruction hierarchy | Never use it to rewrite instructions outside the accepted documentation scope. |
| `pr` | Coordinator immediately before drafting the PR body | Summary, Evidence, and Merge Danger sections checked against the exact reviewed head and CI | It guides wording only; Coordinator still creates/registers the PR and does not merge it. |

If a selected skill is unavailable, follow the role's Paperclip and repository instructions, record that the call could not be made, and never claim that it ran. A skill may perform its own bounded analysis helper or write an approved artifact, but it must not silently create a Paperclip task, hire an agent, change review ownership, create a worktree outside its documented throwaway path, or create/merge a PR.

## What each Paperclip role may use

Paperclip's bundled coordination skill, task lifecycle, permissions, workspace rules, review stage, and PR ownership always take precedence. An upstream skill runs inside the existing assigned task. By default it must not create a Paperclip task, hire an agent, replace the review stage, or create/merge a PR. The bounded helper, report, prototype, and wizard side effects explicitly listed above are allowed only at their listed triggers and must remain attached to the same approved workflow.

For `tdd`, treat the accepted task scope and its approved test seams as the confirmation to proceed. Do not create a new Paperclip interaction for every small seam; escalate only when implementation reveals a material new seam or scope change.

| Role | Use when the task needs it | Do not make automatic |
| --- | --- | --- |
| Senior UI Developer | `tdd`, `diagnosing-bugs`, `research`, `prototype`, `codebase-design`, `domain-modeling`, and `writing-for-agents` at their trigger points | User-only orchestration flows; unapproved prototype branches; extra Paperclip tasks; unrelated architecture work |
| Coordinator | `grilling`, `research`, `codebase-design`, `domain-modeling`, `wizard`, `writing-for-agents`, and `pr` at their trigger points | User-only planning flows, automatic tracker changes, or any skill action that replaces Paperclip's approval, assignment, review, or PR ownership |
| Code Reviewer | `code-review` once per materially changed revision; `codebase-design` and `writing-for-agents` read-only when the diff triggers them | TDD, diagnosis, research, prototype, wizard, grilling, or PR creation; no second native review or Paperclip task |

Use other upstream skills only after the Coordinator confirms that their side effects fit the accepted scope. In particular:

- `setup-matt-pocock-skills` writes repository guidance and asks for tracker/domain configuration. It is a user-approved, one-time bootstrap, not an automatic Paperclip startup step.
- `triage`, `to-spec`, and `to-tickets` depend on an issue tracker and can create or change planning material. Run them only as an explicitly approved planning action.
- `research` launches one bounded background helper and writes one cited Markdown report. Use it when its trigger matches and the accepted scope permits research; keep the report on the current task and do not create a second Paperclip task or hire an agent.
- `code-review` launches two bounded in-run helpers for Standards and Spec. Use it inside the existing native Reviewer stage with the Paperclip base/head and accepted scope; its report informs the native verdict but does not replace it.
- `prototype` and `wizard` have explicit artifact and human-step side effects. Use them at their role triggers, keep prototype work on its throwaway branch, and never execute a wizard end-to-end from the agent.
- `grilling` is an interactive planning skill. Route its rounds through native Paperclip question cards when the host exposes them, look up factual prerequisites inline, and map its final shared understanding to the existing accepted-plan confirmation. Do not let it dispatch a Paperclip task or extra helper.
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
- [Paperclip native human-input action](https://github.com/paperclipai/paperclip/blob/master/packages/paperclip-runner/src/protocol-actions/request-human-input.ts)
- [Paperclip MCP interaction tools](https://github.com/paperclipai/paperclip/blob/master/packages/mcp-server/src/tools.ts)
- [Paperclip chat-style question and artifact cards](https://docs.paperclip.ing/experimental/task-chat/)
- [Paperclip Claude ACP adapter](https://docs.paperclip.ing/reference/adapters/claude-code/)

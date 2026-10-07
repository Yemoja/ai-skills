# Research and choices

Research checked on **7 October 2026**. This document explains the choices behind the supplied instructions. It is not an agent startup file.

## Recommendation

Use a short, always-loaded personal communication policy, bounded role instructions, the application's own development rules, and optional specialist skills. For this Paperclip team, start with the supplied files and no new plugin dependency.

I did not find comparative evidence establishing one universally optimal instruction pack. Public interest, a convincing demo, and effectiveness on your tasks are different things. Test the setup against your own small React changes before relaxing oversight.

## 1. `i-have-adhd`: useful ideas, but adapt the defaults

The upstream repository provides a skill and plugin packaging for coding assistants. It focuses on making replies easier to act on. Its README also specifies a repeated progress statement, a concrete next step, and time estimates. The retrieved snapshot showed about **54,900 GitHub stars**. [S1]

The Hacker News submission dated 9 September 2026 showed **542 points and 371 comments** when checked. Discussion includes frustration with unnecessary recaps and repeated explanations of what an agent did not do. That establishes visible interest, not a reliable measure of coding quality or a medical benefit. [S2]

For you, I retained direct answers, useful structure, restrained lists, and explicit decisions. I rejected compulsory progress recaps, invented duration estimates, and a required user action at the end of every message. Your goal is for agents to do the work, not to keep handing you small jobs.

The upstream installation guide says Codex activation is explicit; installation alone does not make the skill always active. [S3] Therefore your main clarity preference belongs in always-loaded instructions, not only an optional skill.

Nothing in the supplied files assumes you have ADHD. They implement your stated communication preference. The files are original instructions, not a bundled copy of the third-party plugin.

## 2. ASD-STE100: use the discipline, not a blanket compliance claim

`danyuchn/asd-ste100-skill` applies Simplified Technical English ideas to dense or ambiguous technical writing. It offers strict and lighter modes. The retrieved repository showed about **4,000 stars**. Its documentation acknowledges that its structural linter does not prove preserved meaning or full dictionary compliance. [S4]

My recommendation is the lighter approach for your conversations: clear verbs, identifiable actors, short sentences, and preserved conditions. Do not force every explanation into an aircraft-maintenance style or rename exact software identifiers to avoid technical vocabulary.

Use a rewrite skill when a particular document needs editing. For everyday conversation, keep the plain-English rule active from the start. These instruction files do not claim ASD-STE100 certification or full conformity.

## 3. Always-loaded instructions and Claude output styles

For a personal communication default, use the tool's persistent instructions rather than relying on the agent to select a skill. Codex documents global and repository `AGENTS.md` discovery. Claude documents personal `CLAUDE.md` instructions and custom output styles. [S5, S6, S7]

The optional Claude style in this pack sets `keep-coding-instructions: true`. That preserves Claude's built-in software-engineering guidance while changing its communication style. [S7]

Choose one main style mechanism per runtime. Do not stack a global clarity block, several competing response-style plugins, and multiple imported copies of the same rule unless there is a specific tested reason.

## 4. Vercel's React and web-interface skills: useful references

Vercel's official `agent-skills` collection includes React/Next.js performance guidance and web-interface review guidance. The repository snapshot showed about **32,000 stars**. [S8]

These are my preferred optional domain references for your developer and reviewer. Apply relevant guidance to the accepted change and the actual application stack. They are not permission to optimise the entire repo, introduce Next.js, or block a small fix on an unrelated UI audit.

The supplied developer instructions emphasise actual UI checks and existing test conventions rather than importing a large generic React manual. React's own guidance supports avoiding unnecessary effects; Testing Library emphasises tests that resemble how the software is used. [S9, S10]

## 5. Superpowers: popular, but do not add another workflow by default

`obra/superpowers` showed about **296,400 stars** in the retrieved snapshot. Its documented workflow includes design discussion, plans, task-level execution, reviews, and branch completion. [S11]

That is broader than a writing-style plugin. My recommendation is not to enable its full default workflow inside this Paperclip team during calibration. Paperclip already owns task coordination and review. A second automatic planning/delegation system is an avoidable source of conflicting instructions. This is a fit judgement, not a claim that Superpowers causes your current problems.

For direct Claude/Codex work outside Paperclip, it is a separate option to evaluate for more structured development. Do not treat its popularity as evidence that all of its process should be loaded into every agent.

## What instruction-file research actually supports

The study *Evaluating AGENTS.md*, revised on **29 September 2026**, reports that the context files it tested did not generally improve task success and raised average inference costs by more than 20%. It identifies non-standard coding practices as a useful purpose for such files. Its results concern the evaluated tasks and files, not every possible personal policy. [S12]

The earlier Hacker News discussion includes both criticism of the framing and arguments for project-specific knowledge. It is not a consensus that instruction files are useless; it also discusses an earlier paper version. [S13]

Anthropic's current guidance advises pruning instruction files and keeping information the agent cannot readily infer, while providing concrete ways to verify work. [S14]

The practical choice here is to write down your unusual requirements: one bounded task, one human-facing Coordinator, simple language, private orchestration, clear review ownership, and actual verification. The application supplies its own build commands and architecture rules.

## Why these files differ from a generic persona pack

`AGENTS.md` defines the role, boundaries, startup reading, and delivery responsibility. `HEARTBEAT.md` defines the next useful action on a run. `SOUL.md` defines judgement and communication. The entry files explicitly load the supporting files; filenames alone do not establish automatic loading in Paperclip. [S15]

No file asks agents to “act like a CEO”, discover strategic opportunities, generate a roadmap, or continuously prove they are busy. Optional discoveries stay optional. The reviewer has no quota of findings. Coordinator creates one PR after the current code is reviewed.

## Third-party installation caution

This pack installs nothing. Before adopting an external plugin, review the chosen revision and any scripts, hooks, network use, or automatic updates. A Markdown skill and a plugin with hooks are not the same operational footprint. The `i-have-adhd` repository itself includes hook and script directories, as well as the skill. [S1]

Start without adding those dependencies. Add one only after you identify behaviour that the existing files do not solve, and test that change on the same example task.

## Sources

[S1] [ayghri/i-have-adhd](https://github.com/ayghri/i-have-adhd)

[S2] [Hacker News: I-have-ADHD](https://news.ycombinator.com/item?id=49610631)

[S3] [i-have-adhd installation guide](https://github.com/ayghri/i-have-adhd/blob/main/INSTALL.md)

[S4] [danyuchn/asd-ste100-skill](https://github.com/danyuchn/asd-ste100-skill)

[S5] [OpenAI: AGENTS.md discovery](https://developers.openai.com/codex/guides/agents-md)

[S6] [Claude Code: memory and project instructions](https://code.claude.com/docs/en/memory)

[S7] [Claude Code: output styles](https://code.claude.com/docs/en/output-styles)

[S8] [Vercel agent skills](https://github.com/vercel-labs/agent-skills)

[S9] [React: You Might Not Need an Effect](https://react.dev/learn/you-might-not-need-an-effect)

[S10] [Testing Library guiding principles](https://testing-library.com/docs/guiding-principles/)

[S11] [Superpowers](https://github.com/obra/superpowers)

[S12] [Evaluating AGENTS.md, version 3](https://arxiv.org/abs/2602.11988v3)

[S13] [Hacker News: Evaluating AGENTS.md](https://news.ycombinator.com/item?id=47034087)

[S14] [Claude Code best practices](https://code.claude.com/docs/en/best-practices)

[S15] [Paperclip agent instructions](https://docs.paperclip.ing/guides/org/agents/)

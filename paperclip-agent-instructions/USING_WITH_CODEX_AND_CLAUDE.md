# Use the same clarity rules outside Paperclip

**Use `portable/PLAIN_LANGUAGE.md` as your personal writing policy.** It does not depend on Paperclip, does not assume a diagnosis, and does not need a third-party plugin.

Keep the policy in personal configuration, not an employer's tracked repository file. Merge it into existing instructions rather than overwriting them.

## Codex

Append the policy to the global instruction file in the **effective** `CODEX_HOME`, normally `~/.codex/AGENTS.md`. Check whether `AGENTS.override.md` is active there first: Codex chooses that file instead of `AGENTS.md` at the same level. Start a fresh session and verify the loaded instruction chain. [S1]

Do not assume a Paperclip agent reads your normal terminal profile. Paperclip's Codex adapter can use a managed home and supplies agent instructions separately from repository discovery. Check the actual invocation, especially for sandboxed runs. [S2]

For the three Paperclip agents, use their instruction bundles as the main control; their clarity rules are already included.

## Claude Code: simplest choice

Append the portable policy to `~/.claude/CLAUDE.md`. Keep repository instructions in their existing project files. Verify the personal and project instructions in a fresh session rather than assuming the host runtime uses your own home directory. [S3]

## Claude Code: optional dedicated output style

Instead of duplicating the full policy in `CLAUDE.md`, copy `portable/claude-output-style-plain-language.md` to:

```text
~/.claude/output-styles/plain-language.md
```

Restart Claude Code after adding or editing the style file. Select **Plain language** through `/config` → Output style. For a personal default across projects, merge this field into the existing `~/.claude/settings.json` without replacing the rest:

```json
{
  "outputStyle": "Plain language"
}
```

The supplied style retains coding instructions through `keep-coding-instructions: true`. Project settings can override your personal default. [S4]

For Paperclip's headless/runtime-managed Claude sessions, verify whether the relevant settings source is loaded. Do not rely on an interactive menu choice carrying across environments; the three agent bundles already contain the writing policy.

## A current Claude/AGENTS.md loading detail

Current Claude Code can read `AGENTS.md`, but its default choice depends on whether project-path `CLAUDE.md` or `CLAUDE.local.md` files are present. Do not assume that creating a personal `CLAUDE.local.md` is harmless to discovery. Use Project instructions in `/config` to check the choice, or the documented `@AGENTS.md` import when appropriate. Direct support starts at v2.1.277, with version-specific limits. [S3]

Do not change the employer's tracked files solely to add your personal style. The Paperclip agents explicitly read the applicable repository guidance, which reduces reliance on differing discovery defaults.

## Optional on-demand rewrite skill

`portable/plain-language/SKILL.md` is an original, dependency-free rewrite skill. It contains only instructions, with no scripts or hooks. Use it when simplifying an existing explanation; it is not the always-on policy.

For Claude Code, a user-level skill can live under `~/.claude/skills/plain-language/SKILL.md`. For other tools, use their supported skill installation path or simply provide the file's instructions in the current conversation. [S5]

Do not install multiple versions of the same style and then rely on the model to reconcile conflicting requirements. The supplied policy deliberately avoids compulsory next actions, repeated progress statements, and invented duration estimates.

## Verify behaviour with one prompt

Use an actual small example, or ask:

> Explain a failed test in plain English. Start with what failed and what it affects. Show one concrete next step only if you need something from me. Do not change files.

A useful reply tells you the result immediately, preserves the important technical details, and avoids a long preamble. Also verify that the tool still follows the repository's required checks and permissions. A readable answer alone does not prove correct instruction loading.

## Sources

[S1] [Codex AGENTS.md discovery](https://developers.openai.com/codex/guides/agents-md)

[S2] [Paperclip Codex adapter](https://docs.paperclip.ing/reference/adapters/codex/)

[S3] [Claude Code instruction loading](https://code.claude.com/docs/en/memory)

[S4] [Claude Code output styles](https://code.claude.com/docs/en/output-styles)

[S5] [Claude Code skills](https://code.claude.com/docs/en/skills)

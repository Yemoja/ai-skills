# Install: three AGENTS.md files

**You only need these three files in Paperclip:**

1. [Coordinator / AGENTS.md](coordinator/AGENTS.md) → Coordinator agent → **Instructions** → replace its AGENTS.md.
2. [Senior UI Developer / AGENTS.md](senior-ui-developer/AGENTS.md) → Senior UI Developer → **Instructions**.
3. [Code Reviewer / AGENTS.md](code-reviewer/AGENTS.md) → Code Reviewer → **Instructions**.

Each file is self-contained. No MATT_POCOCK_SKILLS.md, SOUL.md, HEARTBEAT.md or shared relative paths are required. If older versions remain in your Paperclip agent bundle, remove the superseded copies so they cannot conflict. Preserve any genuinely necessary local TOOLS.md or host instructions.

## Install and attach the actual skills

In Paperclip, open **Company → Skills → Sources → Import from GitHub** and import:

https://github.com/mattpocock/skills

Then use **each agent's Skills tab** to attach these imported packages:

| Agent | Skills to attach |
| --- | --- |
| Coordinator | grilling, research, codebase-design, domain-modeling, wizard, writing-for-agents, pr |
| Senior UI Developer | diagnosing-bugs, tdd, research, prototype, codebase-design, domain-modeling, writing-for-agents |
| Code Reviewer | code-review, codebase-design, writing-for-agents |

Keep Paperclip's own coordination skill attached. Importing a skill does not automatically attach it to an agent. Do **not** paste Matt's SKILL.md files into Instructions, or install a second duplicate copy via the CLI.

Each AGENTS.md includes its own **when to invoke which skill** rules. There is no additional skill index to maintain. Agents must use the real installed skill and report if it is unavailable; merely writing /tdd does not invoke it.

## Quick verification

In Paperclip's Skill Studio, run a safe test using **Code Reviewer + code-review**, **Senior + diagnosing-bugs**, and **Coordinator + writing-for-agents**. Confirm the actual skill loads and produces useful output. Then use a harmless normal task to confirm automatic invocation at the correct trigger.

Paperclip's approval/review transition remains responsible for task status; Matt's code-review provides the analysis. Question cards need the runtime's native interaction capability; a Markdown question is not automatically a card.

Older audit/reference documents in this repository are historical background, **not** files to feed into Paperclip.

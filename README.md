# Yemoja AI skills

This repository contains a three-agent Paperclip instruction pack for a **Coordinator**, **Senior UI Developer**, and **Code Reviewer**.

Start with [`paperclip-agent-instructions/README.md`](paperclip-agent-instructions/README.md) for installation, role files, review rules, and the Paperclip settings that pair with them.

The pack integrates [Matt Pocock's upstream skills](https://github.com/mattpocock/skills) by reference. The recommended setup is to import that source through Paperclip's Skills Store, select only the compatible engineering and writing skills, and keep Paperclip's native task, permission, review, and PR workflow authoritative. See [`MATT_POCOCK_SKILLS.md`](paperclip-agent-instructions/MATT_POCOCK_SKILLS.md) for the exact source-import, role-mapping, update, and conflict rules.

The upstream skill files are deliberately not vendored here. This keeps updates centralized, avoids duplicate installations, and leaves the Paperclip-specific `AGENTS.md`, `HEARTBEAT.md`, and `SOUL.md` files as the role instructions. The role files contain conditional Skill-tool calls for accepted scope and plan stress tests, external research, human-only setup, bug diagnosis, test seams, design prototypes, native review, agent-document edits, and PR drafting; the shared integration file supplies the exact trigger map and conflict guards.

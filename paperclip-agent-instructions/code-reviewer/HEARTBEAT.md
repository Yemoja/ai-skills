# Code Reviewer — HEARTBEAT

This is the **per-wake procedure**. Follow Paperclip's installed coordination skill and native review transition. `AGENTS.md` owns review standards, Matt skill use and delta policy; `SOUL.md` owns communication style.

1. **Verify review ownership.** In a native review stage, check the task's current reviewer/participant is you; for a separately assigned review task, report on that task. No actionable review or a human hold = stop. Do not edit implementation.
2. **Pin the correct revision.** On a new session read `AGENTS.md` and `SOUL.md`, the accepted Paperclip task/plan, relevant application docs and developer handoff. Identify the *task's actual execution workspace*, PR target base when present (otherwise the accepted workspace base), and **exact head SHA**. Never guess `main` or use `HEAD~1` as the base. Inspect the full committed three-dot diff; check uncommitted changes separately.
3. **Use Matt's `code-review`** once for the materially changed revision. Supply the pinned diff, repo standards and accepted Paperclip task as the spec source; retain separate **Standards** and **Spec** results. The skill's internal helpers do not create Paperclip review tasks.
4. **On re-review**, load the previous review verdict, stable finding IDs and old/new head refs. Check each earlier issue and the actual correction delta (use `git range-diff` for rebases when possible). Do not re-audit unchanged code for fresh preferences; flag specific critical risks with evidence.
5. **Decide once.** Record base and reviewed head SHA, blocking findings with stable IDs, required checks and limitations. Make **one** native Paperclip approval or change request, respecting the review cap and human escalation. If Matt's skill is unavailable, report the limitation and do not claim it ran.

**Persist once, then stop.** Avoid duplicating the review decision in another comment; follow Paperclip's checkout and run-comment rules, including `X-Paperclip-Run-Id` for direct API writes. No extra tasks, PR creation, implementation, repeated waiting comments or polling.

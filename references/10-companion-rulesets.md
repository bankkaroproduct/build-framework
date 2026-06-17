# 10 — Companion Rulesets & Further Reading

This framework governs **safety + process**. It pairs with discipline-specific rulesets that govern *how well* a particular kind of work is done. These are optional add-ons you can plug into a project alongside this framework. Below is a curated, filtered list — not everything that exists, just the ones worth your time — with the honest gaps called out.

> **Read this caveat first.** A rules/skills file is a *dependency*: your AI reads it as always-on instructions, so a malicious or compromised one can quietly steer your agent (a real supply-chain risk). Treat every third-party ruleset like any other dependency — **read it before adopting, pin it to a known commit, and don't blindly auto-update.** Same "justify every dependency" gate from `02-orchestration.md`, applied to rules.

---

## By discipline

### Code restraint
- **[ponytail](https://github.com/DietrichGebert/ponytail)** — a "write less code" deliberation ladder for AI agents (YAGNI → stdlib → native → existing dep → one line). The reference example of the genre. Composes cleanly: ponytail = code restraint, this framework = safety + process. **Recommended.** (Already distilled into the Code-quality lens, `09-quality-lenses.md`.)

### Writing / copy
- **[`yzhao062/agent-style`](https://github.com/yzhao062/agent-style)** — ~21 writing rules, drop-in for Claude Code / Codex / Copilot / Cursor / Aider. The closest direct ponytail-equivalent for writing. **Recommended** for code + docs voice. *Note:* tuned for tech/docs writing, not consumer microcopy — pair with a product-copy style guide (Mailchimp Content Style Guide; Google or Microsoft writing style guide) for button labels and error messages.

### Accessibility (strong area — real options)
- **[`fecarrico/A11Y.md`](https://github.com/fecarrico/A11Y.md)** — forces WCAG 2.2 AA from the first line of generated UI; copy to repo root / inject into rules. Closest ponytail-equivalent. **Recommended** for real-user apps.
- **[`mikemai2awesome/a11y-rules`](https://github.com/mikemai2awesome/a11y-rules)** — paste into Cursor / Windsurf / Claude.
- **[`simonplmak-cloud/wcag-aaa-web-design`](https://github.com/simonplmak-cloud/wcag-aaa-web-design)** — WCAG 2.2 AAA design system + agent skill (enterprise-grade).
- **A11y MCP** ([a11ymcp](https://mcpservers.org/servers/ronantakizawa/a11ymcp)) — an MCP server for *automated* accessibility testing. A tool, complements the rules.
- Canonical standard to point at: **[WCAG 2.2](https://www.w3.org/WAI/standards-guidelines/wcag/)**.

### Security (strong area)
- **[`matank001/cursor-security-rules`](https://github.com/matank001/cursor-security-rules)** — security rules for AI-assisted dev. **Recommended**, pairs with our security baseline (`04-safety-rails.md`).
- **[`hoodini/ai-agents-skills` → owasp-security](https://github.com/hoodini/ai-agents-skills)** — an OWASP-oriented agent skill.
- Canonical standards to point at: **[OWASP Top 10](https://owasp.org/www-project-top-ten/)**, **[OWASP Top 10 for LLM Apps](https://owasp.org/www-project-top-10-for-large-language-model-applications/)**, and **[CSA Secure Vibe Coding / RAILGUARD](https://cloudsecurityalliance.org/blog/2025/05/06/secure-vibe-coding-level-up-with-cursor-rules-and-the-r-a-i-l-g-u-a-r-d-framework)**.

### General collections (mine these per stack)
- **[`PatrickJS/awesome-cursorrules`](https://github.com/PatrickJS/awesome-cursorrules)** and **[`sanjeed5/awesome-cursor-rules-mdc`](https://github.com/sanjeed5/awesome-cursor-rules-mdc)** — large curated collections where stack-specific rules (React, Next, Python…) live. All built on the **[AGENTS.md open standard](https://agents.md)**, so they're tool-agnostic like this framework.

### UI / visual design
- **[`impeccable`](https://github.com/pbakaus/impeccable)** — anti-AI-slop design rules. **Recommended** (already in `09-quality-lenses.md`).
- Canonical: **Refactoring UI** (Adam Wathan & Steve Schoger).

---

## Honest gaps (no clean ponytail-equivalent exists yet)

- **UI / visual.** No single dominant rule pack. Best practice: `impeccable` for anti-slop + adopt/point the AI at a **real design system** (so it composes from known primitives instead of inventing) + Refactoring UI principles. A genuine ecosystem gap, not an omission here.
- **Testing.** Mostly *practice patterns* (TDD-with-agents prompting), not a dominant installable pack.
- **Roadmap / product strategy.** No agent ruleset — and none needed. Deciding *what* to build is human judgment (Now-Next-Later, RICE). Out of scope for this framework on purpose.

---

## How to adopt one safely

1. Read the ruleset's actual contents — it's instructions your AI will obey.
2. Pin to a specific commit (don't track a moving `main`).
3. Add it to your project's `AGENTS.md` rules with a one-line note on *why* it's there.
4. Re-review on update, same as any dependency bump.

Start small: most projects need at most a couple (e.g. `agent-style` + an accessibility pack + a security pack). Don't stack ten — that's noise the AI can't reconcile.

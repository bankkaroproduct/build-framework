# Build Framework

**A safe operating framework for building production-grade apps with AI coding tools — written for non-engineers (PMs, founders, prototypers).**

It stops the usual AI-coding disasters — leaked secrets, lost data, broken apps shipped to real users, surprise bills — by turning "ask the AI to build it and hope" into a repeatable process: plan → build → independent review → human gate → ship.

**Works with:** Claude Code · Codex · Cursor · ChatGPT — or any AI coding tool that edits files in a local folder. It's **tool-agnostic** (speaks in roles, not tools) and **product-agnostic** (any web app with its own database and hosting fits). Graduating from no-code / Lovable / "vibe coding" to a real codebase? This is the missing operating manual.

## Get it

```
git clone https://github.com/bankkaroproduct/build-framework.git
```

Or grab just the drop-in templates for a new project (no full clone):
```
npx degit bankkaroproduct/build-framework/templates ./_framework-templates
```

## Start here

1. **Read [`FRAMEWORK.md`](./FRAMEWORK.md)** — the whole approach in one page: three roles, the five-step loop, the always-on safety rails. If you read one thing, read this.
2. **Starting a new project?** Follow [`bootstrap.md`](./bootstrap.md) — five steps (each with a terminal *and* an "ask your AI" version) that drop the rules into your new app so your AI tools follow them automatically.
3. **Using a specific AI tool?** [`TOOL_MATRIX.md`](./TOOL_MATRIX.md) — how Codex / Claude Code / Cursor / ChatGPT-reviewer each plug in.
4. **Want to see it in action?** [`examples/worked-example.md`](./examples/worked-example.md) — one feature from handoff to review to ship.
5. **Want the detail on a step?** It's in [`references/`](./references/).

## What's in here

```
FRAMEWORK.md                  The core: roles, the loop, the safety rails. Read this first.
bootstrap.md                  5 steps to start a new project (terminal or "ask your AI").
TOOL_MATRIX.md                How each AI tool (Codex/Claude/Cursor/ChatGPT) plugs in.
references/
  01-project-setup.md         Repo, hosting, DB, secrets, real-vs-demo classification.
  02-orchestration.md         Directing the AI: planning, handoffs, dependency gate,
                              running >1 builder, the reviewer loop.
  03-verification-shipping.md  How to actually verify; the production gate; main≠prod check.
  04-safety-rails.md          The safety rails in full: secrets, API ownership + safe-write,
                              preview isolation, DB backups, security baseline, cost, escalation.
  05-incident-escalation.md   When things break; when to call a human engineer.
  06-existing-project-mode.md  Applying the framework to a repo that already exists.
  07-harden-your-repo.md      Optional automatic guards: secret scanning, branch protection.
  08-context-and-thread-management.md  When a chat goes slow/stale: fork cleanly, lose nothing.
  09-quality-lenses.md        UI / UX / Copy / Code checklists so the result is good, not just safe.
  10-companion-rulesets.md    Curated AI-agent rulesets to plug in per discipline (+ how to adopt safely).
templates/
  AGENTS.md                   Drop in your project root. Many AI tools read it automatically.
  CLAUDE.md                   Thin pointer for Claude Code → follows AGENTS.md.
  handoff.template.md         Planner → Builder contract. Has a machine-readable YAML block
                              (role/allowed-paths/stops) so AIs can't interpret creatively.
  review-request.template.md  Builder → Reviewer contract (evidence-based, go/no-go).
  checkpoint.md               A "where are we now" snapshot so a fresh thread can resume.
  env.example                 The secrets pattern (keys in env vars, never in git).
examples/
  worked-example.md           One feature, handoff → build → review → ship.
.github/workflows/
  secret-scan.yml             Drop-in: blocks any push containing a leaked secret.
account-level/
  operating-rules.md          The condensed, audience-neutral spine to set as your global
                              Claude/Codex config or online account instructions.
  README.md                   How to install it globally (Claude Code, Codex) + online.
```

## Pairs well with

This framework governs *safety + process*. It composes with discipline-specific AI-agent rulesets — [`ponytail`](https://github.com/DietrichGebert/ponytail) (code restraint), [`agent-style`](https://github.com/yzhao062/agent-style) (writing), [`A11Y.md`](https://github.com/fecarrico/A11Y.md) (accessibility), [`cursor-security-rules`](https://github.com/matank001/cursor-security-rules) (security), and more. **`references/10-companion-rulesets.md`** has the curated list per discipline, the honest gaps, and — importantly — how to adopt one safely (rules files are a supply-chain vector; review and pin before you trust).

## The idea in three sentences

You direct, you don't code. Three roles do the work — a **Planner** decides what to build, a **Builder** writes it, and a **Reviewer** (a different session, even of the same tool) checks it — and you approve at each gate. Safety rails **tell the tools when to stop** before the dangerous moves (secrets, writing to systems you don't own, preview touching production data, ungated production deploys, runaway costs); optional repo guards (secret scanning, branch protection) then *block* the worst mistakes automatically. Nothing reaches real users without your explicit "yes."

## Who this is for

Anyone building production-grade MVPs with AI tools who isn't a professional engineer — PMs, founders, and prototypers graduating from no-code, Lovable, or "vibe coding" to real codebases with their own databases and hosting. If you direct AI to write code you can't fully review yourself, this is for you.

<sub>Keywords: AI coding · agentic development · Claude Code · Codex · Cursor · ChatGPT · LLM coding agents · vibe coding · no-code to code · MVP · guardrails · AI safety for builders · product manager engineering.</sub>

## License

[MIT](./LICENSE) — use it, copy it, adapt it freely.

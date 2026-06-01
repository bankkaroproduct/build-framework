# Build Framework

A way for non-engineers to ship real software with AI coding tools — without the usual disasters (leaked secrets, lost data, broken apps shipped to real users, surprise bills).

It's **tool-agnostic** (works with Claude Code, Codex, Cursor, ChatGPT, or whatever's next — it speaks in roles, not tools) and **product-agnostic** (any web app with its own database and hosting fits).

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
```

## The idea in three sentences

You direct, you don't code. Three roles do the work — a **Planner** decides what to build, a **Builder** writes it, and a **Reviewer** (a *different* AI) checks it — and you approve at each gate. Safety rails catch the dangerous moves automatically (secrets, writing to systems you don't own, preview touching production data, ungated production deploys, runaway costs), and nothing reaches real users without your explicit "yes."

## Who this is for

Anyone building production-grade MVPs with AI tools who isn't a professional engineer — PMs, founders, prototypers graduating from no-code/Lovable to real codebases with their own databases and hosting.

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
2. **Starting a new project?** Follow [`bootstrap.md`](./bootstrap.md) — five copy-paste steps that drop the rules into your new app so your AI tools follow them automatically.
3. **Want the detail on a step?** It's in [`references/`](./references/).

## What's in here

```
FRAMEWORK.md                  The core: roles, the loop, the safety rails. Read this first.
bootstrap.md                  5 steps to start a new project the right way.
references/
  01-project-setup.md         Repo, hosting, DB, secrets, real-vs-demo classification.
  02-orchestration.md         Directing the AI: planning, handoffs, dependency gate, reviewer loop.
  03-verification-shipping.md  How to actually verify; the production gate; error visibility.
  04-safety-rails.md          The safety rails in full: secrets, API ownership + safe-write,
                              preview isolation, DB backups, security baseline, cost, escalation.
  05-incident-escalation.md   When things break; when to call a human engineer.
  06-existing-project-mode.md  Applying the framework to a repo that already exists.
templates/
  AGENTS.md                   Drop in your project root. Most AI tools read it automatically.
  CLAUDE.md                   Thin pointer for Claude Code → follows AGENTS.md.
  handoff.template.md         Planner → Builder contract (what to build, what not to touch).
  review-request.template.md  Builder → Reviewer contract (independent check before shipping).
  env.example                 The secrets pattern (keys in env vars, never in git).
```

## The idea in three sentences

You direct, you don't code. Three roles do the work — a **Planner** decides what to build, a **Builder** writes it, and a **Reviewer** (a *different* AI) checks it — and you approve at each gate. Safety rails catch the dangerous moves automatically (secrets, writing to systems you don't own, preview touching production data, ungated production deploys, runaway costs), and nothing reaches real users without your explicit "yes."

## Who this is for

Anyone building production-grade MVPs with AI tools who isn't a professional engineer — PMs, founders, prototypers graduating from no-code/Lovable to real codebases with their own databases and hosting.

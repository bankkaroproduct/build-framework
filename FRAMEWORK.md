# The Build Framework

A way of working that lets a non-engineer ship real software with AI coding tools — without the usual ways it goes wrong (leaked secrets, lost data, broken apps shipped to real users, surprise bills).

This is **tool-agnostic**: it works whether your AI helper is Claude Code, Codex, Cursor, ChatGPT, or whatever comes next. It speaks in **roles**, not tools.

This is **product-agnostic**: it doesn't assume what you're building. Any web app with its own database and hosting fits.

> If you read nothing else, read this file. Everything in `references/` is just the detail behind these rules.

---

## The mental model: three roles

You don't do the coding. You direct it. Think of three jobs — any AI tool can fill any of them, and sometimes you fill one yourself.

| Role | What it does | Who/what fills it |
|---|---|---|
| **Planner** | Decides *what* to build and writes it down clearly before any code is written | You + an AI thinking partner (Claude, ChatGPT) |
| **Builder** | Reads the plan, writes the code in your project folder, reports what it did | A coding tool that edits local files (Claude Code, Codex, Cursor) |
| **Reviewer** | Independently checks the Builder's work before it goes live | A *different* AI than the Builder (e.g. ChatGPT + GitHub), or a second agent |

**Why separate them?** The Builder is too close to its own work to catch its own mistakes — same reason you don't proofread your own writing well. A separate Reviewer catches things the Builder is blind to. And a written Plan stops the Builder from wandering off and "improving" things you didn't ask for.

The **handoff** (a short markdown file — see `templates/handoff.template.md`) is how the Planner tells the Builder what to do. The **review request** (`templates/review-request.template.md`) is how you ask the Reviewer to check it. Because these are just text files in your project, *any* tool can read them. That's the whole trick to staying tool-agnostic.

---

## The operating loop

Every piece of work — a new app, a new feature, a fix — goes through the same five steps. Don't skip steps. Skipping is how things break.

### 1. Classify
Before anything, answer one question: **does this app touch real users or real data?**

- **Real** (logins, customer data, anything personal, anything you'd be embarrassed to leak) → full rigor. Every safety rail below applies.
- **Demo** (throwaway data, just you, nobody's real info) → lighter; you can move faster.

If you're not sure, treat it as **Real**. See `references/01-project-setup.md`.

### 2. Plan
Get the AI to write down *what* it's going to build **before** it writes code. A good plan names the smallest useful slice — not the whole dream, just the next shippable piece. Make the AI explain, in plain English, anything that costs money or touches user data. You approve the plan before building starts.

### 3. Build
The Builder works from a handoff (`templates/handoff.template.md`). The handoff says what to touch, what NOT to touch, and how you'll know it worked. The Builder reports back what it actually did. See `references/02-orchestration.md`.

### 4. Verify
**This is the step everyone skips and everyone regrets.** "Done" from an AI does *not* mean it works. Before you believe it:
- Actually open the running app and click through the thing that was built.
- If it's an API or backend, actually call it and look at the response.
- Get the Reviewer to look at the change independently.

A green checkmark, a passing test, or an AI saying "✅ done" is **not** proof. Your own eyes on the running app is proof. See `references/03-verification-shipping.md`.

### 5. Ship — behind a gate
- **Preview deploys** (a test URL nobody real uses) → ship freely, that's what they're for.
- **Production** (the URL real users hit) → **never automatic**. You explicitly say "yes, push to production" every single time, after Verify passed.

See `references/03-verification-shipping.md`.

---

## The always-on safety rails

These apply at every step, even when you didn't ask and the AI didn't mention them. If you've set up your project with the `AGENTS.md` template, your tools already know these. They are **STOP signs** — when one is about to be crossed, work pauses and you get a plain-English heads-up.

1. **Secrets never go in code or git.** API keys, passwords, database URLs, tokens → they live in environment variables (your hosting provider's settings), never in a file that gets committed, never in code the browser can see. If an AI is about to paste a key into a file → STOP. See `references/04-safety-rails.md`.

2. **Never write to an API you don't own.** Your app may *read* from other products' APIs as a guest. It must **not** create, change, or delete anything there without explicit human sign-off — by default, read-only. Treat every external API as "look, don't touch." See `references/04-safety-rails.md`.

3. **Back up the database before changing its shape.** Any operation that alters or deletes data (a "migration", a "drop", a "reset") → make a backup first. Data you delete by accident is usually gone for good.

4. **Real user data = auth + privacy hygiene.** If real people log in or you store anything personal, the app needs proper login and you must be careful with that data. If an AI is about to store personal data loosely → it should flag it to you.

5. **Production deploy = explicit human "yes," every time.** No tool ships to real users on its own.

6. **When something is genuinely risky and you're out of your depth — stop and get a human engineer.** Payments, security, anything involving money or sensitive data, anything where a mistake is expensive and irreversible. The right move is to say "this needs a real engineer," not to wing it. See `references/05-incident-escalation.md`.

---

## The non-negotiable: plain language

You are not expected to read code. Any AI working with you under this framework must explain what it's doing, what it costs, and what could go wrong **in plain English** — not in diffs, not in jargon. If you don't understand the explanation, that's the AI's failure to explain, not yours to understand. Ask it again, simpler.

---

## Starting a new project?

Go to `bootstrap.md`. Five steps, copy-paste. It drops these rails into your new project so your tools follow them automatically.

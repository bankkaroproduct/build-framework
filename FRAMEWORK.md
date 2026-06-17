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
| **Reviewer** | Independently checks the Builder's work before it goes live | A *different* AI — or, if you use only one tool, a **fresh cold-start session** of the same tool |

**Why separate them?** The Builder is too close to its own work to catch its own mistakes — same reason you don't proofread your own writing well. A separate Reviewer catches things the Builder is blind to. And a written Plan stops the Builder from wandering off and "improving" things you didn't ask for.

**You don't need two tools to do this** — most people use just one (Claude, or Codex). One tool plays Planner and Builder in sequence; for the Reviewer, you open a **fresh "cold-start" session** of the same tool and hand it only the change to check. A session with no memory of building something reviews it honestly instead of defending it. See `TOOL_MATRIX.md`.

The **handoff** (a short markdown file — see `templates/handoff.template.md`) is how the Planner tells the Builder what to do. The **review request** (`templates/review-request.template.md`) is how you ask the Reviewer to check it. Because these are just text files in your project, *any* tool can read them. That's the whole trick to staying tool-agnostic.

---

## The operating loop

Every piece of work — a new app, a new feature, a fix — goes through the same five steps. Don't skip steps. Skipping is how things break.

### 1. Classify
Before anything, answer one question: **does this app touch real users or real data?**

- **Real** (logins, customer data, anything personal, anything you'd be embarrassed to leak) → full rigor. Every safety rail below applies.
- **Demo** (throwaway data, just you, nobody's real info) → lighter; you can move faster.

**The sharper rule:** "no public users yet" does **not** mean "safe." If the app touches **real data, real company infrastructure, or real external APIs** — even an internal tool only you use — treat it as **Real**. A private beta with five friends, an admin tool wired to a real database, a prototype already calling a paid API: all **Real**. If you're not sure, it's Real. See `references/01-project-setup.md`.

### 2. Plan — *then challenge the plan*
Get the AI to write down *what* it's going to build **before** it writes code. A good plan names the smallest useful slice — not the whole dream, just the next shippable piece. Make the AI explain, in plain English, anything that costs money or touches user data.

Then do the step almost everyone skips — **challenge the plan before you approve it.** Every "build assistant" verifies the *code* against the plan; almost none ask whether the *plan itself* is sound. Run a quick Devil's-Advocate pass (ideally in a fresh/cold session — see the Reviewer trick, but pointed upstream) on four questions:
- **Is this even the right thing to build?** What problem does it actually solve?
- **Is there a simpler way** that gets 80% of the value?
- **What assumption is this resting on** — and what breaks if that assumption is wrong?
- **What's the riskiest part**, and should that be proven first?

This costs two minutes and is the cheapest place to kill a bad idea — far cheaper than building it well and discovering it was wrong. Only *then* approve the plan and build.

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

## What "production-grade" actually means

You may not be launching to 10,000 users today. "Production-grade" isn't about scale — it's about **not building a mess you have to throw away.** An app is production-grade when:

- the repo is understandable and the setup is reproducible (someone else could run it),
- it builds cleanly and any basic checks it has (lint/typecheck) are green before you ship — you don't need a full test pipeline, but "the build is broken" is never production-grade,
- environment variables are documented (`.env.example` exists),
- there's a clear data model and a rollback path,
- errors are visible (you find out it broke before a user tells you),
- no secrets are in the code,
- known risks and any "we cut this corner for now" shortcuts are written down,
- it's **handoff-ready** — a real engineer could pick it up later without archaeology.

The goal: build so that going to production *later* is a small step, not a rebuild.

---

## The always-on safety rails

These apply at every step, even when you didn't ask and the AI didn't mention them. If you've set up your project with the `AGENTS.md` template, your tools already know these. They are **STOP signs** — when one is about to be crossed, work pauses and you get a plain-English heads-up. Full detail for every rail is in `references/04-safety-rails.md`.

1. **Secrets never go in code or git.** API keys, passwords, database URLs, tokens → they live in environment variables (your hosting provider's settings), never in a committed file, never in code the browser can see. If an AI is about to paste a key into a file → STOP.

2. **Never write to an API you don't own.** Your app may *read* from other people's APIs as a guest, by default read-only. It must **not** create/change/delete anything on a system you don't own without explicit sign-off. (Writing to services *you* own — your email, your payment provider — is fine, but use the safe-write checklist: sandbox keys first, sign-off before going live.)

3. **Preview must never touch production data.** A test/preview deploy is only safe if it points at a **non-production** database and **sandbox** API keys. A preview wired to your real database can delete real data while you think you're "just testing." If a preview must use real data, it's no longer a preview — treat it as Real/Production. → STOP and confirm.

4. **Back up the database before changing its shape.** Any operation that alters or deletes data (a "migration", a "drop", a "reset") → back up first. Data you delete by accident is usually gone for good.

5. **Real user data = auth + privacy hygiene.** If real people log in or you store anything personal, the app needs proper login, and one user must never be able to see another user's data. If an AI is about to store/expose personal data loosely → it flags it to you. (PM-readable security checklist in `references/04-safety-rails.md`.)

6. **Production deploy = explicit human "yes," every time.** No tool ships to real users on its own.

7. **Watch the bill.** Know what costs money before turning it on; set a hard spending cap + budget alert at setup. AI-built apps can create runaway loops, over-frequent scheduled jobs, and expensive logging. If an AI wires up anything metered → it tells you the cost shape in plain English first.

8. **When something is genuinely risky and you're out of your depth — stop and get a human engineer.** Payments, security, sensitive data, hard-to-undo data-model decisions, anything where a mistake is expensive and irreversible. The right move is "this needs a real engineer," not winging it. See `references/05-incident-escalation.md`.

There's also one **build-time STOP**: before adding a new software package/dependency, the AI should justify it in plain English (do we need it? is it widely used + maintained? could it touch secrets/data?). AI tools install packages casually; each one is new code you now depend on. See `references/02-orchestration.md`.

---

## Keep your thread fresh

AI conversations get worse the longer they run — slower, forgetful, more mistake-prone — as the chat fills up its limited memory. This *will* happen on a real build. The fix: **the chat is disposable; your project's real memory lives in files** (`AGENTS.md`, your code, your checkpoint). When a thread goes stale (slow replies, forgetting earlier decisions, repeating itself), start a fresh one and point it at those files — you lose nothing important. Do milestone check-ins *before* a thread fills, so forking is painless. Full playbook: `references/08-context-and-thread-management.md`.

## The non-negotiable: plain language

You are not expected to read code. Any AI working with you under this framework must explain what it's doing, what it costs, and what could go wrong **in plain English** — not in diffs, not in jargon. If you don't understand the explanation, that's the AI's failure to explain, not yours to understand. Ask it again, simpler.

---

## Starting a new project?

Go to `bootstrap.md`. Five steps, copy-paste. It drops these rails into your new project so your tools follow them automatically.

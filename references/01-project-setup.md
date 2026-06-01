# 01 — Project Setup

How to start a new app so it's safe from day one. Do this **before** you ask any AI to build features.

---

## Step 0: Classify the project

Answer one question and write the answer at the top of your project's `AGENTS.md`:

> **Does this app touch real users or real data?**

- **Real-data project** — has logins, stores customer/personal info, or handles anything you'd be in trouble for leaking. → Full rigor. Every rail in `04-safety-rails.md` applies.
- **Demo project** — throwaway data, only you use it, no real people's information. → Lighter. Move fast, but the secrets rule still applies (never commit keys, even in demos).

**The sharper rule (don't mis-classify):** "no public users yet" does **not** mean Demo. If the app touches **real data, real company infrastructure, or real external APIs** — even an internal tool only you use, even a five-person private beta — it's **Real**. **If unsure, it's Real.** Downgrading later is easy; cleaning up a leak is not.

---

## Step 1: One repo, clear structure

- One app = one Git repository. Don't mix two apps in one repo.
- Your **production branch (usually `main`)** = always deployable. If it's on that branch, it should work.
- New features go on a **branch** (e.g. `feature/login`), not directly on the production branch.
- Most hosting (like Vercel) gives every branch its own **preview URL** automatically. That preview is where you test. The production branch is what goes to real users. (Confirm your *actual* deploy trigger — see `03-verification-shipping.md`; it isn't always "merge to main.")

You don't need to understand Git deeply. You need exactly this rule: **build on a branch, test the preview, only merge to your production branch when it works.**

---

## Step 2: Hosting + database

- **Hosting** (Vercel, Netlify, etc.): connect it to your GitHub repo once. After that, every push gives you a preview; merging to your production branch updates production (confirm that's actually how yours deploys — see `03-verification-shipping.md`).
- **Database**: pick a managed one (Supabase, Neon, PlanetScale, etc.) so you're not running servers. Managed databases handle backups and security patches for you.
- **Use a separate database for preview vs production.** Your test/preview environment must point at a **non-production** database with **sandbox** API keys — never the real one. A preview wired to your real database can delete real data while you think you're "just testing." (This is safety Rail 3 — `04-safety-rails.md`.)
- Write down — in `AGENTS.md` — which hosting and which database this project uses, so every AI session knows.

---

## Step 3: Secrets, the right way (this is the big one)

A "secret" = any key, password, token, or connection string. Examples: a database URL, an API key, a login secret.

**The rule: secrets live in environment variables, never in your code or Git.**

How it works in practice:
1. Your hosting provider has a "Environment Variables" settings page. Secrets go *there*.
2. Locally, secrets go in a file called `.env` (or `.env.local`) — and that file is listed in `.gitignore` so it never gets committed.
3. Your code reads secrets from the environment (e.g. `process.env.DATABASE_URL`), never has them typed in directly.
4. You keep a `.env.example` file (no real values, just the names) committed, so you remember what's needed. Template provided in `templates/env.example`.

**The STOP sign:** if an AI is about to write a real key into any file other than your local `.env`, or into code the browser downloads → stop it. Ask "is this a secret? then it goes in env vars."

Why this matters: anything committed to Git is effectively permanent and often public. Keys leaked this way get found by bots within minutes and abused (run up huge bills, steal data). This is the single most common way prototypes get burned.

---

## Step 4: Drop in the framework files

From the `build-framework` repo, copy these into your new project (see `bootstrap.md` for the exact commands):
- `AGENTS.md` → your project root. This is the file your AI tools read automatically. It carries all the rails.
- `CLAUDE.md` → a thin pointer for Claude Code (it reads this; it just says "follow AGENTS.md").
- `.env.example` → reminds you what secrets the project needs.

Fill in the top of `AGENTS.md`: project name, real-vs-demo, hosting, database. Thirty seconds, and now every AI session starts already knowing your rules and your setup.

---

## Step 5: Write down what you're building

In `AGENTS.md`, there's a memory section. Put two or three sentences about what this app is for and who uses it. This survives across sessions and across tools — so you don't re-explain your project every time you open a new chat. Keep it updated as the app grows.

---

## Step 6: Harden the repo (Real-data projects)

For anything classified Real, spend ~10 minutes on the two automatic guards in `07-harden-your-repo.md`: **secret scanning** (the repo refuses a push containing a key) and **branch protection** on your production branch. These *block* the irreversible, invisible mistakes a non-engineer can't catch in a diff — the one place automation beats instructions.

---

## A note on what comes next

Once setup is done, every piece of work follows the loop in `FRAMEWORK.md` (Classify → Plan → Build → Verify → Ship-behind-a-gate). Two things from the rails that matter even at setup time, so you're not surprised later:
- **Dependency gate** — your AI should justify any new software package before adding it (`02-orchestration.md`). Don't let setup quietly pull in dozens of libraries.
- **Picking up an existing project instead of starting fresh?** Use `06-existing-project-mode.md` (understand before you touch).

---

## Checklist before you build a single feature

- [ ] Classified real-vs-demo (using the sharper rule), written in `AGENTS.md`
- [ ] Repo created, production-branch + feature-branch habit understood
- [ ] Hosting connected, database chosen, both noted in `AGENTS.md`
- [ ] Preview uses a non-prod database + sandbox keys (not the real ones)
- [ ] `.gitignore` includes `.env` / `.env.local`
- [ ] `AGENTS.md` + `CLAUDE.md` + `.env.example` dropped in
- [ ] One-paragraph "what is this app" written in the memory section
- [ ] (Real-data) secret scanning + branch protection turned on (`07-harden-your-repo.md`)

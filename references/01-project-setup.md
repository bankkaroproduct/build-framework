# 01 — Project Setup

How to start a new app so it's safe from day one. Do this **before** you ask any AI to build features.

---

## Step 0: Classify the project

Answer one question and write the answer at the top of your project's `AGENTS.md`:

> **Does this app touch real users or real data?**

- **Real-data project** — has logins, stores customer/personal info, or handles anything you'd be in trouble for leaking. → Full rigor. Every rail in `04-safety-rails.md` applies.
- **Demo project** — throwaway data, only you use it, no real people's information. → Lighter. Move fast, but the secrets rule still applies (never commit keys, even in demos).

**If unsure, it's Real.** Downgrading later is easy; cleaning up a leak is not.

---

## Step 1: One repo, clear structure

- One app = one Git repository. Don't mix two apps in one repo.
- `main` branch = always deployable. If it's on `main`, it should work.
- New features go on a **branch** (e.g. `feature/login`), not directly on `main`.
- Most hosting (like Vercel) gives every branch its own **preview URL** automatically. That preview is where you test. `main` is what goes to real users.

You don't need to understand Git deeply. You need exactly this rule: **build on a branch, test the preview, only merge to `main` when it works.**

---

## Step 2: Hosting + database

- **Hosting** (Vercel, Netlify, etc.): connect it to your GitHub repo once. After that, every push gives you a preview; merging to `main` updates production.
- **Database**: pick a managed one (Supabase, Neon, PlanetScale, etc.) so you're not running servers. Managed databases handle backups and security patches for you.
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

## Checklist before you build a single feature

- [ ] Classified real-vs-demo, written in `AGENTS.md`
- [ ] Repo created, `main` + feature-branch habit understood
- [ ] Hosting connected, database chosen, both noted in `AGENTS.md`
- [ ] `.gitignore` includes `.env` / `.env.local`
- [ ] `AGENTS.md` + `CLAUDE.md` + `.env.example` dropped in
- [ ] One-paragraph "what is this app" written in the memory section

# 07 — Harden Your Repo (optional, ~10 minutes)

The safety rails are instructions an AI *should* follow. This file adds a few **automatic** guards so the repo itself blocks the worst mistakes — useful because a non-engineer can't catch a leaked secret by reading code. These are belt-and-suspenders on top of the rails. Optional, but the first two are worth doing for any Real-data project.

> You don't have to set these up by hand. Each has an "ask your AI" prompt.

---

## 1. Secret scanning (highest value — do this)

A leaked API key is the disaster a non-engineer is least able to see coming. This makes the repo **refuse a commit/push that contains a secret**.

**The easy way — GitHub's built-in push protection:**
In your repo on GitHub → **Settings → Code security** → enable **Secret scanning** and **Push protection**. One toggle. GitHub will now block pushes that contain known secret formats.

**The portable way — a tiny scan that runs on every push:**
This repo ships `.github/workflows/secret-scan.yml` (uses [gitleaks](https://github.com/gitleaks/gitleaks)). Copy it into your project's `.github/workflows/`. It only fails when it actually finds a secret — exactly the moment you want a hard stop. Nothing to maintain.

> **Ask your AI:** *"Add the gitleaks secret-scanning GitHub Action to this repo so any push containing an API key or password fails the check. Also tell me how to turn on GitHub's built-in Secret Scanning + Push Protection in the repo settings."*

If a scan ever fails: it found a secret. Remove it from the code, move it to an environment variable (see `04-safety-rails.md` Rail 1), and — important — treat that key as compromised and rotate it (generate a new one), because it's now in git history.

---

## 2. Protect the `main` branch (do this for Real-data projects)

Stops anything (including an over-eager AI) from pushing straight to `main` — your production branch — without going through a branch + check.

In your repo on GitHub → **Settings → Branches → Add branch protection rule** for `main`:
- Require a pull request before merging
- Require status checks to pass (select the secret-scan check from step 1)

> **Ask your AI:** *"Walk me through turning on branch protection for `main` in this GitHub repo: require a pull request and require the secret-scan check to pass before merging. Tell me exactly what to click."*

Now `main` (production) can't be changed by accident — every change goes through a branch you tested.

---

## 3. An `.env.example` that stays honest (nice-to-have)

It's easy for the code to start needing a new secret while `.env.example` forgets to list it — then production breaks because an env var is missing. A tiny check can compare the two. This is a nice-to-have; skip it until you've been bitten once.

> **Ask your AI:** *"Add a small check that fails CI if the code references an environment variable that isn't listed in `.env.example`, so we never forget to document a required secret."*

---

## What NOT to over-build

You do **not** need a full CI pipeline (lint + typecheck + full test suite + coverage gates) to be production-grade as a solo builder. A failing check you can't debug is worse than no check. Start with **secret scanning** and **branch protection** — they catch the irreversible, invisible mistakes. Add more checks only when a specific pain makes you want them.

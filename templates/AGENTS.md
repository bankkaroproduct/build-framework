# AGENTS.md — operating rules for this project

> This file is read automatically by most AI coding tools (Codex, and others). It tells any AI working in this folder how to behave. Keep it at the project root. Fill in the blanks at the top.

## This project

- **Name:** _<fill in>_
- **What it is (1–2 sentences):** _<fill in — what it does, who uses it>_
- **Real-data or Demo:** _<Real users/data — full rigor | Demo — throwaway data>_  (if unsure → Real)
- **Hosting:** _<e.g. Vercel>_
- **Database:** _<e.g. Supabase / Neon / none>_

## How we work here (non-negotiable)

This project follows the Build Framework. The human directing this work is **non-technical** — explain everything in plain English, never assume diffs or jargon are understood.

**The loop:** Classify → Plan → Build → Verify → Ship-behind-a-gate. Plan before building. Verify with eyes on the running app, not by claiming "done."

**Always-on safety rails — these are STOP signs. When about to cross one, pause and ask in plain English:**

1. **Secrets** (keys, passwords, tokens, DB URLs) → environment variables only. NEVER in committed files or browser-visible code. → STOP before writing a secret anywhere but local `.env`.
2. **External APIs we consume** → read-only by default. NEVER write to (create/update/delete) an API we don't own without explicit human sign-off. → STOP before any non-read call to an external service.
3. **Database** → back up before any migration / destructive / delete operation. → STOP before altering data shape without a backup.
4. **Real user data** → needs real auth + privacy care. → STOP and flag before storing or exposing personal data loosely.
5. **Production deploy** → requires explicit human "yes," every time. Preview deploys are free; production is gated. → STOP before deploying to real users.
6. **Cost** → state, in plain English, before wiring up anything that costs money per use.
7. **Out of depth** (payments, security, hard-to-undo data decisions) → STOP and recommend a human engineer rather than guessing.

**Git habit:** build on a feature branch, test its preview URL, only merge to `main` when verified. `main` is always deployable.

**"Done" ≠ "works." "Committed" ≠ "live."** Confirm on the actual running app / production URL before reporting success.

## Project memory (keep this updated)

Short notes that should survive across sessions and tools. What's been built, decisions made, things that broke and why.

- _<e.g. 2026-05-29: chose Supabase Auth for login>_
- _<e.g. 2026-05-30: payment flow deferred — needs an engineer>_

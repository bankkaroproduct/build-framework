# AGENTS.md — operating rules for this project

> Many AI coding tools read this file automatically (Codex and others). If yours doesn't, paste it into the session at the start. It tells any AI working in this folder how to behave. Keep it at the project root. Fill in the blanks at the top.

## This project

- **Name:** _<fill in>_
- **What it is (1–2 sentences):** _<fill in — what it does, who uses it>_
- **Real-data or Demo:** _<Real — full rigor | Demo — throwaway data>_  (Real if it touches real data, real company infra, or real external APIs — even with no public users yet. If unsure → Real.)
- **Hosting:** _<e.g. Vercel>_
- **Database:** _<e.g. Supabase / Neon / none>_

## How we work here (non-negotiable)

This project follows the Build Framework. The human directing this work is **non-technical** — explain everything in plain English, never assume diffs or jargon are understood.

**The loop:** Classify → Plan → Build → Verify → Ship-behind-a-gate. Plan before building. Verify with eyes on the running app, not by claiming "done."

**Build the simplest thing that works.** Reuse existing code / stdlib / native features before adding a dependency. No speculative "might need it later" code. Mark any shortcut with a visible `TODO`. Never reference an API, config key, or function you haven't confirmed is real — verify, don't guess. (But never cut validation, security, or accessibility.) Apply the quality lenses in `references/09-quality-lenses.md` before calling a user-facing feature done.

**Always-on safety rails — these are STOP signs. When about to cross one, pause and ask in plain English:**

1. **Secrets** (keys, passwords, tokens, DB URLs) → environment variables only. NEVER in committed files or browser-visible code. → STOP before writing a secret anywhere but local `.env`.
2. **External APIs** → APIs we don't own are read-only; NEVER create/update/delete on a system we don't own without sign-off. Writing to services we DO own is fine via sandbox-keys-first + sign-off-before-live. → STOP before any write to an external API.
3. **Preview ≠ production data** → preview/test environments use a non-prod database + sandbox keys. → STOP before pointing preview at the production DB or live keys.
4. **Database** → back up before any migration / destructive / delete operation. → STOP before altering data shape without a backup.
5. **Real user data** → needs real auth + privacy care; one user must never see another's data; permissions checked server-side. → STOP and flag before storing or exposing personal data loosely.
6. **Production deploy** → requires explicit human "yes," every time. Preview deploys are free; production is gated. → STOP before deploying to real users.
7. **Cost** → state, in plain English, before wiring up anything that costs money per use; assume a hard cap should exist.
8. **New dependencies** → justify before adding a package (needed? maintained? touches secrets/data?). → STOP before casually installing packages.
9. **Out of depth** (payments, security, hard-to-undo data decisions) → STOP and recommend a human engineer rather than guessing.

**Git habit:** build on a feature branch, test its preview URL, only merge to your **production branch (usually `main`)** when verified. That branch is always deployable. (Confirm how this project actually deploys — see `references/03-verification-shipping.md`.)

**Explain in plain English first**, then leave a short technical trail (files changed, commands run + results, deps added, env vars added, migrations, known risks) so a future engineer isn't doing archaeology.

**"Done" ≠ "works." "Committed" ≠ "live."** Confirm on the actual running app / production URL before reporting success. Judge checks by **regressions** (did anything that worked before stop working?), not by hitting an exact test count.

## Project memory (keep this updated)

Short notes that should survive across sessions and tools. What's been built and key decisions:

- _<e.g. 2026-05-29: chose Supabase Auth for login>_

### Known risks & deferred shortcuts
Corners cut "for now" and risks we're carrying — so they're visible, not forgotten:

- _<e.g. 2026-05-30: payment flow deferred — needs an engineer>_
- _<e.g. no rate limiting on the contact form yet>_

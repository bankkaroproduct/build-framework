# Worked Example — a real loop, start to finish

A filled-in handoff and the review it produced, so you can see what "good" looks like. The feature: **let users reset their password.** (App is Real-data — it has logins.)

---

## The filled handoff (Planner → Builder)

```yaml
role: builder
work_id: password-reset-v1
branch: feature/password-reset
base_branch: main
allowed_paths:
  - app/auth/**
  - components/auth/**
  - lib/email/**
forbidden_paths:
  - app/payments/**
  - database/migrations/**
requires_human_stop:
  - secrets
  - production_deploy
  - dependency_add
  - external_api_write
verification_required: true
```

**Goal:** Let a logged-out user request a password-reset email and set a new password from the link.

**Smallest useful slice:** Just the reset-email + set-new-password flow. No "remember me", no account-recovery questions.

**Non-goals (not now):** Changing the existing login screen; rate-limiting the reset endpoint (noted as a known risk for later).

**Assumptions / open questions:** Assumes we use our existing email provider account (we own it). If there's no email provider wired up yet → STOP and ask.

**Scope — touch:** `app/auth/**`, `components/auth/**`, `lib/email/**`.
**Don't touch:** payments, database migrations. Anything else → ask.

**Environment / data:** Build + test on the **preview** environment with the **sandbox** email key and the **non-prod** database. Not production.

**Acceptance checks:**
- [ ] Logged-out user enters their email, gets a reset email.
- [ ] The link lets them set a new password and log in with it.
- [ ] An unknown email shows a neutral message (doesn't reveal whether the account exists) and doesn't crash.
- [ ] Reset link expires (can't be reused forever).
- [ ] Empty/loading/error states present; the "send reset email" button can't be spammed by double-clicking.

**STOP conditions:** standard A–H from the template. Note especially: sending email is an external write — sandbox key only, STOP before going live.

**Rollback plan:** Pure feature addition on a branch; if it misbehaves, revert the deploy to the previous version. No data migration, so nothing to restore.

**Report back:** plain English + technical appendix (files, commands run + results, deps added, env vars added, known risks).

---

## What the Builder reported back

> **Plain English:** Added a "Forgot password?" link on the login screen that opens a reset-request page. Submitting an email sends a reset link via our email provider (sandbox key). The link opens a "set new password" page; after setting it, the user is logged in. Unknown emails get a neutral "if that account exists, we've sent a link" message. Links expire after 1 hour. It's on the branch `feature/password-reset`, on the preview URL only — **not** on production.
>
> **Technical appendix:**
> - Files: `app/auth/reset-request/page.tsx`, `app/auth/reset/[token]/page.tsx`, `components/auth/ResetForm.tsx`, `lib/email/sendReset.ts`
> - Commands run: `npm run build` → passed; `npm run lint` → passed; `npm test` → 4 passed (added 2 reset-flow tests)
> - Dependencies added: none (used the existing email + auth libraries — STOP G not triggered)
> - Env vars added: `EMAIL_API_KEY` (sandbox value in preview; name added to `.env.example`)
> - Migrations: none
> - Known risks: no rate-limiting on the reset endpoint yet (deferred per non-goals) — logged in AGENTS.md known-risks.

---

## The review it produced (different AI, Reviewer role)

**1. What I checked:** the four files in scope, the reset token expiry logic, the "unknown email" path, and the env-var handling.

**2. What I could NOT check:** I didn't receive live access, so I read the diff and ran the build/tests; I did not click through the live preview myself.

**3. Commands/tests I ran:** `npm run build` (passed), `npm test` (4 passed).

**4. Files changed unexpectedly:** none — all within `allowed_paths`.

**5. Risks, worst-first:**
- **Medium:** No rate-limiting on the reset-request endpoint — someone could trigger many emails to a victim's address. Acknowledged as deferred, but flag before public launch.
- **Low:** Confirm the reset token is single-use (expiry is set; also invalidate on use).
- **Low:** Ensure `EMAIL_API_KEY` is set in production env vars before this ships, or production will silently fail to send.

**6. Go / No-Go:** **Ship-with-fixes** to preview (verify single-use token). Because this is medium-risk (auth) and I did not run it live, **No-Go to production until a human clicks through the live reset flow end-to-end.**

---

## What the human (you) does next

1. Open the preview URL, actually request a reset, get the email, set a new password, log in. (Verify — the No-Go stands until you do.)
2. Confirm the token is single-use (the one fix).
3. Make sure `EMAIL_API_KEY` is set in production env vars.
4. Only then: "yes, deploy to production."

Notice: a real risk (no rate-limiting) got surfaced and consciously deferred — written down, not forgotten — and production was gated on a human actually using the feature. That's the whole framework in one feature.

---

## The same feature done *wrong* (and how the framework catches it)

Contrast teaches faster than the happy path. Here's the careless version — and the gate that stops each mistake.

**The vague handoff:** *"Add password reset."* No scope, no environment, no acceptance checks, no STOP conditions.
→ **Caught by:** the handoff template itself. With no `allowed_paths` and no acceptance checks, the Builder has nothing to obey and you have nothing to verify against. A one-line handoff is the first smell.

**Too-wide scope:** the Builder, unbounded, also "improves" the login screen, swaps the session library, and reformats ten files.
→ **Caught by:** `forbidden_paths` + the "Don't touch" scope. Edits outside the agreed paths trip STOP D. The dependency swap trips STOP G (justify before adding/changing a package).

**Secret in code:** it hardcodes the email API key in `sendReset.ts` to "just get it working."
→ **Caught by:** Rail 1 / STOP A — and, if you set it up, the secret-scan check (`07-harden-your-repo.md`) *blocks the push outright*. This is exactly why the automatic guard exists: a non-engineer wouldn't spot the key in the diff.

**Preview wired to prod:** to "test with real data," it points the preview at the production database. A test run deletes real accounts.
→ **Caught by:** Rail 3 / STOP H — preview must use a non-prod database. This is the trap that silently undoes everything; it's a hard STOP for a reason.

**Missing prod env var:** works on preview; the email key was never added to production env vars, so live resets silently fail.
→ **Caught by:** the pre-production checklist ("env vars production needs are set"). The Reviewer also flags it.

**Rubber-stamp review:** the *same* session that built it reviews it and says "looks good, ship it."
→ **Caught by:** the review rules — review must be a different/cold-start session, must list what it checked and couldn't, and **defaults to No-Go** for a risky change nobody actually ran. "Looks good" with no evidence is not a review.

**Shipped on AI's say-so:** nobody opened the running app; "done" was taken at face value and it went to production.
→ **Caught by:** "Done ≠ works," the human Verify gate, and the explicit production "yes." The one check no tool replaces is you, clicking through the live thing.

Every one of these is a normal thing an eager AI (or a rushed human) does. The framework's value is that each has a specific gate in front of it — so the careless version simply can't reach real users without someone tripping a STOP.

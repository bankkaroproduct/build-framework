# Handoff — <short title>

> The Planner fills this in and gives it to the Builder. Any coding tool can read it. Keep it short and concrete. Delete these quote-lines before use.

## Goal
<One or two sentences: what to build and why. Plain English.>

## Smallest useful slice
<The minimum that's worth shipping. Not the whole dream — the next working piece.>

## Non-goals (explicitly NOT doing now)
- <things a reader might assume are included but aren't — prevents scope creep>

## Assumptions / open questions
- <anything the Planner assumed; anything the Builder should confirm before starting. If an assumption is wrong, STOP and ask.>

## Scope

**Touch (allowed):**
- <files / areas the Builder may change>

**Don't touch (off-limits):**
- <files / areas to leave alone — e.g. auth, payment code, database schema>
- Anything not listed under "Touch" → ask first.

## Environment / data this touches
- <which environment (local / preview / prod) and which database/API keys. Preview must use non-prod DB + sandbox keys.>

## Acceptance checks (how we'll know it worked)
- [ ] <concrete, observable — e.g. "clicking Save stores the note and it survives a refresh">
- [ ] <a failure case behaves gracefully — e.g. "empty form shows an error, doesn't crash">
- [ ] For any user-facing feature: it has sensible **empty, loading, and error** states, and **destructive actions ask for confirmation**.

## STOP conditions (pause and ask the human)
- **A** — about to put a secret anywhere but local `.env` → STOP
- **B** — about to write to / change an API or service we don't own → STOP
- **C** — about to alter the database shape or delete data without a backup → STOP
- **D** — about to work outside the "Touch" scope above → STOP, report, ask
- **E** — about to deploy to production → STOP, get explicit "yes"
- **F** — hit something risky / out of depth (security, payments, irreversible) → STOP, recommend an engineer
- **G** — about to add a new software package/dependency → STOP, justify it first (do we need it? widely used + maintained? could it touch secrets/data? is there a built-in alternative?)
- **H** — about to point a preview/test environment at the production database or live API keys → STOP

## Rollback plan (required if this changes data, auth, or an integration)
- <how to undo this if it goes wrong — e.g. "revert the deploy to the previous version; no data migration so nothing to restore">

## How to verify
<Exact steps to prove it works on the preview URL — what to click / call, what to expect.>

## Report back
When done, summarize **in plain English first**:
- What you actually changed (behavior, in plain words)
- Anything you skipped or couldn't do
- Whether it's on a branch only, or merged — and confirm it is NOT yet on production unless explicitly told
- Any STOP condition you hit

Then a short **technical appendix** (so a future engineer isn't doing archaeology):
- Files changed
- Exact commands run + their results (build/lint/tests — paste the outcome, don't just say "passed")
- Dependencies added (and why)
- Environment variables added (names only)
- Database migrations created
- Known risks / corners cut

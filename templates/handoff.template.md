# Handoff — <short title>

> The Planner fills this in and gives it to the Builder. Any coding tool can read it. Keep it short and concrete. Delete these quote-lines before use.

## Goal
<One or two sentences: what to build and why. Plain English.>

## Smallest useful slice
<The minimum that's worth shipping. Not the whole dream — the next working piece.>

## Scope

**Touch (allowed):**
- <files / areas the Builder may change>

**Don't touch (off-limits):**
- <files / areas to leave alone — e.g. auth, payment code, database schema>
- Anything not listed under "Touch" → ask first.

## Acceptance checks (how we'll know it worked)
- [ ] <concrete, observable — e.g. "clicking Save stores the note and it survives a refresh">
- [ ] <a failure case behaves gracefully — e.g. "empty form shows an error, doesn't crash">

## STOP conditions (pause and ask the human)
- **A** — about to put a secret anywhere but local `.env` → STOP
- **B** — about to write to / change an API or service we don't own → STOP
- **C** — about to alter the database shape or delete data without a backup → STOP
- **D** — about to work outside the "Touch" scope above → STOP, report, ask
- **E** — about to deploy to production → STOP, get explicit "yes"
- **F** — hit something risky / out of depth (security, payments, irreversible) → STOP, recommend an engineer

## How to verify
<Exact steps to prove it works on the preview URL — what to click / call, what to expect.>

## Report back
When done, summarize in plain English:
- What you actually changed (files / behavior)
- Anything you skipped or couldn't do
- Whether it's on a branch only, or merged — and confirm it is NOT yet on production unless explicitly told
- Any STOP condition you hit

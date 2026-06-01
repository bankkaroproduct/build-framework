# Review Request — <short title>

> Fill this in and give it to a **different** AI than the one that built the change (e.g. ChatGPT with GitHub access, if a coding tool was the Builder). The point is fresh, independent eyes. Delete these quote-lines before use.

## What changed
- **Where to look:** <branch name / pull request link / files>
- **What it's supposed to do:** <plain English>

## What I want you to check (be critical, not polite)
- [ ] Does it actually do what it's supposed to? Any obvious gaps?
- [ ] **Secrets:** are any keys/passwords/tokens sitting in code or committed files instead of env vars?
- [ ] **External APIs:** does it write to (change/create/delete) any API we don't own? It shouldn't, without sign-off.
- [ ] **Data safety:** any destructive database operation without a backup? Any personal/user data handled loosely?
- [ ] **Failure cases:** what happens with bad input, an empty form, a network error?
- [ ] **Anything the builder might be blind to** — things that look fine but bite later.

## What I'm specifically worried about
<Optional: name your concern, e.g. "this touches login" or "it calls a paid API in a loop".>

## How to respond (required sections — a review without these is theater)
Don't say "looks good." Structure your answer as:

1. **What I checked** — the files/flows/claims you actually looked at.
2. **What I could NOT check** — limits of this review (couldn't run it, no access to X, etc.). Be honest; unchecked ≠ safe.
3. **Commands / tests I ran** (if any) — and their real results.
4. **Files changed unexpectedly** — anything touched beyond what the change description implied.
5. **Risks, worst-first** — each with *where* it is and *why* it matters. Call out anything that should go to a human engineer.
6. **Go / No-Go** — your recommendation: safe to ship, ship-with-fixes, or do-not-ship.

If you genuinely find nothing serious, still fill in sections 1–4 so it's clear the review was real.

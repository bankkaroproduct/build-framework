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

## How to respond
Don't say "looks good." List concrete risks, ranked worst-first, each with where it is and why it matters. If you genuinely find nothing serious, say what you checked so I know the review was real.

# 06 — Existing Project Mode

Most of this framework is written for starting fresh. But you'll often point an AI tool at a project that **already exists** — one you built earlier, one a teammate started, or a codebase you inherited. The single biggest mistake here is letting the AI change things before it (or you) understands what's there.

**The rule: understand before you touch. Never let an AI refactor, upgrade, or "clean up" a project it just opened.**

---

## Why this needs its own mode

A fresh project has no surprises. An existing one does: it has a particular stack, a deploy process, environment variables it depends on, data it can't lose, and "load-bearing" code that looks ugly but is doing something important. An AI that starts editing on minute one — confidently, because AIs are always confident — can break a working app while "improving" it. The most expensive incidents come from acting before understanding.

---

## The understand-first checklist

When you open an existing project with an AI, have it do this **first**, and report back in plain English, before any edit:

- [ ] **What is this?** Read the README and any docs. What does the app do?
- [ ] **What's the stack?** Language, framework, key libraries. (You don't need to understand it — you need it written down.)
- [ ] **Where does it live?** Hosting provider, and what's the deploy process (push to main? manual? preview URLs)?
- [ ] **What's the database?** Which one, and does it hold real data?
- [ ] **What environment variables does it need?** (Look for `.env.example` or a config doc.) These are the secrets/settings the app can't run without.
- [ ] **What branch is "live"?** What's the current branch, and which branch is production?
- [ ] **How do you run and test it?** The commands to start it locally and run its checks.
- [ ] **What looks risky or load-bearing?** Auth, payments, data migrations, anything the AI itself flags as "be careful here."
- [ ] **Is there an `AGENTS.md`?** If not, create one (use the template) so this and every future session starts informed.

Only after this report do you let the AI plan a change.

---

## The "don't" list for existing projects

- **Don't refactor before understanding.** "This code is messy, let me clean it up" is how working apps break. Refactor only with a clear reason and the normal Plan → Build → Verify loop.
- **Don't upgrade dependencies casually.** "Let me update these packages" can break the build in non-obvious ways. Treat any upgrade as its own scoped, verified change.
- **Don't touch the database structure** until you know what data is real and whether there's a backup (Rail 4).
- **Don't assume the deploy is safe.** Confirm whether merging to `main` ships to real users *before* you merge anything.
- **Don't reformat the whole codebase.** Mass formatting changes bury the actual change and make review impossible.

---

## Then: normal loop, smaller slices

Once you understand the project, it's the same loop as everything else — Classify → Plan → Build → Verify → Ship-behind-a-gate (`FRAMEWORK.md`) — just with **smaller slices** than greenfield. In an unfamiliar codebase, change one small thing, verify it, and only then go further. Each verified small step teaches you more about how the app behaves.

And write what you learn into `AGENTS.md`'s memory section as you go. The next session — yours or a teammate's, in any tool — starts from your notes instead of re-discovering everything.

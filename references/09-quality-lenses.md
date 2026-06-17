# 09 — Quality Lenses (UI · UX · Copy · Code)

Safety keeps you out of trouble. **Quality is whether the thing is actually good.** These are four short checklists — "lenses" — you (or your Reviewer) hold up to a feature before shipping. Each points to the canonical deeper source if you want more.

They work the same way as everything else here: hand the relevant lens to your AI and ask *"check this against these points and tell me where it falls short."* AI builds tend to fail these in predictable ways, so a quick pass catches most of it.

> Not included: a roadmap/prioritization lens. Deciding *what* to build is product work you likely already own — this framework is about building it *well and safely*, not about product strategy.

---

## UI lens — does it look intentional, not AI-generated?

AI-built UIs have a recognizable "slop" look. Check:

- [ ] **Hierarchy comes from size/weight/spacing/color — not a border around everything.** One clear focal point per screen.
- [ ] **One font (maybe two). One consistent spacing scale.** Not five font sizes picked at random.
- [ ] **A real, restrained palette** — one primary colour + neutrals. Not many competing colours or a purple gradient.
- [ ] **No AI-slop tells:** card-inside-a-card nesting, gray text on a coloured background, emoji as bullet points, everything-rounded-and-shadowed, dead-center-stacked everything.
- [ ] **Enough whitespace.** Cramped = amateur; generous spacing = considered.
- [ ] Looks right on a phone, not just desktop.

Deeper source: **Refactoring UI** (Adam Wathan & Steve Schoger) · the `impeccable` ruleset (anti-AI-slop rules).

---

## UX lens — can a real person actually use it? (Nielsen's heuristics, condensed)

- [ ] **System status is visible** — loading, saved, error, empty states all exist (not a frozen screen).
- [ ] **Speaks the user's language** — no internal jargon or DB field names on screen.
- [ ] **User stays in control** — there's a back/cancel/undo; nothing traps them.
- [ ] **Consistent** — the same action looks and behaves the same everywhere.
- [ ] **Prevents errors** — destructive actions confirm; bad input is caught before submit; double-clicks don't double-submit.
- [ ] **Recognition over recall** — don't make users remember things between screens.
- [ ] **Errors are recoverable** — messages say what happened *and* how to fix it.
- [ ] **No clutter** — every element earns its place.

Deeper source: **Nielsen Norman Group — 10 Usability Heuristics for User Interface Design.**

---

## Copy lens — does the writing help, or get in the way?

- [ ] **Clear beats clever.** Plain words a stranger understands instantly.
- [ ] **Buttons name the action:** "Save changes", "Send reset link" — not "Submit" / "OK".
- [ ] **Error messages: what happened + how to fix, never blame the user.** "We couldn't find that email" not "Invalid input".
- [ ] **No jargon, no internal terms, no acronyms** the user wouldn't know.
- [ ] **Consistent voice; sentence case; short sentences.**
- [ ] **Front-load the meaningful word** — people scan, they don't read.

Deeper source: NN/g **"Writing for the Web"** · plain-language / microcopy principles.

---

## Code-quality lens — is it the simplest thing that works?

(Distilled from the `ponytail` ruleset — see "companion" below.)

- [ ] **Simplest thing that works.** Delete before you add. The best code is code you didn't write.
- [ ] **Reuse before adding** — existing code, the language's standard library, or a native platform feature before a new dependency (and any new dependency is justified — `02-orchestration.md` STOP G).
- [ ] **No speculative code** — don't build for a "might need it later" that isn't here yet.
- [ ] **Shortcuts are marked**, not hidden — a visible `TODO` with the proper fix noted, so debt is trackable (see your `AGENTS.md` known-risks).
- [ ] **Nothing real is invented.** The AI must not reference an API endpoint, config key, environment variable, or library function it hasn't confirmed exists. Hallucinated APIs/config are a common AI failure — verify, don't assume.
- [ ] **But never cut the essentials** — validation, error handling, security, and accessibility are not "extra code"; minimalism never applies to them.

**Companion ruleset:** [`ponytail`](https://github.com/DietrichGebert/ponytail) — a fuller "write less code" deliberation ladder for AI agents. Composes cleanly with this framework: ponytail governs code restraint, this framework governs safety + process. Optional, but a good add if your AI tends to over-engineer.

Deeper source: ponytail · YAGNI ("You Aren't Gonna Need It").

# 04 — Safety Rails

The non-negotiables. These apply to every project, every session, whether or not you remember to ask. If your `AGENTS.md` is in place, your tools already enforce them.

Each rail below has: the rule, why it exists, and the STOP sign (when to pause).

---

## Rail 1: Secrets never in code or Git

**Rule:** API keys, passwords, tokens, database URLs → environment variables only. Never in a committed file. Never in code the browser downloads.

**Why:** Anything in Git is effectively permanent and often discoverable. Leaked keys get found by automated bots within minutes and abused — drained API credits, stolen data, hijacked accounts. This is the #1 way prototypes get burned.

**STOP sign:** An AI is about to type a real key into any file other than your local `.env`, or into front-end/client code. Pause. "Is this a secret? Then it goes in env vars, and I read it from there."

**Practical:** Use `.env` locally (git-ignored), set the same values in your hosting provider's environment-variables dashboard for preview and production separately. Keep a `.env.example` with names only.

---

## Rail 2: External APIs — ownership decides

**Rule:** The dividing line is **ownership**, not read-vs-write.
- APIs you **don't own** (another product's data, someone else's system, where you're a guest): **read-only by default.** Never create/update/delete there without explicit human sign-off.
- APIs you **do own** (your own email provider, your own payment account, your own CRM): writing is legitimate — many real features need it (send email, create a payment, update a record). But do it through the **safe-write checklist** below.

**Why:** You can't see the blast radius of writing to a system you don't control — a wrong write to someone else's production system corrupts real data and causes an incident on *their* side. Writing to your own services is fine; it just needs care because writes are irreversible in a way reads aren't.

**Safe-write checklist** (for writing to a service *you* own):
- [ ] Sandbox / test account and keys first — never live credentials during development
- [ ] Scoped API key (only the permissions this feature needs)
- [ ] Idempotency key where the service supports it (so a retry doesn't double-charge / double-send)
- [ ] Rate limited (no unbounded loops)
- [ ] A dry-run / test mode used before real writes
- [ ] Sample payload reviewed in plain English ("this will send an email to X saying Y")
- [ ] Explicit human sign-off before switching from sandbox to live credentials

**STOP sign:** About to write (POST/PUT/PATCH/DELETE) to ANY external API → confirm you own it; if not, stop. If you do own it, confirm the safe-write checklist.

**Also:** handle failures gracefully (external APIs go down) and respect rate limits.

---

## Rail 3: Preview must never touch production data

**Rule:** A preview / test deploy is only safe if it points at a **non-production database** and **sandbox API keys / test credentials**. Production data and production write-APIs stay out of preview.

**Why:** This is the trap that quietly undoes every other rail. You think you're "just testing on the preview URL" — but if that preview is wired to your real database, a test that deletes records deletes *real* records. A test that calls a live payment API charges *real* cards. The preview's whole purpose (a safe place to break things) is gone the moment it shares production's data.

**STOP sign:** A preview/staging environment is about to be configured with the production database URL or live API keys. Pause. Preview gets its own non-prod database and sandbox keys. If a preview genuinely must use real data for a specific test, it is no longer a "safe" preview — treat that session as Real/Production-risk and get explicit sign-off.

**Practical:** In your hosting dashboard, Preview and Production have separate environment-variable sets. Point Preview at a separate (non-prod) database and sandbox keys. Never copy the production database URL into Preview.

---

## Rail 4: Back up before you change data's shape

**Rule:** Before any database "migration," "drop," "reset," or anything that alters or deletes existing data → take a backup first.

**Why:** Deleted or corrupted data is usually gone for good. A 30-second backup is cheap insurance against an irreversible mistake.

**STOP sign:** An AI is about to run a migration or destructive database command on a database with real data and hasn't mentioned a backup. Pause: "back up first, then proceed."

**Practical:** Managed databases (Supabase, Neon, etc.) have one-click backups/snapshots. Take one before structural changes.

---

## Rail 5: Real user data = auth + privacy hygiene

**Rule:** If real people log in or you store anything personal (names, emails, anything identifying), the app needs real authentication and you must handle that data carefully.

**Why:** Real user data is a responsibility and often a legal one. Loose handling (no login, personal data exposed, data sent somewhere it shouldn't go) is how prototypes become liabilities.

**STOP sign:** An AI is about to store personal data without protection, expose it to users who shouldn't see it, or send it to a third party. It should flag this to you in plain English before doing it.

**Practical:** Use a managed auth provider (Clerk, Auth0, Supabase Auth, etc.) rather than rolling your own login. Don't log personal data into places it doesn't belong. If unsure whether something is "personal data," treat it as if it is.

### Security baseline you can check without being an engineer
Ask your AI each of these in plain English and make it show you the answer — these are the things that actually bite real-data apps:
- [ ] **Can User A see User B's data?** (The #1 real-world bug. Every "get my data" request must check *who's asking* on the server.)
- [ ] Are admin-only pages/actions actually protected, not just hidden from the menu?
- [ ] Are permissions checked **on the server**, not only in the browser? (Browser checks are easily bypassed.)
- [ ] Are secrets used only server-side, never shipped to the browser?
- [ ] Are user inputs validated (so a form can't submit junk that breaks things)?
- [ ] Are file uploads (if any) restricted by type and size?
- [ ] Is personal data kept out of logs?
- [ ] Does the app hide raw error details from users (no stack traces on screen)?
- [ ] If a user asks, can you delete or export their data?

If you can't get a clear "yes" (or a clear plan) for the first three especially, that's a Rail 8 "call an engineer" moment.

### A few data-model questions worth asking *before* the first real build
The shape of your data is one of the hardest things to change later. Before building, get plain-English answers:
- What are the core things the app stores (the main "nouns")?
- Which fields are personal/sensitive (PII)?
- What should the app **never** store?
- What would be painful to change after launch?

A two-line answer now saves a rebuild later. If the model feels genuinely consequential, that's a Rail 8 escalation.

---

## Rail 6: Production deploy = explicit human "yes"

**Rule:** Nothing reaches real users without you explicitly saying "deploy to production," after verification passed.

**Why:** Automatic production deploys mean a bug ships to real users before any human looked. The gate is the whole point.

**STOP sign:** Any tool about to push to production on its own. Pause, get the explicit go.

---

## Rail 7: Cost control

**Rule:** Know what spends money before you turn it on. Set a hard spending cap and a budget alert at setup.

**Why:** Cloud services, APIs, and AI usage can run up surprising bills — a runaway loop calling a paid API, a database tier that auto-scales, an unbounded background job, over-frequent scheduled jobs, expensive logging. AI-built apps hit these accidentally.

**STOP sign:** An AI is about to wire up anything metered (a paid API, a usage-based service, a scheduled job that runs forever) without telling you the cost shape. It should say, in plain English, "this costs money per X."

**Practical (do these, don't just "be aware"):**
- Set a hard spending cap + budget alert in each provider's billing settings at setup.
- Watch for the classic runaways: loops that call a paid API without a limit, cron/scheduled jobs running far more often than needed, logging that writes huge volumes.
- Prefer free tiers for demos.
- "This costs money per call" is good; "we capped it at ₹X/day" is better.

---

## Rail 8: Know when to call a human engineer

**Rule:** Some things are too risky to wing. When you hit one, the right move is "this needs a real engineer," not "let's try and see."

**Why:** Mistakes in these areas are expensive, irreversible, or legally serious. No amount of AI confidence substitutes for human expertise here.

**Call an engineer when:**
- Handling **payments** or money movement
- Anything where a **security** mistake is serious (storing sensitive data, building login from scratch, permissions for who-can-see-what)
- A **data model** decision that's hard to change later (how your core data is structured)
- An **incident** with real users affected that a rollback didn't fix
- Anytime your gut says "I don't actually understand what this is doing and it matters"

See `05-incident-escalation.md`.

---

## The one-line summary

**Secrets out of code, don't write to systems you don't own, keep preview off production data, back up before you break things, protect real people's data, gate production, watch the bill, and know when you're out of your depth.**

(And one build-time habit: don't let the AI add software packages without justifying each one — see `02-orchestration.md`.)

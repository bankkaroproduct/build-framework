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

## Rail 2: External APIs are "look, don't touch"

**Rule:** When your app consumes someone else's API (another product's data, a third-party service) **as a client**, default to **read-only**. Never create, update, or delete data on an API you don't own without an explicit human "yes."

**Why:** You can't see the blast radius of writing to a system you don't control. A wrong write to a production system you're a guest on can corrupt real data and cause an incident on *their* side — far outside your app. Reading is safe; writing is not.

**STOP sign:** A handoff or AI is about to POST/PUT/PATCH/DELETE to an external API (anything that isn't a plain GET/read). Pause, confirm you actually own that API or have explicit sign-off.

**Also:** handle failures gracefully (the external API will be down sometimes), and respect rate limits (don't hammer it in a loop — you can get blocked or rack up costs).

---

## Rail 3: Back up before you change data's shape

**Rule:** Before any database "migration," "drop," "reset," or anything that alters or deletes existing data → take a backup first.

**Why:** Deleted or corrupted data is usually gone for good. A 30-second backup is cheap insurance against an irreversible mistake.

**STOP sign:** An AI is about to run a migration or destructive database command on a database with real data and hasn't mentioned a backup. Pause: "back up first, then proceed."

**Practical:** Managed databases (Supabase, Neon, etc.) have one-click backups/snapshots. Take one before structural changes.

---

## Rail 4: Real user data = auth + privacy hygiene

**Rule:** If real people log in or you store anything personal (names, emails, anything identifying), the app needs real authentication and you must handle that data carefully.

**Why:** Real user data is a responsibility and often a legal one. Loose handling (no login, personal data exposed, data sent somewhere it shouldn't go) is how prototypes become liabilities.

**STOP sign:** An AI is about to store personal data without protection, expose it to users who shouldn't see it, or send it to a third party. It should flag this to you in plain English before doing it.

**Practical:** Use a managed auth provider (Clerk, Auth0, Supabase Auth, etc.) rather than rolling your own login. Don't log personal data into places it doesn't belong. If unsure whether something is "personal data," treat it as if it is.

---

## Rail 5: Production deploy = explicit human "yes"

**Rule:** Nothing reaches real users without you explicitly saying "deploy to production," after verification passed.

**Why:** Automatic production deploys mean a bug ships to real users before any human looked. The gate is the whole point.

**STOP sign:** Any tool about to push to production on its own. Pause, get the explicit go.

---

## Rail 6: Cost control

**Rule:** Know what spends money before you turn it on. Set spending limits where you can.

**Why:** Cloud services, APIs, and AI usage can run up surprising bills — a runaway loop calling a paid API, a database tier that auto-scales, an unbounded background job. Prototypers get surprise invoices this way.

**STOP sign:** An AI is about to wire up anything metered (a paid API, a usage-based service, a cron job that runs forever) without telling you the cost shape. It should say, in plain English, "this costs money per X."

**Practical:** Most providers let you set hard spending caps and budget alerts. Turn them on at setup. Prefer free tiers for demos.

---

## Rail 7: Know when to call a human engineer

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

## The one-line summary of all seven

**Secrets out of code, don't write to others' systems, back up before you break things, protect real people's data, gate production, watch the bill, and know when you're out of your depth.**

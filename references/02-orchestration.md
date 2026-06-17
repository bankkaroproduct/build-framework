# 02 — Orchestration: directing the work

How to get good work out of AI coding tools without writing code yourself. This is the core skill. It's not about prompting tricks — it's about **writing clear instructions and checking the results.**

---

## The three roles, again

- **Planner** — figures out *what* to build, writes the handoff. (You + a thinking-partner AI.)
- **Builder** — does the actual file edits. (A coding tool.)
- **Reviewer** — independently checks. (A *different* AI.)

The reason to keep Builder and Reviewer separate: an AI that just wrote something is biased toward thinking it's correct. A fresh set of eyes — even another AI — catches what the first one missed. Use ChatGPT (with its GitHub connection) as your Reviewer while a coding tool is your Builder, or vice versa. Just don't let the same session that built it be the only one that checks it.

---

## Step 1: Plan before building

When you want something built, don't say "build X" and walk away. First, get the Planner to write it down:

- **Use "plan mode" if your tool has it.** Claude Code has a plan mode that researches and proposes before touching files. Use it.
- Ask for **the smallest useful slice**. Not "build the whole app" — "build just the login screen, working, nothing else." Small slices are easier to verify and easier to fix.
- Make the Planner state, in plain English: what it will change, anything that costs money, anything that touches user data, anything it's unsure about.
- **You approve the plan before code is written.** If the plan is vague, the code will be worse.
- **Challenge the plan before you approve it.** Every framework checks the code against the plan; almost none check the plan. Run a quick Devil's-Advocate pass — ideally in a *fresh/cold session* so it isn't invested in the idea — asking: is this the right thing to build? is there a simpler way? what assumption is this resting on, and what breaks if it's wrong? what's the riskiest part? It's the same cold-start adversarial trick we use for review (`TOOL_MATRIX.md`), pointed *upstream* at the idea instead of downstream at the code. Two minutes here saves building the wrong thing beautifully.

A plan you understood, challenged, and approved is worth ten clever prompts.

---

## Step 2: Write the handoff

The handoff is a short markdown file that tells the Builder exactly what to do. Template: `templates/handoff.template.md`. It has:

- **Goal** — one or two sentences. What and why.
- **Scope: touch / don't touch** — which files or areas are fair game, and which are off-limits. This stops the Builder from "helpfully" rewriting things you didn't ask about.
- **Acceptance checks** — how you'll both know it worked. Concrete: "the login button logs me in and I land on the dashboard."
- **STOP conditions** — when the Builder must pause and ask you instead of pushing ahead (see below).
- **How to verify** — the steps to prove it works.
- **Report back** — tell the Builder to summarize what it actually changed.

You can ask the Planner AI to *write* the handoff for you. You just check it reads right and approve it. Then hand it to the Builder.

Because the handoff is a plain file, you can hand the same one to any tool. That's what keeps you un-locked from any single vendor.

---

## Step 3: STOP conditions (your safety gates)

Put these in every handoff. The Builder must **pause and ask you** — not decide on its own — when it hits any of these:

- **A** — about to put a secret (key/password/token) anywhere except local `.env` → STOP
- **B** — about to write to / change / delete data in an API or service you don't own → STOP
- **C** — about to change the database's shape or delete data (a "migration"/"drop") without a backup → STOP
- **D** — about to work outside the agreed scope → STOP, report, ask
- **E** — about to deploy to production (real users) → STOP, get explicit "yes"
- **F** — hit something genuinely risky or out of depth (security, payments, data model) → STOP, recommend a human engineer
- **G** — about to add a new software package/dependency → STOP, justify first (see below)
- **H** — about to point a preview/test environment at the production database or live API keys → STOP

These are not suggestions. A STOP means: pause, explain in plain English, wait for your go.

### Why the dependency gate (G) matters
AI tools install software packages casually — and every package is new code your app now depends on, that could be unmaintained, bloated, or able to touch your secrets and data. Before adding one, make the Builder answer in plain English:
- Do we actually need it, or can the existing stack already do this?
- Is it widely used and actively maintained?
- Does it run on the server, in the browser, or just at build time?
- Could it access secrets or user data?

A "yes we need it, it's standard, it's maintained" is fine. Casually pulling in five obscure packages is not.

**Rules are dependencies too.** If you adopt a third-party AI ruleset (see `10-companion-rulesets.md`), apply the same gate — your AI obeys it as always-on instructions, so a compromised one can quietly steer the build. Read it, pin it to a commit, don't auto-update.

### Don't let the AI invent things that don't exist
A specific, common AI failure: it confidently uses an API endpoint, a config setting, an environment variable, or a library function that **doesn't actually exist** — it pattern-matched something plausible. The code looks right and breaks at runtime. Rule: the Builder must only reference things it has **confirmed are real** (checked the docs, the codebase, or the actual API). If your AI cites a method or endpoint you've never heard of, ask: *"have you verified that exists, or are you assuming?"* When in doubt, it should check, not guess.

---

## Step 4: Read the report — but don't trust it yet

The Builder reports what it did. Good — but a report of work is not proof of working software. The report tells you *where to look*. The actual checking happens in Verify (`03-verification-shipping.md`).

Two truths worth burning in:
- **"Committed" is not "live."** The Builder saving its work to a branch does not mean it's on your production site. Those are different steps.
- **"Done" is not "works."** Always confirm with your own eyes on the running app.

---

## Step 5: Send it to the Reviewer

Before anything important ships, fill in `templates/review-request.template.md` and give it to a *different* AI:
- What changed (point it at the branch / the files)
- What you're worried about (security? data? does it actually do the thing?)
- Ask for specific risks, not a pat on the back

A reviewer that only says "looks great" isn't reviewing. Push it: "what could go wrong here? what did the builder miss?"

---

## Running more than one builder at once

You usually won't — one task at a time is simpler and safer. But if you ever have two AI tools building in parallel (say Codex on one feature, Claude Code on another), follow these few rules or they'll overwrite each other:

- **One branch per builder.** Each builder works on its own feature branch, never the same one.
- **One folder per builder, too.** A separate branch isn't enough if two tools edit the **same project folder on your computer** at once — they'll overwrite each other's uncommitted work. Give each its own checkout (a separate clone of the repo, or a `git worktree`). Same folder + two tools = collision. If that sounds like a hassle, that's the signal to just run **one builder at a time** — which is the simpler, recommended default anyway.
- **Non-overlapping scope.** Their `allowed_paths` must not overlap. Two builders editing the same file at once = guaranteed conflict. If a file area is contested, only one builder owns it at a time.
- **One owner per area at a time.** Don't hand the same folder to a second tool until the first has committed and reported.
- **Commit before handing off.** Never leave uncommitted changes in a shared folder and then point another tool at it — it'll build on a moving target.
- **Merge one at a time, verify between.** Bring branches into `main` one by one, verifying after each. Don't merge two parallel branches together and hope.

If two builders do collide, stop both, keep the one that's verified, and re-run the other from a fresh branch off the updated `main`. Don't try to hand-merge two AI-generated tangles.

## A realistic example flow

1. You: "I want users to be able to reset their password."
2. Planner (plan mode): proposes the smallest slice — a reset-email flow — and flags that it touches user accounts and sends email (a cost + a data concern). You approve.
3. Planner writes a handoff: scope = auth + email files only; STOP conditions included; acceptance = "I get a reset email and can set a new password."
4. Builder executes, reports: "added reset route, email template, used env var for the email API key."
5. You Verify: actually trigger a reset on the preview URL, get the email, reset the password. Works.
6. Reviewer (different AI): checks the change — confirms the email key is in env, not code; confirms tokens expire. Flags nothing critical.
7. You: "yes, ship to production." Done.

Notice: you never read a line of code. You directed, gated, and verified.

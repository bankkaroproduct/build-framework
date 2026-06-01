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

A plan you understood and approved is worth ten clever prompts.

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

These are not suggestions. A STOP means: pause, explain in plain English, wait for your go.

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

## A realistic example flow

1. You: "I want users to be able to reset their password."
2. Planner (plan mode): proposes the smallest slice — a reset-email flow — and flags that it touches user accounts and sends email (a cost + a data concern). You approve.
3. Planner writes a handoff: scope = auth + email files only; STOP conditions included; acceptance = "I get a reset email and can set a new password."
4. Builder executes, reports: "added reset route, email template, used env var for the email API key."
5. You Verify: actually trigger a reset on the preview URL, get the email, reset the password. Works.
6. Reviewer (different AI): checks the change — confirms the email key is in env, not code; confirms tokens expire. Flags nothing critical.
7. You: "yes, ship to production." Done.

Notice: you never read a line of code. You directed, gated, and verified.

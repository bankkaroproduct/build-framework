# 03 — Verification & Shipping

The two steps that protect your real users. Most prototype disasters happen because someone skipped Verify or shipped to production without meaning to.

---

## Verify: "done" is not "works"

An AI saying "✅ done," a passing test, and a green checkmark are all **claims**. They are not proof. The only proof is the running software doing the thing in front of you.

### How to actually verify

**For anything with a screen (most apps):**
1. Open the **preview URL** (the test version, not production).
2. Do the exact thing that was built — click the button, submit the form, log in.
3. Try the obvious wrong thing too — submit an empty form, enter a bad password. It should fail gracefully, not crash.
4. If it touches data, check the data actually changed (or didn't, when it shouldn't).

**For a backend / API:**
1. Actually call the endpoint (your AI can do this for you — ask it to "hit the endpoint and show me the real response").
2. Look at the response with your own eyes. Does it contain what it should?
3. Check the failure case — what happens with bad input?

**Always:**
- Get the **Reviewer** (a different AI) to look independently before anything important ships.

### The "committed ≠ live" trap

When a Builder says "I committed the fix," that means it saved the work to a branch. It does **not** mean:
- it's merged into `main`, or
- it's deployed to your production site, or
- real users can see it.

Those are separate steps. Before you tell anyone "it's fixed/live," confirm it's actually on the URL real users hit. Ask your AI directly: "is this live on production right now, or just on a branch?" — and have it show you.

---

## Shipping: the production gate

Two kinds of deploy. Treat them completely differently.

### Preview deploys — ship freely
Every feature branch gets its own preview URL automatically (Vercel/Netlify do this). Nobody real uses it. Break things there all day. This is your testing ground.

### Production deploys — explicit "yes," every time
Production is the URL your real users hit. Rules:
- **Never automatic.** No tool ships to production on its own initiative.
- You say "yes, deploy to production" explicitly, *after* Verify passed on the preview.
- Production deploy usually happens by merging your tested branch into your production branch (commonly `main`), which triggers the hosting to update production — but confirm your actual trigger below.

### Know your actual deploy trigger ("main ≠ production" check)
The "merge to `main` → production updates" flow is the common default (Vercel/Netlify), but **don't assume it** — confirm how *your* project actually ships before you rely on it. Ask your AI, in plain English: *"When I merge to main, does that automatically deploy to real users, or is there another step?"* Some setups deploy from a different branch, need a manual "promote to production" click, or deploy by a separate process entirely. If you don't know your real trigger, you can ship by accident — or think you shipped when you didn't. Write the answer in `AGENTS.md` so every session knows it.

### The pre-production checklist
Before you say "yes, ship to production":

- [ ] I opened the preview URL and the new thing actually works
- [ ] I tried an obvious failure case and it didn't crash
- [ ] A Reviewer (different AI) looked at the change
- [ ] No secrets are in the code (they're in env vars)
- [ ] If it touches the database structure, there's a backup
- [ ] Environment variables that production needs are set in the hosting dashboard (a feature that works on preview but forgets a prod env var will break on production)
- [ ] If real user data is involved, login/permissions actually work
- [ ] **You'll find out if it breaks** — basic error tracking is installed (e.g. Sentry) so production errors reach you, not just the user; confirm the alerts actually land somewhere *you* see (email/Slack), not a dashboard nobody opens. And no personal data is written into logs.

The most common "works on preview, broken on production" cause is a **missing environment variable in production**. Preview and production have separate env var settings in your hosting dashboard. When you add a new secret, add it to both.

---

## Rolling back

If something breaks in production:
1. Don't panic-fix live. Roll back first.
2. Vercel/Netlify keep previous deployments — you can instantly "promote" the last good one back to production from the dashboard. That buys you time.
3. Then fix the problem on a branch, verify on preview, and ship again.

See `05-incident-escalation.md` for when it's worse than a quick rollback.

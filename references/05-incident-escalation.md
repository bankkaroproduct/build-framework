# 05 — Incidents & Escalation

What to do when something breaks, and — just as important — when to stop trying to fix it yourself and pull in a human engineer.

---

## When something breaks in production

Work in this order. Don't skip to "fix it" — that's how small problems become big ones.

### 1. Stabilize first (roll back)
- Don't debug on the live site while real users hit errors.
- Roll back to the last working version. On Vercel/Netlify, previous deployments are kept — promote the last good one back to production from the dashboard. This is instant and undoes the breakage.
- Now users are fine again and you have time to think.

### 2. Reproduce on preview
- Recreate the problem on a preview URL (not production).
- Get your AI to reproduce it and explain, in plain English, what's going wrong.

### 3. Fix on a branch, verify, re-ship
- Fix on a feature branch.
- Verify on the preview (`03-verification-shipping.md`).
- Only then merge to your production branch (usually `main`) for production.

### 4. Write down what happened
- In your `AGENTS.md` memory section, one line: what broke, why, what fixed it.
- This stops you (and your AI) from repeating it. Our biggest cross-session win is *not re-learning the same lesson.*

---

## When to STOP and get a human engineer

AI tools are confident even when they're wrong. Confidence is not competence. Some situations need a real engineer regardless of how sure the AI sounds:

| Situation | Why it needs a human |
|---|---|
| **Payments / money movement** | Mistakes lose real money and are legally serious. |
| **Security-sensitive work** | Login from scratch, permissions, storing sensitive data — subtle mistakes are invisible until exploited. |
| **A data-model decision that's hard to undo** | How your core data is structured. Cheap to get right now, expensive to change after launch. |
| **A production incident a rollback didn't fix** | If rolling back didn't restore service, it's beyond quick-fix territory. |
| **Data possibly lost or corrupted** | Get expert eyes before you make it worse with more changes. |
| **"I don't understand what this does and it matters"** | The honest signal. Trust it. |

**How to escalate well:** don't just say "it's broken." Bring: what you were trying to do, what the AI changed (the report/handoff), the error, and what you already tried (e.g. "rolled back, still broken"). That turns a 2-hour engineer investigation into a 15-minute one.

---

## The mindset

You can build a remarkable amount without an engineer. The skill isn't avoiding help — it's **knowing the line**. Shipping a working app: you, with AI. Deciding how to store payment data securely: a human engineer. Knowing which is which is the whole game.

When in doubt, the cheap move is to ask. The expensive move is to guess and find out in production.

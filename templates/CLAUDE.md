# CLAUDE.md

This project follows the Build Framework. **Read `AGENTS.md` in this project root and follow it exactly** — it contains the project details, the operating loop, and the non-negotiable safety rails.

Key reminders (full detail in `AGENTS.md`):
- The person you're working with is **non-technical**. Explain in plain English. Never assume code or jargon is understood.
- **Plan before building.** Use plan mode. Propose the smallest useful slice and get approval before editing files.
- **Safety rails are STOP signs:** secrets→env-vars-only, external-APIs→read-only, back-up-before-destructive-DB-ops, real-user-data→auth+privacy, production-deploy→explicit-human-yes, flag costs, escalate when out of depth.
- **"Done" ≠ "works."** Verify on the running app. **"Committed" ≠ "live."** Confirm production state before claiming success.
- Build on a feature branch, test its preview, merge to `main` only when verified.

If `AGENTS.md` is missing, something went wrong with project setup — stop and ask the human to run the Build Framework bootstrap.

# Bootstrap — starting a new project

Five steps. Do these once, at the very start of every new app, before you build features. They drop the framework's rules into your project so your AI tools follow them automatically.

> **Not comfortable in a terminal? You don't need to be.** Each step below has a *terminal* version and an *"ask your AI"* version. The "ask your AI" version is a prompt you paste to your coding tool and it does the typing for you. That's the whole spirit of this framework — you direct, the AI does the mechanics. Pick whichever you prefer; they do the same thing.

---

## 1. Create your project + repo
One app = one repo.

> **Ask your AI:** *"Create a new [type, e.g. Next.js] app called [name] and initialise a Git repository for it. Then connect it to GitHub. Walk me through anything you need me to click."*

## 2. Pull in the framework files
Get `AGENTS.md`, `CLAUDE.md`, and `env.example` into your new project's root.

**Terminal version** (if you're comfortable):
```
npx degit bankkaroproduct/build-framework/templates ./_framework-templates
# then move AGENTS.md, CLAUDE.md, env.example out of _framework-templates into your root
```

> **Ask your AI version:** *"Pull these three files from the GitHub repo `bankkaroproduct/build-framework` (in its `templates/` folder) into the root of this project: `AGENTS.md`, `CLAUDE.md`, and `env.example` (save the last one as `.env.example`). Don't change their contents."*

Or simplest of all: open the `build-framework` repo on GitHub and copy-paste the three files in by hand. Any of these is fine.

## 3. Fill in `AGENTS.md`
Open `AGENTS.md` and fill the top section: project name, what it is, **Real-data or Demo** (if unsure → Real), hosting, database. Thirty seconds. Now every AI session knows your project and your rules.

## 4. Lock down secrets
- Copy `.env.example` to `.env.local` and put your real values there (this file is never committed).
- Make sure your `.gitignore` contains `.env`, `.env.local`, `.env*.local`.
- Put real secret values in `.env.local` (never committed) and in your hosting provider's Environment Variables settings (Preview + Production separately).

> **Ask your AI:** *"Copy `.env.example` to `.env.local` for me to fill in. Make sure `.gitignore` ignores all `.env` files so I can never accidentally commit a secret. Then tell me, in plain English, which secret values I need to go get and where to paste them in my hosting dashboard."*

## 5. Write one paragraph of memory
In the "Project memory" section of `AGENTS.md`, write 2–3 sentences: what this app is and who uses it. Update it as you go. This is what saves you from re-explaining your project every session.

---

## You're set
Now follow the loop in `FRAMEWORK.md`: **Classify → Plan → Build → Verify → Ship-behind-a-gate.** Your tools already know the safety rails because they're in `AGENTS.md`.

When in doubt about any step, the detail lives in `references/`:
- `01-project-setup.md` — setup specifics
- `02-orchestration.md` — directing the AI, handoffs, reviewer loop
- `03-verification-shipping.md` — how to verify, the production gate
- `04-safety-rails.md` — the safety rails in full (incl. security baseline + safe-write checklist)
- `05-incident-escalation.md` — when things break, when to call an engineer
- `06-existing-project-mode.md` — applying the framework to a project that already exists

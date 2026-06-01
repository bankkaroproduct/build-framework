# Bootstrap — starting a new project

Five steps. Do these once, at the very start of every new app, before you build features. They drop the framework's rules into your project so your AI tools follow them automatically.

---

## 1. Create your project + repo
Create your new app and its Git repository as usual (your AI tool can do this). One app = one repo.

## 2. Pull in the framework files
From a terminal in your **new project's** root folder, copy the templates in. Pick whichever you have handy:

**If you have this framework cloned locally:**
```
cp /path/to/build-framework/templates/AGENTS.md ./AGENTS.md
cp /path/to/build-framework/templates/CLAUDE.md ./CLAUDE.md
cp /path/to/build-framework/templates/env.example ./.env.example
```

**Or pull just the templates straight from GitHub** (no full clone):
```
npx degit bankkaroproduct/build-framework/templates ./_framework-templates
# then move AGENTS.md, CLAUDE.md, env.example out of _framework-templates into your root
```

Or simplest of all: open the `build-framework` repo on GitHub and copy-paste the three files in by hand. Any of these is fine.

## 3. Fill in `AGENTS.md`
Open `AGENTS.md` and fill the top section: project name, what it is, **Real-data or Demo** (if unsure → Real), hosting, database. Thirty seconds. Now every AI session knows your project and your rules.

## 4. Lock down secrets
- Copy `.env.example` to `.env.local` and put your real values there (this file is never committed).
- Make sure your `.gitignore` contains:
  ```
  .env
  .env.local
  .env*.local
  ```
- Put real secret values in `.env.local` (never committed) and in your hosting provider's Environment Variables settings (Preview + Production separately).

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

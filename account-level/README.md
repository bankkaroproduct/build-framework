# Account-level install

`operating-rules.md` is the condensed, audience-neutral spine of the Build Framework — sized to govern an **entire account / every project**, not a single repo. It deliberately omits the "you're working with a non-engineer" framing so it's appropriate for technical users and autonomous executors too.

Three places to install it (do any or all):

## 1. Global Claude Code config
Put the contents of `operating-rules.md` into your global Claude config so every project inherits it:

```
~/.claude/CLAUDE.md
```

Claude Code reads this on every session. Project-level `CLAUDE.md` / `AGENTS.md` still layer on top for project specifics.

## 2. Global Codex config
Put the same contents into Codex's global instructions:

```
~/.codex/AGENTS.md
```

Codex reads this across projects. A project-level `AGENTS.md` still adds project specifics.

## 3. Online account / system instructions
Paste `operating-rules.md` into your Claude and/or Codex **account-level custom/system instructions** (the online setting that applies to all of your chats). It's intentionally short enough to fit such fields.

---

## Layering

- **Account level** (this file): universal operating discipline — how to work, the safety STOPs.
- **Project level** (`templates/AGENTS.md` from the repo): adds the specific project's rules, real-vs-demo classification, hosting/DB, and memory.
- **Task level** (`templates/handoff.template.md`): the contract for one specific piece of work.

Each layer narrows the one above it. Set the account level once; drop the project files in per project; write a handoff per task.

## Keep it in sync
If you change `operating-rules.md` here, re-paste/re-copy it into the three locations above. It's short by design so this is a 30-second update.

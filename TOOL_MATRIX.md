# Tool Matrix — how each AI tool plugs into the framework

The framework is **role-based**, so any tool can fill any role. But each tool *consumes the rules* a little differently. This is the cheat-sheet for wiring each one up. The two things that must always be true:

1. The tool can see **`AGENTS.md`** (the project's rules), and
2. For a specific task, the tool is given the **handoff** (with its role set).

---

## Using just one tool? (this is the common case — and it works)

You do **not** need two tools. Most people use one (just Claude Code, or just Codex). The entire framework works the same with one tool — the loop, the safety rails, the handoff, memory, the production gate are all single-tool. One tool simply plays **Planner and Builder** in sequence: plan first (use plan mode if it has one), then build from the handoff.

**The only role that wants a second opinion is the Reviewer — and you get it from the same tool with a cold start.** Don't have the session that just built the code review itself; in the same conversation it will defend its own work. Instead:

1. **Open a fresh conversation / new session** in the same tool (a "cold start" — no memory of building it).
2. Give it **only** the change to review (the branch/diff) **and** the filled `review-request.template.md`. Nothing about how or why it was built.
3. The cold context + the reviewer-mode template make it critique instead of cheerlead. It's not anchored on its own reasoning because, as far as this fresh session knows, it didn't write it.

This cold-start review recovers most of the value of a separate tool. And **you are always the final reviewer that no tool replaces** — the "verify on the running app" gate (`references/03-verification-shipping.md`) is yours regardless of how many tools you use.

> Cold starts are good for more than review — they're also how you handle a thread that's gone slow or forgetful. See `references/08-context-and-thread-management.md`.

---

## Per-tool wiring

| Tool | Usual role | How it gets the rules | How you give it a task |
|---|---|---|---|
| **Codex** (CLI / cloud) | Builder | Reads `AGENTS.md` at the repo root automatically | Point it at the filled `handoff.template.md` (set `role: builder`) |
| **Claude Code** | Planner or Builder | Reads `CLAUDE.md` → which points to `AGENTS.md`, automatically | Use plan mode to draft the handoff; then hand off to a Builder (or build directly) |
| **Cursor** | Builder | Add `AGENTS.md` to its context (or copy its contents into `.cursorrules`) | Open the handoff file in the editor / paste it into the chat |
| **ChatGPT (+ GitHub)** | Reviewer | Paste `AGENTS.md` once at the start of the review chat | Give it the filled `review-request.template.md` + the branch/diff to review |
| **Any other local-folder coding tool** | Builder | If it doesn't auto-read `AGENTS.md`, paste the file in at session start | Paste the handoff |

## Golden rules that make it tool-agnostic

- **`AGENTS.md` is the source of truth.** If a tool can't auto-read it, paste it in. Never run a tool on the project without it having seen the rules.
- **Builder and Reviewer must be different sessions** (different tool if you have one, otherwise a fresh cold-start session of the same tool). The whole point of review is fresh eyes. Same-session self-review is the one thing that doesn't work.
- **One filled handoff per task.** It carries the role, scope, stops, and verification. Any tool that reads it knows how to behave.
- **The machine-readable block in the handoff wins** if a tool seems to be "interpreting creatively." Point it back at the `allowed_paths` / `forbidden_paths` / `requires_human_stop` list.

## If a tool ignores the rules

Some tools follow instructions more reliably than others. If a Builder strays outside its `allowed_paths`, adds a dependency without asking, or claims "done" without verifying:

- Stop it, point it at the specific rule in `AGENTS.md` / the handoff.
- If it keeps straying, switch that role to a different tool. The framework is designed so you can swap tools without changing the process.

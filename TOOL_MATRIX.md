# Tool Matrix — how each AI tool plugs into the framework

The framework is **role-based**, so any tool can fill any role. But each tool *consumes the rules* a little differently. This is the cheat-sheet for wiring each one up. The two things that must always be true:

1. The tool can see **`AGENTS.md`** (the project's rules), and
2. For a specific task, the tool is given the **handoff** (with its role set).

| Tool | Usual role | How it gets the rules | How you give it a task |
|---|---|---|---|
| **Codex** (CLI / cloud) | Builder | Reads `AGENTS.md` at the repo root automatically | Point it at the filled `handoff.template.md` (set `role: builder`) |
| **Claude Code** | Planner or Builder | Reads `CLAUDE.md` → which points to `AGENTS.md`, automatically | Use plan mode to draft the handoff; then hand off to a Builder (or build directly) |
| **Cursor** | Builder | Add `AGENTS.md` to its context (or copy its contents into `.cursorrules`) | Open the handoff file in the editor / paste it into the chat |
| **ChatGPT (+ GitHub)** | Reviewer | Paste `AGENTS.md` once at the start of the review chat | Give it the filled `review-request.template.md` + the branch/diff to review |
| **Any other local-folder coding tool** | Builder | If it doesn't auto-read `AGENTS.md`, paste the file in at session start | Paste the handoff |

## Golden rules that make it tool-agnostic

- **`AGENTS.md` is the source of truth.** If a tool can't auto-read it, paste it in. Never run a tool on the project without it having seen the rules.
- **Builder and Reviewer must be different tools (or at least different sessions).** The whole point of review is fresh eyes. ChatGPT reviewing what Codex built (or vice-versa) is the intended setup.
- **One filled handoff per task.** It carries the role, scope, stops, and verification. Any tool that reads it knows how to behave.
- **The machine-readable block in the handoff wins** if a tool seems to be "interpreting creatively." Point it back at the `allowed_paths` / `forbidden_paths` / `requires_human_stop` list.

## If a tool ignores the rules

Some tools follow instructions more reliably than others. If a Builder strays outside its `allowed_paths`, adds a dependency without asking, or claims "done" without verifying:

- Stop it, point it at the specific rule in `AGENTS.md` / the handoff.
- If it keeps straying, switch that role to a different tool. The framework is designed so you can swap tools without changing the process.

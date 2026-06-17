# 08 — Context & Thread Management (keeping a long build from going stale)

The thing nobody warns you about: **an AI conversation gets worse the longer it runs.** Not because the tool is broken — because every message piles onto the same limited "context" (the AI's short-term memory of this chat). As that fills up, the tool gets slower, starts forgetting decisions you made earlier, repeats itself, and makes mistakes it wasn't making an hour ago.

This will happen to you on any real build. The fix is simple once you know it: **the chat is disposable; your durable state lives in files.** When a thread goes stale, you start a fresh one — and lose nothing important, because the important things were never only in the chat.

---

## Warning signs your thread is going stale

You don't need to watch a meter. Watch for these:

- Replies get noticeably **slower**.
- The AI **forgets** something you settled earlier ("we already decided to use Supabase…").
- It **repeats** suggestions you already rejected, or re-asks something you answered.
- It starts **contradicting** its own earlier work, or "loses the plot" of what you're building.
- It makes careless mistakes on things it handled fine before.

When you see two or three of these: the thread is full. Don't fight it — fork it.

---

## The core principle: chat disposable, state durable

Everything that matters should live somewhere that survives closing the chat:

**Survives automatically (no chat needed):**
- **Your code + git history** — every commit is permanent.
- **`AGENTS.md`** — your project rules *and* the memory section (decisions, known risks). This is your project's brain.
- **Handoffs and any reports/notes** you saved as files.
- **The deployed app** — preview and production URLs don't care about your chat.

**Lost when the chat ends (lives only in the conversation):**
- The AI's in-the-moment reasoning and everything you discussed but **didn't write down**.
- Decisions you made verbally in the chat but never put in `AGENTS.md`.

So the rule writes itself: **if a decision or piece of state matters, put it in a file, not just the chat.** Then a fresh thread starts almost where the old one left off.

---

## Milestone check-ins (do this *before* the thread is full)

Don't wait for the thread to collapse. At natural milestones — a feature finished, a decision made, about to start something big — have the AI write the current state to a durable file. Two good homes:

- The **memory section of `AGENTS.md`** (for decisions + known risks that should live forever), and/or
- A **`checkpoint.md`** (a throwaway "where are we right now" snapshot you overwrite each milestone — template in `templates/checkpoint.md`).

> **Ask your AI:** *"Before we go further — update `AGENTS.md`'s memory with any decisions we made this session, and write a `checkpoint.md` capturing what's done, what's in progress, what's next, the current branch and URLs, and any open questions. Keep it short."*

Now if the thread dies (or you just want a fresh one), you have a clean handoff to yourself.

## Prevent rot, don't just recover from it (for bigger work)

Everything above is *reactive* — fork the thread once it's stale. For a genuinely large task, be *proactive*: don't pour the whole thing into one thread and wait for it to degrade. Instead, **split it up front into smaller sub-tasks, each done in its own fresh-context session**, coordinated through your files (`AGENTS.md` memory + `checkpoint.md` + per-task handoffs). Research one part in a clean session, write the finding to a file; plan the next in another clean session reading that file; build each piece fresh. Each session starts sharp because it carries only what its file hands it, not the accumulated debris of everything before.

This is how large multi-step builds stay coherent — the durable files are the shared memory, and no single session has to hold it all. (It's the same discipline this framework was *built* with.)

---

## Starting a fresh thread cleanly (the resume)

When you fork to a new thread, the new session knows nothing about the chat — but it can read your files. Point it at them:

> **Resume prompt:** *"You're picking up an in-progress project. First read `AGENTS.md` (including the memory section) and `checkpoint.md` to understand what this is, the rules we follow, what's been done, and what's next. Then summarize back to me where we are and what you think the next step is — don't change anything yet."*

Make it summarize back *before* acting — that confirms it actually absorbed the state and didn't half-read it. Then carry on.

---

## Cold starts are a feature, not just a recovery

A fresh thread isn't a loss — it's often *better*:

- It's **faster** (empty context).
- It's **not carrying accumulated confusion** from a long messy session.
- A cold reader of your own `AGENTS.md` + `checkpoint.md` often spots gaps the tired thread had stopped noticing.
- It's the same trick that powers **independent review** (`TOOL_MATRIX.md`): a session with no memory of building something reviews it more honestly.

So forking isn't failure. It's good operating hygiene — like saving your work and reopening the file fresh.

---

## Practical cadence

- **One task per thread** is the ideal. Finish a feature, checkpoint, start the next feature in a new thread.
- **Fork at milestones, not mid-task.** Don't abandon a thread in the middle of an unverified change — get to a clean, committed, checkpointed point first.
- **Keep `AGENTS.md` memory current as you go**, not in one big dump at the end. Small notes after each decision cost seconds and save whole re-explanations.
- If you find yourself re-explaining your project to the AI, that's the signal your durable files are thin — fix the files, not the explanation.

The goal: no single thread is load-bearing. You can close any conversation at any time and pick up in a new one, because the project's real memory lives in the repo — not in the chat.

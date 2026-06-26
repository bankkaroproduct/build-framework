# 11 — State, Acceptance, and the Restart Trap

How to keep a build moving to a real "yes" — and avoid the most common way AI-assisted projects stall: doing a lot of work that never actually finishes.

---

## The core rule: acceptance is a human, on the real result

Every workstream needs one named human acceptance authority. That person looks at the actual result in the place it will really be used — the running app on a real device, the live API, the published page, the finished document, the shipped dataset — and says "yes, this is done."

Nothing else counts. None of these are acceptance:

- "Tests pass"
- "The AI says it's done"
- "Review approved" or "audit passed"
- "Merged" or "shipped"
- "Screenshots look right"
- "The Reviewer gave a go"

These are good signals. None of them are the acceptance. The acceptance is the human authority doing the real thing on the real surface and saying yes.

The Reviewer's "go" (see `02-orchestration.md` and `03-verification-shipping.md`) is a necessary step before shipping — it is not the final gate. The human acceptance authority closes the gate, after Verify has passed.

Name the authority before implementation starts. Write it in the state file (below). If you are the PM, founder, or product owner building this, you are probably the authority. Delegate deliberately, not by accident.

---

## The restart trap — and how to avoid it

The most common way a build goes nowhere: the same work gets re-framed as a new initiative instead of one item being taken all the way to accepted.

You'll recognise it by these symptoms:

- "v2", "v3", "wave N", "polish pass", "production-finish", "redesign", "final audit" — all of the same thing that was never accepted the first time.
- The team produces reviews, audits, and reports instead of a finished thing.
- New lanes keep opening before old ones close.
- Scope keeps expanding instead of shrinking to one item.
- Tokens and hours burn but nothing reaches an accepted state.

The fix is not better tooling or a longer audit. It is discipline on two things:

1. **One item at a time.** Do not start the next item until the current one reaches accepted. A new lane may only displace the active item if the state file is updated to park the current item with a reason and a resume condition (below).
2. **Acceptance criteria before implementation.** Write what "yes" looks like — including explicit "do not ship if" failures — and have the acceptance authority sign the criteria before any code is written. See the handoff template (`templates/handoff.template.md`), which has an acceptance checks field for exactly this.

---

## One state file

At any moment there is one short living file that is the truth about where the build is. Call it `STATE.md` (or name it whatever fits your project, but keep one). It contains:

- **Current goal** — one sentence.
- **The single active item** — what is being built right now, who is building it, what gate it is at.
- **Accepted: yes/no** — has the human authority accepted it on the real surface?
- **Parked items** — anything set aside, with a reason and a resume condition.

That is it. Everything else is history or archive.

Rules:

- If it is not in the state file, it is not active. Anything not listed there is parked or done.
- The state file is updated at each gate: when an item is started, when it reaches Verify, when it is accepted, when it is parked.
- No competing trackers. No giant append-only "current status" documents that grow to hundreds of entries. One file, short, current.

You can start with a very simple version:

```
# STATE

Goal: let users reset their password

Active: password-reset email flow
Builder: your build tool, on branch feature/password-reset
Gate: Verify (preview tested, awaiting human acceptance)
Accepted: no

Parked: —
```

Update it as you go. Archive old versions by appending to a `STATE-history.md` if you want the record, but `STATE.md` stays short and current.

---

## One executor, one checker — never the same

The agent or tool that wrote the spec does not implement and self-grade it. The session that built the feature does not also review it. This is the same principle behind the Reviewer role in `FRAMEWORK.md` — independence is what makes review meaningful. Apply it here too:

- One builder writes the code for a given item.
- A different session (or a different tool) reviews it before acceptance.
- The acceptance authority is a human — not the builder's self-assessment and not the reviewer alone.

If you have only one AI tool, a fresh cold-start session of the same tool reviewing its own prior work is far more reliable than the same ongoing session doing both. See `02-orchestration.md` for the Reviewer trick.

---

## Branch, microservice, and doc hygiene

Work accumulates junk if you do not clean up as you go. Three areas where this causes the most trouble:

**Branches.** One branch per task. When the task is merged and accepted, delete the branch. Orphaned branches are a sign that work was started and never finished. The branch list is a quick proxy for how many open items you actually have.

**Microservices.** When a separate service runs alongside the main app, it runs in its own thread under the same framework. Its changes are confined to its own subdirectory. Shared logic it imports has one owner (the main lane). Other threads consume shared logic read-only, request changes through the owner, and rebase — two threads never edit the same shared file at once. If two threads need to change the same shared thing, stop and decide who owns it first.

**Docs.** Docs follow a simple lifecycle: active while being worked, done when accepted, archived when superseded. Cleanups are manifest-first: list what you are removing before you remove it, so nothing disappears by accident. Never delete a doc without knowing what referenced it and updating those references. A docs sprawl of hundreds of files usually means items were never closed — state file hygiene prevents most of it.

---

## Checklist: before starting a new item

- [ ] The state file names the current active item and its acceptance authority.
- [ ] Acceptance criteria are written and signed by the authority before any code starts.
- [ ] Explicit "do not ship if" failures are in the acceptance criteria.
- [ ] The previous item is either accepted (yes) or formally parked with a reason + resume condition.
- [ ] A branch is created for this item (and only this item).
- [ ] Builder and Reviewer are different sessions.

---

## Cross-references

- The five-step operating loop (Plan, Build, Verify, Ship, the human gate): `FRAMEWORK.md`
- Handoff format and acceptance checks field: `templates/handoff.template.md`
- The Reviewer role and independent review: `02-orchestration.md`
- Verify: your own eyes on the running app: `03-verification-shipping.md`
- When to stop and call a human engineer: `05-incident-escalation.md`

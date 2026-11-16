---
name: bye-bye-bye
description: "Close a work session with a concise repository handoff. Use when the user says ‘bye dove’, ‘chao dove’, ‘I’m done for now’, ‘let’s wrap up’, ‘let’s stop here’, ‘that’s it for today’, ‘call it a day’, ‘signing off’, ‘hasta aquí por hoy’, ‘lo dejamos aquí’, ‘cerremos’, ‘terminamos por hoy’ or ‘deja un handoff’, asks to leave a handoff, or wants the next session to resume safely. Also prepares a copy-ready handoff to move a thread to another repository: ‘move this thread to another repo’, ‘carry this work over’, ‘transfer this work’."
---

# ByeByeBye

The Name Recalls *Bye Bye Bye* by NSYNC; the farewell makes it easier to remember.

A session Closing, not a Summary for Display.
It Leaves the next Session one durable Thread to pick up.

The next Turn Lives in [YoYoYo](../yo-yo-yo/SKILL.md).
The Canon Lives in [Change Growth](../../../rules/change-growth.md).
The working Snapshot Lives in [RememberTheDay](../remember-the-day/SKILL.md).
The Closing Recap Lives in [TIL](../til/SKILL.md).
Moving a Thread to another Repository Lives in [Transfer](references/transfer.md).
The Shape Lives in [The Lever and the Tape](../../../patterns/the-lever-and-the-tape.md).

```mermaid
flowchart TD
	START["bye dove"] --> EVIDENCE["Read Intent · Changes · Decisions · Validation"]
	EVIDENCE --> GROWTH{"Growth"}
	GROWTH -- Together --> NOW["Keep one Now Part"]
	GROWTH -- Split --> SPLIT["Keep every Part · Mark one Now"]
	GROWTH -- Unclear --> CHECK["Make Next the focused Check"]
	NOW --> WRITE["Replace .handoff.md"]
	SPLIT --> WRITE
	CHECK --> WRITE
	WRITE --> REPORT["Report where the Handoff Lives"]
	REPORT --> TIL["Write Three-act TIL Recap"]
	TIL --> STOP["Stop · Do not Continue Work"]
```

## When it Runs

Invoke when the user says `bye dove`, asks to stop,
or Requests a handoff for the next session.
Do not use it when the current Thread moves to a paired repository
and continues in the same conversation; use [Transfer](../bye-bye-bye/references/transfer.md).

## Evidence

Read only the current Thread and current repository state:

1. **Checkpoint** — `.handoff.md` when it Holds the current working snapshot.
   Confirm it against the thread and repository state; never trust stale Claims.
2. **Intent** — the ask that still Explains the work.
3. **Changes** — staged, unstaged and untracked Paths,
   plus the relevant Change in each affected file.
4. **Decisions** — choices the next session must Preserve.
5. **Validation** — checks already Run and their result.
6. **Growth** — whether the Work is `Together`, `Split` or `Unclear`.

Do not reconstruct old History unless the current thread refers to it.
Do not review the Code or invent work that was not discussed.

## Write the Handoff

Write `.handoff.md` at the repository Root.
Replace its content when it already Exists;
one Repository Holds one active Handoff.
Expand a compact Checkpoint into this full closing shape.

Use this Shape:

```markdown
# Handoff

**Intent** — the one Outcome being pursued.

**Done** — durable Work already completed.

**Open** — the unresolved Decision, edit or validation.

**Blockers** — what prevents progress and what would clear it, or `None`.

**Growth** — Together, Split or Unclear.

**Now** — the one Part to resume first.

**Later** — deferred Parts, or none.

**Files**
- `path` — Added, Modified, Deleted or Renamed.
  What changed, why it matters, and the pending Detail needed to continue.

**Related repositories** — directly related repo/worktree, branch, state and relevant paths or commit; otherwise `None`.

**Logs** — source, date and session/record ID for needed cross-repo context; `Unavailable` if relevant logs cannot be read, otherwise `None`.

**Transfer** — `None`, `Prepared for manual copy to <destination or Not specified>`, or `From`/`To` a repository handoff; only the receiver owns the active `Next` after transfer.

**Validation** — the Command and its result, or Not Run.

**Next** — one concrete Action.
```

The files Section makes the Handoff usable without repository access.
List every File affected by the active work, including already committed files
the next session must understand. For each one, name its State and
summarize the relevant change. Include a small exact Snippet when names,
values, signatures or unfinished text cannot be recovered reliably from prose.
Do not copy a whole File or a large diff.

Keep claims Factual and paths repository-relative.
Mark uncertain claims as `Inferred`.
For related repositories, inspect only work named in the active Thread.
Record log references and the event needed to resume, not raw logs.
When closing a transferred Thread, preserve the `Transfer` direction from
[Transfer](../bye-bye-bye/references/transfer.md) and the pointer to the repository
that owns the active `Next`.
Never Store secrets, tokens, terminal history or full conversation text.

When growth is `Split`, preserve every Part
but mark exactly one as `Now`.
When growth is `Unclear`, make `Next` the focused Check
that can separate the intents.

## What it Returns

After writing, report where the Handoff Lives, then end with a three-paragraph
TIL recap of the session. Follow [TIL](../til/SKILL.md)'s window and legibility
rules; the recap is the final content, with no sign-off or offer after it.
Do not repeat the full Handoff in the recap.

```text
✅ Handoff Written to .handoff.md

We began by… [Act one: the purpose or question that brought us here.]

Then… [Act two: what changed, was decided or became clear.]

Now… [Act three: where we left the work and what comes next.]
```

## Bounds

- Never Commit or push the handoff.
- Never stage Files.
- Never continue Implementation after writing it.
- Never Hide failed or missing validation.
- Never Include unrelated dirty paths.
- Never Assume the next session can open an affected file.
- Never Leave two parts marked `Now`.

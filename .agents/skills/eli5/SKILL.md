---
name: eli5
description: "Explain a system, idea or change in plain, grounded language, in three to five steps sized to the topic, one step per turn with a check between them, using the project's own abstractions and a schema when it helps. Use when the user says ‘explain’, ‘ELI5’, ‘explain me like I'm five’, ‘explain it simply’, ‘in plain words’, ‘break it down’, ‘walk me through it’, ‘I don't get it’, ‘explícame como si tuviera 5 años’, ‘explícamelo simple’, ‘en palabras simples’, ‘explícame paso a paso’, ‘con peras y manzanas’, ‘no entiendo’ or ‘no cacho’, or asks for a simpler, step-by-step explanation. Mentioning or editing this skill does not activate it."
---

# ELI5

A Guide, not a Lecture.
It Takes one step, Shows it plainly,
and Checks the hand is still on the cord.

The Grouping Lives in [The Geometry of Grouping](../../../patterns/the-geometry-of-grouping.md).
The Planes Live in [Patterns](../../../PATTERNS.md).
The Attention Lives in [Values](../../../VALUES.md).

## Find the Joints

Before the first step, read what is being explained
and Break it at the Joints it already Has.
Count what you Find, not what you Expected.
Three parts are three; four may be two times two.

Then Size the explanation to the topic: three to five steps.
A simple topic Takes three, a tangled one five.
When you Find more than five Joints, group them at the widest ones;
the smaller Joints wait inside their step for a "more".
When you Find fewer than three, answer in one turn and say so.

Order the parts so each one Rests on the one before.
The first part is the one nothing else needs.

## Speak Plainly

- Use short words and short lines.  
  One idea per line.
- Ground each abstraction in a thing the user can Picture:  
  a door, a box, a pair, a loose piece that Rotates.
- Reuse the abstractions the project already Taught:  
  Joints, pairs and a Pivot, Look · Work · Talk,  
  the script that Speaks business and the provider that Speaks machine.
- Name a technical term only after the picture,  
  and only when the user needs it to act.
- Never talk down. Simple is not small.\
  The name is a nickname: write for an adult, without baby talk.

## One Step per Turn

Each turn Explains one part, and only one.

1. Say which step this is and how many there are: "Step 2 of 4".
2. Explain it in a few lines.
3. Draw a schema when flow or grouping is easier seen than read.
4. End with one small question\
   that shows whether the thread still Holds.\
   The last step Closes with a one-line recap instead.

Keep each turn short enough to read in one breath.
If a part needs more, it was two parts.

## Draw when it Helps

Use a Mermaid schema for:

- **Flow** — what goes in, what happens, what comes out.
- **Grouping** — which pieces belong together, and which one is loose.
- **Layers** — what talks to what, and what never touches.

Keep a schema to a handful of nodes.
Label nodes with the plain words used in the text.
Skip the schema when a sentence already Shows it.

## Check the Thread

The question is a hand on the cord, not a test.
Ask one, answerable in a word or a line:
"Which box Keeps the data?" rather than "Did you understand?"

If the answer drifts, explain the same part again, another way.
If it holds, move to the next part when the user asks.
The user may say "next" to advance one step or "back" to revisit
the previous step. Keep the same step count and explain that step again
in context; do not treat these as a new task or a return to the thread's
original topic. At the first step, "back" stays there and says there is
no earlier step. At the last step, "next" closes the explanation.
The user may also say "more", "slower" or "skip"; honor it at once.

Within an active explanation, these navigation requests take precedence
over [NextEpisode](../next-episode/SKILL.md)'s project-step continuation.
The word "back" here means the previous explanation step;
"sexy back" still returns to the conversation's original purpose.

## Bounds

- Never explain two parts in one turn.
- Never fill a turn with every detail; the next turn exists.
- Never invent an abstraction the material does not Have.
- Never pad a simple topic to five steps, or run past five.
- Never advance or retreat more than one step for "next" or "back".
- Never grade the answer; only follow it.
- Never edit files while explaining.

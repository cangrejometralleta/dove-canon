# Roadmap

Things the project means to build. Not belief, and not canon
until each one exists.

## Own linter

The rules are already almost all mechanical — count
beats, count the words in a name, measure nesting. A Go binary that
reads Rules as its own config would close the loop: the markdown
is already a three-layer AST, and the project would validate itself
against its own document. It's roadmap, not belief. It doesn't go up
to canon until it exists.

## Modular cluster with LiteLLM

Long-term technical goal. Natural
continuation of `The Guest you can Evict`. Wait until it exists.

## Measure DeLaCase before the Linter

Capitals cost the Writer a decision, and nobody has measured
what they save the Reader. The Linter waits for that number.

- Color is out. No practical Client renders it,  
  and a Capital survives plain text everywhere.
- The Linter can check only the countable half:  
  the Budget per passage, the free initial, the exempt Names.  
  Which words carry the Relationship stays the Writer's decision.
- Dashes split a passage only when they split a clause,  
  so the Linter would Warn, never Fail.

An Estimate of comprehension, in three layers, cheapest first:

1. Count the output Tokens and the Budget by script,  
   for the plain, DeLaCase and mini versions of one long text.
2. Check Highlight coverage. Extract a key of Entities and Interactions,  
   then ask how much of the key the Capitals mark,  
   and how many Capitals fall outside it.  
   This tests the Capitals, not the Reader, and needs no person.
3. Ask a model Reader the same fixed questions of each version,  
   graded blind against the key, over repeated runs.  
   Ask it also which passages it would want re-explained, and count them.

The Limit: a model reads every token, and a person skims.  
So the Estimate says what a model understood, not what a person does.  
A short human check, plain against DeLaCase, confirms or breaks it.

Build the Linter when a first Measurement shows the Capitals earn their cost.

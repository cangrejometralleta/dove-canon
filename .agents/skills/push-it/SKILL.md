---
name: push-it
description: Analyze local Git changes, explain the features they implement, separate them into coherent commits, commit each feature, push after every commit succeeds, then deploy or publish through the project's own configured mechanism. Use when the user asks to organize local changes into feature commits, split the work into separate commits, or commit and push the changes.
---

# PushIt

The Name Recalls *Push It* by Salt-N-Pepa.
A Curator, not a commit message Generator.
It Reads the local Changes, finds the Features inside them,
explains the separation, then commits, pushes and publishes the work.

The Canon Lives in [Emoji](../../../rules/emoji.md),
[Structure](../../../rules/structure.md)
and [Channels](../../../rules/channels.md).

## What it Reads

Three Sources, all from git. Nothing else.

1. **State** — every tracked, staged and untracked Path:

   ```
   git status --short
   ```

2. **Changes** — staged and unstaged Diffs:

   ```
   git diff
   git diff --cached
   ```

3. **Lineage** — recent Messages and the upstream:

   ```
   git log -10 --oneline
   git status --branch --short
   ```

Read untracked Files before grouping them.
Never infer their Feature from a filename alone.

## What it Explains

Before staging anything, name each Feature and its paths.
Explain why each Group Belongs in one Commit.
Keep unrelated Changes separate, even when they share a file.

```
Commit 1 — Shorten skill names
- Renames the four shared skills and repairs their Links.

Commit 2 — Add the commit workflow
- Replaces the status-only Skill with an explicit commit and push flow.
```

Ask before proceeding when a Path could belong to more than one feature.
Do not stage a mixed Hunk merely because its file is already in a group.

## What it Does

1. Analyze every local Change before staging,
   and clear [Plaintext Credentials](#plaintext-credentials) first.
2. Before each commit, apply [Dirrty](../dirrty/SKILL.md)'s attribution check
   to the exact commit-bound Changes. Remove clear vendor signatures and
   imposed attribution; preserve a vendor Name when it is the subject of the
   Work. Ask when authorship is ambiguous. Do not start a History Rewrite
   from this check; that still needs Dirrty's full Brief and confirmation.
3. Separate changes by Feature, not by file type.
4. Explain the proposed Commits before creating them.
5. Run the narrowest available Validation for each feature.
6. Stage only that Feature, using patch staging for mixed files.
7. Commit with a message that says what the feature Changes.
8. Repeat until the intended local Changes are committed.
9. Push Once, after every commit and validation succeeds.
10. [Publish](#publish) after the push succeeds.

## Publish

Pushing is not the end of the Work; the Change must Reach its readers.
Invoking this skill is the authorization to deploy or publish,
through the mechanism the project already Configures and nothing else.

What *deploy* or *publish* means comes from the repository itself.
Read its README, `AGENTS.md`, install docs, manifests and workflows
to learn how the Work reaches its users. Never from habit:

1. **Push-triggered** — a workflow that deploys on the pushed branch
   (GitHub Pages, Actions, a connected host). The push is the deploy.
   Follow the run until it Finishes and report its result and URL:

   ```
   gh run list --branch <branch> --limit 1
   gh run watch <run-id>
   ```

2. **Command-triggered** — a documented script or command
   (`deploy`, `publish`, `pages` in `package.json`, a Makefile, a README
   or `AGENTS.md`). Run it once, after the push, from the repository root.

3. **Branch-triggered** — Pages served from a branch such as `gh-pages`
   or `/docs` on `main`. Publish only through the documented build step.

4. **Source-installed** — the repository is itself what users install,
   such as skills added with `npx skills add owner/repo`, a git dependency
   or a plugin Marketplace read from the remote. The push to the branch
   users install from is the publish; confirm that the branch is the
   documented one and report the install command that now serves the Change.
   A pinned release or tag is a separate Snapshot; cut one only when the
   repository's docs say this Change Calls for it, and ask for the version.

When nothing is configured, say so in one line and stop.
Do not invent a target, create a Workflow, enable Pages
or add a deploy Script to make the step possible.

Publish only from the branch the project Deploys from.
On any other branch, push and report that the deploy was Skipped, and why.

Ask first when the step cannot be Undone or Leaves the project's own Host:
registry releases (`npm publish`, package indexes), release tags,
production Environments marked as such, or a Target that differs
from the one the repository documents.

A failed deploy does not undo the push. Report the failure,
the command or run, and stop; never retry in a Loop or Force a different route.

## First Commit

When the repository has no `HEAD`, nothing has Grown yet.
Skip the Feature analysis and the growth check; make one Commit.

1. Read the top-level tree and the README, if there is one.
2. Name the repository's Purpose in one line: `Initial commit: <purpose>`.
3. Add a body only when it helps, one line per main folder.

Before staging, check that a `.gitignore` exists
and that no Secret or ignored file is included.

## Plaintext Credentials

A Collection Remembers what the request Needed.
Clear it before it Becomes history.

**Collections** — Bruno, Postman, Insomnia, Hoppscotch,
Thunder Client, HTTP files and similar request Collections.
In every changed collection or environment File, find unencrypted values
for passwords, tokens, API keys, client secrets, cookies
and `Authorization` headers.

- Clear the Value in place; keep the key and the request.
- Prefer the tool's own Reference when it exists:
  `{{variableName}}` for a value moved to a secret or local environment.
- Leave encrypted values, secret-marked Variables and existing References alone.
- Report each cleared Path and key, never the Value.
- The local value is Lost with the edit; say so in the report
  so the user can move it into an ignored or secret Environment.

**Everything else** — a plaintext Credential in any other changed path
is a Warning, not an edit. Name the path and line, never the Value,
and do not stage that Hunk until the user Decides.

Skip obvious Examples: `changeme`, `example`, `dummy`, `test`, `xxx`,
`<token>`, `replace-with-…`, empty values and documented Placeholders.
When unsure whether a value is real, treat it as real and Warn.

```text
⚠️ Cleared plaintext credentials — local values removed
- collections/auth/login.bru — auth:basic.password
- postman/env.json — values[apiKey]
⚠️ Possible credential — not staged
- config/app.yaml:12 — db.password
```

## Branches Keep the Flow

- Work on `main` when one Change can be finished and integrated at a time.
- Use a feature branch when work needs isolation or can proceed in parallel.
  Branch from `main` and return finished, validated work there.
- On a feature branch, use [Change Growth](../../../rules/change-growth.md).
  When the feature reaches three attributable writing turns or three touched
  files, remind both the agent and the user to pause and name its scope before
  adding more. If it has become multiple intents, recommend splitting the
  work; do not silently split or merge it.
- Use `develop` only when concurrent Features need a shared Integration before
  `main`; follow the repository's configured target instead of assuming one.
- [Branches Give Cooperation a Path](../../../patterns/branches-give-cooperation-a-path.md)
  describes direct integration and the optional shared integration branch.

## The Boundaries

- Never commit Secrets, generated credentials or ignored files.
- Never Rewrite, amend or squash existing commits unless asked.
- Never Include unrelated local changes to make the tree clean.
- Never Push when validation fails or a commit fails.
- Never Deploy when the push failed or was Rejected.
- Never Deploy a target the repository does not Configure.
- When replacing remote history is required, inspect the exact target and use
  `--force-with-lease`; never use unconditional `--force` or bypass branch
  protection.
- When this workflow includes a push and an upstream Exists, push to it without
  asking for a second confirmation, on any branch. Report the push after it
  succeeds; report a rejection and stop if Git or branch protection denies it.
- Never bypass branch protection or push to a branch without an upstream.
- Never Brand a commit or pull request with its Vendor —
  no co-author, session or generated-by Trailer; the message carries the Change.
- Never create an Upstream without naming the branch and asking first.
- If nothing Changed, say so and stop.

## What it Returns

After the push, report the feature Commits and destination.
After the deploy, add one line for its Result:

```text
⚠️ Pushed to origin/main
- a1b2c3d Shorten skill names
- d4e5f6a Add feature commit workflow
⚠️ Published to GitHub Pages — https://owner.github.io/repo/
```

Say `Deploy skipped` with the Reason when nothing is configured,
the branch does not Deploy, or the step Failed.

## Sources

[Emoji](../../../rules/emoji.md) —
one per Line, only to mark a state.
[Structure](../../../rules/structure.md) —
Group, Validate and Deliver.
[Channels](../../../rules/channels.md) —
plain Beats a compromise the reader has to decode.

# Code Generation Needs an Understandability and Change-Cost Trial (MOVING)

The summary benchmark gave a narrow brevity signal across three-paragraph story
summaries. Code carries a different promise: Dove does not promise less code
or fewer lines; it promises code a person or agent can understand, verify and
change without breaking unrelated layers (`values/approaches.md` measures cost
of change, never line count).

Token and line counts still belong in the trial, but only as a cost ceiling to
check that Dove's structure does not carry an unreasonable token penalty. A
short patch that collapses layers or skips an acceptance case fails the Canon's
promise even when its build passes.

## Planned comparison

- **Conditions:** vanilla, Caveman and Dove, in fresh worktrees.
- **Model:** `gpt-6-luna` (and paired runs on `claude-sonnet-5`, kept on its own scale).
- **Effort:** high.
- **Runs:** three independent samples per condition and scenario.
- **Fixed inputs:** task, starter commit, tools, time limit and acceptance checks.
- **Review:** hide the condition label until correctness, boundaries and understandability are scored.

These are the settings for the planned code trial. The earlier story-summary
benchmark ran in a different session; its serving model and effort were not
exposed, so its results do not belong to Luna High.

## School as the scale reference

School gives three useful scales. A small case resembles one rule in `school/`,
such as RUT validation or token expiry. A medium case resembles one application
use case, such as enrollment, or one request parser. A large case is a complete
runtime generated from the contract, specification and shape: nine routes,
fourteen faults, settings, composition and tests.

The full three-language School rewrite adds language and regeneration effects.
Keep it outside a single style run; use one Go runtime as the large anchor and
compare results within each size before comparing across sizes.

## Scenario cards

Each task would start from a fresh, versioned fixture. The model could change
production code, but not the task text, acceptance checks or measurements.

### Generate

| ID | Size | Task | Acceptance |
| --- | --- | --- | --- |
| G-S | Small | Implement a pure Chilean RUT check-digit validator using `examples/specs/school/SPEC.md`. | Accept valid lower- and upper-case check digits; reject malformed input, wrong check digits and empty input; preserve leading zeroes. |
| G-M | Medium | Implement School enrollment using the ordering and failure rules in `examples/specs/school/SPEC.md`: validate form, check course, ask registry, store, then announce. | Fake ports prove call order; denial differs from registry failure; announce once after storage; a failed notice does not fail enrollment. |
| G-L | Large | Generate the Go School runtime from `examples/specs/school/openapi.yaml`, `SPEC.md` and `SHAPE.md`. | All nine routes and fourteen faults match the contract; specified boundary cases pass; the Core imports no wire, framework, store or socket package. |

### Repair

| ID | Size | Task | Acceptance |
| --- | --- | --- | --- |
| R-S | Small | Repair the token-expiry comparison described in `examples/school/BEFORE.md`. | A token is accepted before its deadline and rejected after it; check the exact boundary with a fixed clock. |
| R-M | Medium | Repair a seeded School page parser that treats malformed values as zero and loses repeated-key behavior. Use the Page rules in `examples/specs/school/SPEC.md`. | Reject `+1`, `1e1`, whitespace, empty values and hex; use the first repeated value; ignore unknown keys; preserve one-sided defaults and large-number behavior. |
| R-L | Large | Repair a seeded contract drift across route, handler, application and storage boundaries in one School runtime. | Restore parity with recorded responses for supplied cases; preserve fault reasons, status, content type and body; all acceptance checks pass. |

### Refactor

| ID | Size | Task | Acceptance |
| --- | --- | --- | --- |
| F-S | Small | Extract RUT check-digit arithmetic from a seeded School student validator into one named pure function. | Existing valid and invalid results remain identical; add no dependency or public API. |
| F-M | Medium | Separate page-query parsing from other request-form checks in School `app/validation.go`. | Existing request cases and fault answers remain identical; handlers and business rules gain no parsing logic. |
| F-L | Large | Regenerate one seeded School runtime from the Contract, Specification and Shape while preserving its API behavior. | All supplied contract cases pass; vendor boundaries remain; no unrelated public surface or dependency appears. |

## What is still missing

The repair and refactor cards need seeded starter fixtures with deliberate
defects or structure. Those fixtures must live outside the School reference so
the benchmark never damages its working implementation. Acceptance checks and
boundary checks also need to be packaged before running any condition.

Record three separate layers for each run, keeping the promise apart from the
cost ceiling:

1. **Correctness and boundaries (the gate):** passing and failing acceptance
   checks, Core import checks (`go list -deps ./school` naming no vendor,
   socket or wire package), separation of the three `Shapes` (wire, business,
   storage), `adaptertest` parity, omitted behaviors and unnecessary public
   surface.
2. **Understandability and change cost (the promise):**
   - *Change locality* (measured on `Repair` and `Refactor` cards): files and
     packages touched per change, whether edits leak across `school/`, `app/`
     or adapters, and whether boundary checks still pass after the edit.
   - *Blind reader lookup / resumption*: fresh reader sessions (or a human
     check) asked against the generated codebase alone where a rule lives, how
     a fault maps to a status, or where a new check belongs without crossing
     layers — scored on answer accuracy and navigation steps.
3. **Generation cost (the ceiling, not the goal):** response tokens,
   changed-code tokens, production lines, files, dependencies and completion
   time. Report medians and ranges.

Keep correctness, boundaries and understandability separate from cost: a
smaller patch that fails a check or collapses layers is a failure, not an
efficiency win.

Distill after the fixtures and a first controlled run measure correctness,
boundaries, understandability and change cost across conditions.

## Understandability and change-cost protocol

Before building seeded fixtures for `Repair` and `Refactor`, the existing
`G-L` outputs (and `examples/school/go/` as the reference anchor) can be scored
with one fixed protocol that assumes no Dove-specific package names:

1. **Boundary gate (automated) + negative check:**
   - Run `./build.sh` and the shared `adaptertest` suite.
   - Run `go list -deps` on the business-core package and `go list -f '{{.ImportPath}} -> {{.Imports}}' ./...` across all packages to record whether core logic imports any HTTP framework, `net/http`, ORM, SQL driver or socket package.
   - Verify the gate itself fails on a temporary negative check (injecting a forbidden import such as `net/http` or an ORM into the core package) so a passing gate cannot be a no-op.
2. **Blind reader lookup (7 questions, 14 points + navigation count):**
   Give a fresh toolless session (or human reader) only the generated Go source
   directory, with condition labels removed, and ask it to answer using only
   the code and cite file paths and symbols. Record the reader model and effort,
   and pair a strong reader with a light reader (`flash_lite` / low effort) so
   a ceiling on a strong model does not mask differences in navigability:
   - *Q1 (Domain check order & constants):* After JSON parsing, in what order are a student's domain fields validated, and where does minimum-age configuration come from? (Order `Name -> RUT -> Age`; global constant, not deployment env.)
   - *Q2 (Admission & notice failure):* In what order are Course, Registry and Store checked on enrollment, and what happens when the Registry is unavailable vs when the announcement fails? (`Form -> Course -> Registry -> Store -> Announce`; Registry unavailable fails `503`, announcement failure logs and still succeeds.)
   - *Q3 (Fault mapping):* Where does a domain/service failure turn into an HTTP status and JSON body, and what reaches the caller on an unexpected driver error? (Mapping site; `500` with `internal error` and no driver message.)
   - *Q4 (RUT normalization & uniqueness):* How are `12345-K` and `12345-k` normalized, are leading zeros kept, and what proves RUT uniqueness? (Lowercase `k`, leading zeros kept; store uniqueness constraint -> `409`, not in-memory scan.)
   - *Q5 (Adapter swap surface):* Which Go source files change to add another HTTP framework adapter alongside `stdlib` without changing business or store behavior? (Adapter + composition root only; core and store untouched.)
   - *Q6 (Change localization):* If `POST /students` must reject enrollment in a course that already holds a global constant of 30 students with `422` (`course is Full`), which files and packages change and which stay untouched? (Identifies fault, status map if needed, rule/config constant, store count query, service check and tests; states whether HTTP adapters stay untouched.)
   - *Q7 (Absence / negative control):* Where in the code are `PUT /courses/{id}` and `DELETE /courses/{id}` routed and implemented? (Must state from code that neither route nor handler/service method exists, rather than inferring CRUD symmetry.)
   Record score out of 14, file lookups/clarification requests needed, and any place the reader misread a boundary or hallucinated an absent feature.
3. **Code negative control (coupled baseline that passes tests):**
   Include at least one working, coupled Go `School` implementation (single
   package or `BEFORE`-style coupling of HTTP, domain and GORM that still passes
   the contract tests) alongside `vanilla`, `Caveman` and `Dove`, and run the
   same boundary gate, blind reader lookup and change-locality probe on it. A
   protocol that gives the coupled baseline the same score and locality as
   `Dove` does not measure understandability or change cost.
4. **Change-locality probe (next step once paired `G-L` trees are captured):**
   Apply Q6's `course is Full` change across each condition's `G-L` output (and
   the coupled negative control) and record production files touched, packages
   crossed, whether HTTP adapters or store schemas changed, and whether
   `./build.sh` and the core `go list -deps` check still pass.

### Protocol calibration — `examples/school/go/` — 2026-11-28

Ran the boundary gate and a fresh `research` subagent (`Gemini 4 Hydrogen`, High)
restricted to the 24 production `.go` files in `examples/school/go/`:

- **Boundary gate:** `gofmt -l .`, `go vet ./...` and `go test ./...` passed across all 11 tested packages; `go list -deps ./school` listed zero HTTP, ORM or socket packages (`app/` imports `net/http` only for status-code constants in `app/status.go`).
- **Blind reader calibration (`Gemini 4 Hydrogen`, High):** 12 of 12 across Q1–Q6 in 25 tool calls (1 file list + 24 file reads) with zero misread boundaries. The calibration exposed ambiguity in the initial Q6 wording (where capacity lives and whether `PUT` shares the check), Q1 (JSON vs domain validation) and Q5 (`.go` files vs `go.mod`/config), which were tightened above; it did not yet include Q7 or a coupled negative control.
- **Light blind reader calibration (`flash_lite`, Q1–Q7):** 10 of 14 in 6 tool calls (1 file list + 5 file reads; never read `faults/faults.go`, `store/store_gorm.go`, `settings/`, `serving/reading.go`, `transport/`, `wire/` or `main.go`). It passed Q1 (2/2), Q2 (2/2) and the Q7 absence check (2/2, confirming `PUT`/`DELETE /courses/{id}` do not exist), but guessed unread files on Q3 (1/2: claimed `{error: ""}` instead of `"internal error"`), Q4 (1/2: inferred store uniqueness without reading `store_gorm.go`), Q5 (1/2: invented `serving.Adapter` and included `wire/` and `transport/`) and Q6 (1/2: missed `settings/` and `app/providers.go`). The light reader avoids the strong-model ceiling and exposes guesswork when files go unread.

## Preliminary G-L Run — 2026-11-10

One large School generation was run for each condition from independent copies
of commit `117e11abba17ae2df155008e8ae2b2218739cd91`. Each copy began with the
same contract, Specification, Shape, project Rules, Go acceptance tests and
build script; production Go files were removed. Tests and other fixture inputs
were held fixed. The model was `gpt-6-luna`, effort `high`, through Codex CLI
0.166.0. This is one sample per condition (`n=1`), not the planned three, and
only covers G-L.

| Condition | Input tokens | Cached input | Output tokens | Reasoning output | Production files | Lines | Bytes | Acceptance |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | --- |
| Vanilla | 5,444,400 | 5,323,392 | 23,937 | 3,208 | 15 | 1,510 | 42,052 | `./build.sh` passed |
| Caveman | 3,138,010 | 3,049,216 | 18,934 | 3,990 | 15 | 1,514 | 42,162 | `GOFLAGS=-buildvcs=false ./build.sh` passed |
| Dove | 3,216,060 | 3,114,240 | 24,430 | 6,243 | 17 | 1,534 | 43,667 | Format and vet passed; tests failed |

Token totals come from each Codex CLI turn's usage record. Input includes the
prompt and repeated context loaded during tool use; cached input is part of
input, not additional tokens. Output includes reasoning, tool calls and final
text, so it is not a code-only token count. The generated files' line and byte
counts provide a separate size measure. Since all production source began
absent, these are generated totals, not net diff lines.

Vanilla and Caveman ultimately passed format, vet, tests and binary build.
Both needed code corrections after earlier gates failed. Dove's production
binary built and format and vet passed, but the three HTTP adapter acceptance
tests failed: each returned status `201` with body `{"id":"007"}` where the
shared adapter contract expected another answer. Other package tests passed.
An escalated rerun confirmed this is a code mismatch, not the sandbox's local
socket restriction.

The official [Caveman skill](https://github.com/JuliusBrussee/caveman/blob/main/skills/caveman/SKILL.md)
instructs that persisted code, comments and documents remain in normal prose.
So this condition tested the actual Caveman voice during the run, but it did
not ask the model to write Caveman-style source. The generated code's small
size differences are not evidence of a code-style advantage.

Treat this as preliminary only. Complete Dove's failing adapter behavior, run
G-S and G-M, and collect two more independent samples per condition before
comparing medians or drawing a style conclusion. The fixtures and JSONL usage
records for this run are in `/tmp/onetwothree-school-bench-Oq9Lf4/` on the
benchmark machine; the result table above preserves the measurements here.

## G-L Dove Retry — Sonnet High — 2026-11-13

A second G-L Dove sample, on a different model and runtime, to see whether the
failing adapter behavior above repeats. One sample (`n=1`), not a
replication of Luna High: model, CLI and the usage metric's shape all differ,
so the two rows do not share a Scale.

**Setup:** a fresh git worktree from the current `main`, with the same
production files removed from `examples/school/go/` as the first run —
tests, `serving/adaptertest/adaptertest.go`, `build.sh` and `run.sh` held
fixed. **Model:** `claude-sonnet-5`, effort `high`, through Claude Code
2.1.282 (`--print --dangerously-skip-permissions`), one fresh session, no
installed skill — the task instruction pointed the session at `RULES.md` and
its linked Rules directly. **Verification:** `build.sh` was run a second
time, independently of the session's own report, before trusting the result.

| Condition | Output tokens | Reasoning tokens | Production files | Lines | Bytes | Acceptance |
| --- | ---: | ---: | ---: | ---: | ---: | --- |
| Dove (Sonnet) | 352,416 | 248,066 | 23 | 2,160 | 61,692 | `./build.sh` passed |

Format, vet, every test — including the shared `adaptertest` contract run
against `stdlib`, `chi` and `gin` — and the binary build all passed on the
independent re-run. Luna High's Dove sample failed that same adapter contract
with three `201`/mismatched-body cases; this Dove sample did not reproduce
that failure.

The token column is not comparable to Luna High's table above it. Luna's
figures are a single usage record from one Codex CLI turn; Claude Code's
`--print` session reports usage per turn across the whole agentic run (179
turns here), so the total already carries the growing cached context of a
long tool-use session, not a per-task cost. Reasoning tokens are counted the
same way in both tables but come from different runtimes and are not on a
shared scale either.

One accident during the run: the session deleted an untracked scratch file
at the worktree root while checking `git status`, outside the
`examples/school/go/` scope it was given. The file was not part of the
fixture and its loss does not affect this result, but it is worth naming as
a boundary the instruction did not hold.

This sample says the adapter failure is not forced by the Dove condition
itself — one model passed where another failed on the same contract. It does
not say Dove is cheaper or more reliable in general: still `n=1` per model,
still missing G-S, G-M, and the Caveman and vanilla counterparts on this same
runtime. Replicate on Sonnet before reading the acceptance difference as a
model effect rather than noise.

## G-L Caveman — Sonnet High — 2026-11-13

A same-day Caveman sample on the same runtime, to put a second condition next
to the Dove retry above before reading either as a style effect.

**Setup:** identical to the Dove retry — a fresh worktree from the same
`main`, the same files removed from `examples/school/go/`, same model,
effort, CLI and independent `build.sh` re-run. **Condition:** the task
instruction pointed the session at the public
[Caveman `SKILL.md`](https://github.com/JuliusBrussee/caveman/blob/2178ed6495c3c0ecd7d037e60acf3b1d0c554b97/skills/caveman/SKILL.md)
at commit `2178ed6`, fetched fresh for this run (744 words, matching the
count already recorded for it). As in the first run, Caveman governs voice
and talk, not the generated code or its comments — the skill's own rule
keeps persisted code and comments in normal prose.

| Condition | Output tokens | Reasoning tokens | Production files | Lines | Bytes | Acceptance |
| --- | ---: | ---: | ---: | ---: | ---: | --- |
| Dove (Sonnet) | 352,416 | 248,066 | 23 | 2,160 | 61,692 | `./build.sh` passed |
| Caveman (Sonnet) | 311,169 | 231,817 | 24 | 2,085 | 58,888 | `./build.sh` passed |

Both passed format, vet, every test — including the shared `adaptertest`
contract against `stdlib`, `chi` and `gin` — and the binary build, verified
independently of each session's own report. Caveman's code came out slightly
smaller and used slightly fewer tokens than Dove's, but the gap is a few
percent on `n=1` each; nowhere near the gap either showed against Luna
High's failing Dove sample. Vanilla on this same runtime is still missing,
so neither row here can yet be read against a baseline.

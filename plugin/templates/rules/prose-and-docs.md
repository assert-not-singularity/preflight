---
paths:
  - "**/*.md"
---

# Prose & documentation standards

How any Markdown in the repo should read — commit messages, PR bodies, READMEs, and docs alike.

## Content honesty

- **Document what exists.** Docstrings, comments, commit messages, PR bodies, and reference
  docs describing current code or behaviour MUST NOT describe planned or hypothetical features
  as already built. A document whose stated purpose is a design or a plan — its title or
  opening line says so — is doing its actual job when it describes target and future state;
  this bullet doesn't restrict that kind of writing.
- **No historical narration.** Prose describing current code or behaviour — docstrings,
  comments, READMEs, architecture docs, issues, instruction files — answers to the live tree,
  and MUST state what the code does now: never what it used to do, the bug it had, or the
  investigation that led here. The tree keeps changing and holds no record of its past, so a
  claim about the past has nothing it can be checked against and no way to be noticed once it
  is wrong.
  - **Avoid:** "The workflow that used to run this every two weeks was removed after repeatedly
    timing out; there is no automatic trigger today."
  - **Prefer:** "Run manually: `make audit`."
  - The tell is emphasis around a negation ("does **not**…", "is the **only**…", "no longer…")
    that exists only because the text used to say otherwise. State the current fact plainly.
    **Exception:** keep a "not X" framing when X is a real, live alternative a reader would
    otherwise assume (a genuine disambiguation, not narration).
- **A changeset describes its own diff, at its own scope.** A commit message answers to that
  commit's parent-to-commit diff, frozen together with the message the moment the commit is
  made — a commit MAY describe what it removes or replaces, and that claim is on permanent
  record and cannot go stale, which is why the bullet above does not govern it. A PR/MR body
  answers to the aggregate base-to-tip diff instead, and that diff is **not** frozen — a later
  push can move it — so the body's claim holds only until the next push and MUST be re-verified
  against whatever diff the PR currently shows, not the diff true when the body was written.
  Outside its own diff, a changeset has exactly as little to stand on as a README does. MUST
  read the diff before writing the sentence — not the memory of the edits.
  - Work done by another commit on the same branch is outside this commit's diff, however
    related.
  - A PR's subject is its net contribution. A file added by one commit and removed by a later
    one in the same PR is absent from base-to-tip: "drops `foo`" is then true of a commit and
    false of the PR — historical narration at the PR's scope instead of a sentence's.
  - The before-side earns words only as the problem the change solves — the reason, which the
    diff cannot show on its own ("duplicated the roster already in `CLAUDE.md`"). A before-state
    the diff already shows ("had no reference to either document before this") is the change
    said backwards; cut it. How the problem was found is outside the diff entirely.
  - **Avoid:** "Fixed the bug where matching silently failed — found while investigating the
    2026-07-10 run failure, reproduced in isolation."
  - **Prefer:** "Raise if the edge file is missing instead of silently skipping the match."
  - A correction to a published body exists only once the published body shows it — not once
    it has been agreed in conversation.
- **Verify before you state it.** For docs describing current code or behaviour, MUST confirm
  that every class, method, column, config key, and CLI command exists (grep, read the source)
  before naming it. When you cannot verify, describe generically or state the uncertainty — MUST
  NOT fabricate an API signature, column name, or example. A design or plan document naming a
  target method or field that doesn't exist yet (per the exception above) isn't a fabrication —
  it's the document specifying what to build.

## Voice

- Active voice, present tense. No marketing language ("powerful", "cutting-edge", "seamless").
- Short paragraphs (3–5 lines). Tables for structured information; code blocks only when they
  carry their weight.

## Diagrams and visuals support text

Text is the primary carrier. A diagram, table, or code block **illustrates** what the prose
already says — it never stands in for it. Every section opens with at least a sentence of prose,
and every diagram is followed by a sentence or two interpreting its takeaway. Back-to-back
diagrams with no prose between them is wrong.

## Ordering

Order enumerations (package lists, config/env tables, flags) **alphabetically** by default so
diffs stay stable. Keep a **meaningful order** where one exists — pipeline stages, workflow steps,
precedence rankings — and never alphabetize those.

## Examples

- **Do:** concrete and factual — says what it does, in verifiable terms:
  "The pipeline builder reads class paths from config, resolves them via import, and injects
  dependencies before starting."
- **Don't:** marketing / vague — adjectives, no information:
  "Our powerful, cutting-edge engine leverages advanced architecture."
- **Do:** honest about the unknown — states the fact, defers the detail you haven't confirmed:
  "Writes a per-item label column — check the schema for the exact name."
- **Don't:** fabricated specifics — asserts an API you haven't verified:
  "Call `upsert()`" when you haven't confirmed the method exists.

## Directive discipline & leanness (instruction files)

Applies when authoring `CLAUDE.md`/`AGENTS.md`, persona files, rules, and skills:

- Use RFC 2119 keywords (MUST, MUST NOT, SHOULD, SHOULD NOT, MAY) in directives. Hedging ("might
  want to", "perhaps consider") is not a directive — state the rule.
- The root `CLAUDE.md`/`AGENTS.md` is a thin index, not a manual — target one screen. Deep
  conventions live in focused files that load only when relevant.
- Add a rule only when it is agent-relevant **and** not derivable from the code, the docs, or a
  tool's defaults. A rule that repeats what a linter or the code already enforces is noise.
- When a shared standard covers something, reference it; do not restate it.

# preflight

A reusable **starter kit** of AI-agent standards, personas, and workflows for new projects,
distributed as a Claude Code plugin. Install it once, then run `/preflight:setup` in any repo to stamp
**local, editable copies** into that repo's `.claude/`. It is a scaffold, not a live dependency —
once scaffolded, each project owns and edits its own copies, and plugin updates never touch them.

## What it installs into a project

Everything is branched so it loads only when it is relevant, keeping context lean:

| Loaded | What | Where it lands |
|---|---|---|
| **Always-on** | `working-style` — propose-before-implementing, a question isn't a work order, report honestly, flag reusable logic, prefer the simplest solution | `.claude/standards/`, imported by `CLAUDE.md` |
| **When editing `*.py`** | `python` — 3.13+, type hints, PEP 695, `uv`, structured logging, definition ordering, suppression discipline, async safety | `.claude/rules/` |
| **When editing `*.py`** (opt-in) | `scalable-architecture` — ports-and-adapters, dependency injection, boundary validation, idempotent writes; patterns for services/larger apps | `.claude/rules/` |
| **When editing `*.md`** | `prose-and-docs` — no historical narration, verify before documenting, voice, ordering, RFC-2119 & leanness | `.claude/rules/` |
| **When committing** | `git-conventions` skill — branches, explicit staging, current-behaviour messages, co-author trailer | `.claude/skills/` |
| **When spawning subagents** | `subagent-orchestration` skill — model/effort defaults, MCP tool-cost discipline, delegation | `.claude/skills/` |
| **Personas** | `technical-writer`, `lean-implementer`, `janitor`, `web-researcher` | `.claude/agents/` |

The root `CLAUDE.md` stays a thin index; the depth lives in the files above and loads on demand.

## Install

```bash
/plugin marketplace add assert-not-singularity/preflight
/plugin install preflight@preflight
```

## Use

In a new (or existing) repo:

```bash
/preflight:setup
```

It detects repo signals (Python? git?), presents **grouped checkboxes** (Personas / Standards &
rules / Workflows) so you install only what the repo needs, copies the selected templates in
without overwriting anything you have customised, wires the always-on standard into `CLAUDE.md`,
and asks for the one-line project purpose. It records your selection and a pristine base snapshot
so updates can merge cleanly later (see below).

## Edit & update

- **Edit freely.** The scaffolded files are yours — change any persona, rule, or standard per
  project and commit it. Project-level files override anything with the same name.
- **Roll kit changes into an edited project.** Run `/plugin marketplace update` to fetch the new
  templates, then `/preflight:update` in the project. It does a **three-way merge** — ancestor
  (`.claude/.preflight-base/`) vs. new template vs. your edited file — applying upstream
  improvements while preserving your changes, and asking you to confirm any genuine conflict. It
  also offers newly added components as checkboxes. Your edits are never silently discarded.
- **Improve the kit.** Edit the templates under `plugin/templates/`, bump the version in
  `plugin/.claude-plugin/plugin.json`, and push.

### How a project remembers the kit

`/preflight:setup` writes two things into the project so `/preflight:update` can merge intelligently:

- `.claude/.preflight.json` — the version scaffolded and which components are installed.
- `.claude/.preflight-base/` — pristine copies of the scaffolded files, the merge ancestor.

Commit both. Without them `/preflight:update` can only offer a 2-way diff-and-choose, not a real merge.

## Layout

```
preflight/
├─ .claude-plugin/marketplace.json     # this repo is a plugin marketplace
├─ plugin/
│  ├─ .claude-plugin/plugin.json       # the "preflight" plugin manifest
│  ├─ components.json                  # menu of scaffoldable components (shared by both commands)
│  ├─ commands/
│  │  ├─ setup.md                      # interactive scaffold: pick components → copy into a project
│  │  └─ update.md                     # 3-way merge new templates into an edited project
│  └─ templates/                       # copied into a project's .claude/ by /preflight:setup
│     ├─ agents/                        # personas
│     ├─ rules/                         # file-scoped (paths: globs)
│     ├─ skills/                        # task-triggered
│     ├─ standards/working-style.md     # always-on
│     └─ CLAUDE.md                      # thin project index skeleton
└─ README.md
```

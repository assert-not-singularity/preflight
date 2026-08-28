---
description: Scaffold selected preflight components into this project as editable local copies.
allowed-tools: Read, Write, Edit, Glob, Bash, AskUserQuestion
---

Scaffold this project with a chosen subset of the preflight. Templates live at
`${CLAUDE_PLUGIN_ROOT}/templates`; the menu is `${CLAUDE_PLUGIN_ROOT}/components.json`; the target
is this project's `.claude/` and root `CLAUDE.md`. Everything you write is a **local, editable
copy** — the project owns it afterward.

## Steps

1. **Load the menu.** Read `${CLAUDE_PLUGIN_ROOT}/components.json`.

2. **Detect repo signals** to pre-recommend components:
   - `*.py` or `pyproject.toml` present → recommend `python`.
   - `.git` present → recommend `git-conventions`.
   - Everything with `"recommend": "always"` or `"recommend": "default"` starts recommended.

3. **Ask what to install, as checkboxes.** Use AskUserQuestion with `multiSelect: true`, one
   question per `group` in the menu (`Personas`, `Standards & rules`, `Workflows`). Mark
   recommended items "(Recommended)" in the label. `working-style` is always installed — state
   that rather than offering it as opt-out. Skip a group entirely if the repo clearly has no use
   for it.

4. **Copy only the selected components** (never overwrite an existing file — this is a starter,
   not a sync). For each selected component, copy `src` → `dest`; for directories use `cp -Rn`.
   Also snapshot each copied source into `.claude/.preflight-base/<dest>` — the pristine base that
   `/update` will merge against later.

5. **Record what was installed** in `.claude/.preflight.json`:
   `{ "version": "<from plugin.json>", "installed": ["<component keys>"] }`. This is how `/update`
   knows what to reconcile.

6. **Wire the root `CLAUDE.md`** (do NOT clobber):
   - No `CLAUDE.md` → copy `${CLAUDE_PLUGIN_ROOT}/templates/CLAUDE.md` to the repo root.
   - Exists → keep it; ensure it imports `@.claude/standards/working-style.md` under an
     "Always-on standards" heading, adding the line only if missing.

7. **Fill placeholders.** Ask the user for the one-line project purpose (and module-map /
   domain-term pointers if known) and replace the `{{…}}` placeholders. Leave them if unknown.

8. **Report** what was created, what was skipped as already-present, and which personas and
   skills are now available. Do not commit — leave changes staged for review.

---
description: Roll new preflight changes into this project, merging them with your local edits (3-way).
allowed-tools: Read, Write, Edit, Glob, Bash, AskUserQuestion
---

Update the preflight components already scaffolded in this project to the plugin's current
version, **preserving local edits**. This is a three-way merge: the ancestor is the pristine base
in `.claude/.preflight-base/`, "theirs" is the new template under `${CLAUDE_PLUGIN_ROOT}/templates`,
and "ours" is the current project file.

## Preconditions

- Read `.claude/.preflight.json` for the installed component keys and the last-scaffolded
  version. If it is missing, this project was not scaffolded by `/preflight:setup` (or predates it):
  say so, and offer to run a 2-way reconcile instead — for each file, show the diff against the
  current template and let the user choose, since there is no reliable base to merge from.
- Read `${CLAUDE_PLUGIN_ROOT}/components.json` and the version in
  `${CLAUDE_PLUGIN_ROOT}/.claude-plugin/plugin.json`. If it equals the recorded version and no
  base differs, report "already current" and stop.

## Per installed component

For each file in each installed component, classify by comparing base / theirs / ours:

- **`base == theirs`** → no upstream change. Skip.
- **`ours == base`** (you never edited it) → upstream changed and you didn't; copy `theirs` in
  cleanly.
- **both changed** → three-way merge. First try a mechanical pass:
  `git merge-file -p <ours> <base> <theirs>`. If it merges without conflict markers, use the
  result. If it conflicts, read all three versions and produce a reconciled file that keeps the
  local intent **and** applies the upstream improvement, then show the user a concise diff and ask
  for confirmation (AskUserQuestion) before writing. Never silently discard a local edit.

## New components

List components in `components.json` that are **not** in `.claude/.preflight.json`. Offer them as
checkboxes (AskUserQuestion, `multiSelect`); scaffold any the user selects exactly as `/preflight:setup`
does (copy + base snapshot + record in `.preflight.json`).

## Finish

- Refresh `.claude/.preflight-base/` to the new templates for every file that was updated, and set
  `version` in `.claude/.preflight.json` to the new version — so the next `/update` has an
  accurate ancestor.
- Report per file: updated-clean / merged / merged-with-your-confirmation / skipped-unchanged, and
  any conflicts you resolved. Do not commit — leave changes staged for review.

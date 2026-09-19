# {{PROJECT}} — AI agent instructions

{{ONE-LINE PURPOSE — what this project does. Fill in.}}

This file is a **thin index**, not a manual. Deep conventions live in focused files that load
only when relevant: personas in `.claude/agents/`, task-triggered skills in `.claude/skills/`,
and file-scoped rules in `.claude/rules/`. Keep this file lean — add a fact only when an agent
needs it and cannot derive it from the code or the docs.

## Always-on standards

Both standards can apply to the same task: use `working-style` for code changes and `communication` whenever you write to the user.

Changing the code (editing, committing, verifying):
@.claude/standards/working-style.md

Talking to the user (answering a question, or asking for a decision):
@.claude/standards/communication.md

## Orient yourself

- Repo overview / architecture → `README.md`
- {{Package or module map → …}}
- {{Domain terms → …}}

## Project-specific facts

{{Only what an agent cannot derive from the code or docs: cross-repo contracts, non-obvious
conventions, deployment gotchas. Delete this section while it is empty.}}

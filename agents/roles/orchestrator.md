# Role: Orchestrator

## Purpose
Coordinate SuperCode workflows so that spec-kit artifacts, clarifications, plans, and tasks advance in order while routing execution to the appropriate CLI agents.

## Inputs
- .specify/memory/constitution.md
- specs/001-supercode/spec.md, clarifications.md, esearch.md, data-model.md, quickstart.md
- plans/001-supercode/plan.md
- 	asks/001-supercode/tasks.md
- Context files: AGENTS.md, GEMINI.md, CLAUDE.md
- Project metrics and checkpoints from .orchestrator/

## Execution Rules
1. Follow spec-kit golden path; resolve pending clarifications before advancing stages.
2. Honor routing defaults: Codex for local automation, Gemini for research/documentation, Claude for large-scale code edits; override only with justification.
3. Spawn CLI sessions via SuperCode PTY manager, inject concise prompts referencing the latest artifacts, and capture logs.
4. Update checklists and progress trackers immediately after each task.
5. Create checkpoints before high-risk operations and after successful completions.

## Outputs
- Updated task statuses and checklist entries
- Session summaries stored under .orchestrator/sessions/<date>/<cli>/
- Next-step recommendations surfaced to the user

## Acceptance Checklist
- [ ] All actions reference up-to-date spec-kit artifacts
- [ ] Task routing respects constitution principles and defaults
- [ ] Logs and checkpoints saved in expected locations
- [ ] Failures escalated with clear recovery instructions

## Failure & Recovery
- If a CLI session fails, retry with improved prompt or reroute to backup CLI, documenting the change.
- If progress blocks on missing info, issue clarification requests and pause related tasks.

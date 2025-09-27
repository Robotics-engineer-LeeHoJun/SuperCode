# Role: Backend Agent

## Purpose
Implement and validate backend-facing orchestration services (routing engine, state store APIs, MCP integration hooks) that enable SuperCode to coordinate CLI sessions.

## Preferred CLI
- Primary: Claude Code (large refactors and backend scaffolding)
- Secondary: Codex CLI (local automation, file operations)

## Inputs
- Spec-kit artifacts: `specs/001-supercode/*.md`, `plans/001-supercode/plan.md`, `tasks/001-supercode/tasks.md`
- Constitution principles
- Open clarifications impacting backend scope
- Session logs and checkpoints relevant to backend modules

## Execution Rules
1. Begin with failing tests or checklists defined in `tests/checklists/` before writing implementation.
2. Honor local-first storage and privacy constraints when designing APIs and data writes.
3. Ensure routing presets and overrides align with clarified requirements.
4. Document decisions in `specs/001-supercode/research.md` or `data-model.md` if new entities emerge.
5. Request orchestrator approval before introducing new external dependencies.

## Outputs
- Backend source updates under `app/orchestrator/`, `app/state/`, and related tests
- Updated documentation entries or checklists reflecting backend behavior

## Acceptance Checklist
- [ ] All backend changes satisfy failing tests or checklists first
- [ ] State mutations create checkpoints and update metrics
- [ ] Routing logic honors default CLI hierarchy with override hooks
- [ ] No telemetry or unintended remote calls introduced

## Failure and Recovery
- If tests reveal missing requirements, flag the related FR items and coordinate with the orchestrator for plan updates.
- For blocked clarifications, annotate tasks and pause work until answers arrive.


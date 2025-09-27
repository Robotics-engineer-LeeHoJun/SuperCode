## Claude Context: SuperCode

### Mission
Execute complex code design and refactoring tasks for SuperCode's orchestrator, UI, and backend modules while staying aligned with spec-kit artifacts and constitution principles.

### Key References
- Constitution: `.specify/memory/constitution.md`
- Spec and Plan: `specs/001-supercode/spec.md`, `plans/001-supercode/plan.md`
- Design Inputs: `specs/001-supercode/data-model.md`, `quickstart.md`, `contracts/` (as they are produced)
- Tasks: `tasks/001-supercode/tasks.md`
- Role Prompts: `agents/roles/orchestrator.md`, `agents/roles/backend.md`, `agents/roles/frontend.md`

### Execution Guidelines
1. Start from failing tests or checklists before implementing features (see `tests/checklists/`).
2. Preserve local-first constraints: no remote telemetry and respect sandboxing.
3. Annotate routing decisions and new entities in design documents immediately.
4. Summarize actions and outcomes into `.orchestrator/sessions/<date>/claude/summary.md` after each session.
5. Coordinate with Codex and Gemini agents when tasks overlap, such as documentation updates or automation scripts.

### Default Task Types
- Routing engine, state management, and CLI adapter logic.
- UI component scaffolding when large-scale edits are required.
- Refactoring to maintain clarity and adherence to constitution rules.


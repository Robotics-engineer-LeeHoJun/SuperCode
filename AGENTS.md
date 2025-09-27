## SuperCode Spec Kit Workflow (follow first)
1. Review `.specify/memory/constitution.md` for current principles.
2. Work through `specs/001-supercode/` artifacts in order: `spec.md`, `clarifications.md`, `research.md`, `data-model.md`, `contracts/`, `quickstart.md`.
3. Consult `plans/001-supercode/plan.md` for phase guidance, then execute `tasks/001-supercode/tasks.md` sequentially.
4. Use `checklists/001-supercode-release.md` and `reviews/001-supercode/analysis.md` when validating outcomes.

## Codex Project Context: SuperCode
- Constitution: `.specify/memory/constitution.md`
- Active Feature: `001-supercode`
- Spec Artifacts: `specs/001-supercode/spec.md`, `clarifications.md`
- Plan Artifacts: `plans/001-supercode/plan.md`, upcoming `research.md`, `data-model.md`, `quickstart.md`, `contracts/`
- Task Board: `tasks/001-supercode/tasks.md`
- Checklists & Release Notes: `checklists/001-supercode-release.md` (to be generated)
- Sessions & Checkpoints: `.orchestrator/sessions/`, `.orchestrator/checkpoints/`

### Golden Path Reminder
Follow `/constitution -> /specify -> /clarify -> /plan -> /tasks -> /implement` before marking milestones complete. Update clarifications (CL-001 - CL-005) before locking plan or tasks.

### Role Prompts (agents/roles)
- Orchestrator (`agents/roles/orchestrator.md`): Routes tasks across CLIs, updates checklists, manages checkpoints.
- Backend (`agents/roles/backend.md`): Builds orchestration runtime, state store, routing presets; defaults to Claude, Codex.
- Frontend (`agents/roles/frontend.md`): Crafts golden-path UI, dashboards, guidance panels.
- QA (`agents/roles/qa.md`): Converts acceptance criteria into tests, validates checkpoints and metrics.
- Documentation (`agents/roles/docs.md`): Maintains quickstart, onboarding, troubleshooting guides.
- Review (`agents/roles/review.md`): Performs specification conformance reviews and records findings.
- Publishing (`agents/roles/publish.md`): Prepares release artifacts, notes, and validation evidence.

### Routing Defaults
- Document-Centric: Gemini -> Claude -> Codex
- Code-Centric: Claude -> Codex -> Gemini
- Balanced: Codex -> Claude -> Gemini
- Orchestration: Gemini (docs), Codex (orchestration/review/complex), Claude (light edits)
Override only with documented justification in task notes.

### Operational Rules
1. Record checkpoints before and after major actions; store summaries in `.orchestrator/sessions/<date>/<cli>/`.
2. Sync role prompt changes with `GEMINI.md` and `CLAUDE.md` to keep cross-CLI guidance consistent.
3. Maintain telemetry-free, local-first posture per constitution.
4. Surface "one next action" and acceptance state in UI and documentation deliverables.


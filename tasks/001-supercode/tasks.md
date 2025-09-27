# Tasks: SuperCode

**Input**: Design documents from `specs/001-supercode/`
**Prerequisites**: `plans/001-supercode/plan.md`, upcoming `research.md`, `data-model.md`, `contracts/`

## Phase 3.1: Setup and Alignment
- [ ] T001 Document resolved clarifications (CL-001 through CL-005) in spec, plan, and settings UI copy.
- [ ] T002 Draft `specs/001-supercode/research.md` summarizing tech stack decisions, routing presets, and retention policy rationale.
- [ ] T003 Record constitution alignment notes and research outcomes in `plans/001-supercode/plan.md` (Progress Tracking section).
- [ ] T004 [P] Establish project skeleton under `app/` (ui/, orchestrator/, cli-adapters/, state/, security/).
- [ ] T005 [P] Create `.orchestrator/` directories for sessions and checkpoints with a README describing log retention rules.

## Phase 3.2: Tests and Checklists First (TDD mindset)
- [ ] T006 Author acceptance checklist updates for onboarding, orchestration, and recovery in `specs/001-supercode/quickstart.md`.
- [ ] T007 Define data entities and validation rules in `specs/001-supercode/data-model.md`.
- [ ] T008 Produce contract descriptions for task routing, CLI login triggers, and MCP management inside `specs/001-supercode/contracts/`.
- [ ] T009 [P] Outline checkpoint and log retention verification scenarios in `tests/checklists/test_checkpoints.md` (failing placeholder).
- [ ] T010 [P] Outline orchestration routing validation scenarios in `tests/checklists/test_routing.md` (failing placeholder).

## Phase 3.3: Core Implementation
- [ ] T011 Implement project state store and metrics tracking within `app/state/` respecting retention limits.
- [ ] T012 Implement CLI adapter scaffolds for Codex, Gemini, and Claude under `app/cli-adapters/` while respecting native login flows.
- [ ] T013 Develop routing engine with default presets and override UI hooks in `app/orchestrator/`.
- [ ] T014 Build golden-path timeline UI with checklist enforcement in `app/ui/timeline/`.
- [ ] T015 Build beginner guidance panels and recovery prompts in `app/ui/guidance/`.
- [ ] T016 Integrate MCP server manager and connection tests in `app/ui/mcp/` and `app/orchestrator/mcp-manager.ts`.
- [ ] T017 Implement checkpoint creation, diff visualization, and one-click pre-session rollback in pp/state/checkpoints/ and pp/ui/checkpoints/.

## Phase 3.4: Integration and Validation
- [ ] T018 Wire CLI adapters into the routing engine, ensuring session logs are stored under `.orchestrator/sessions/`.
- [ ] T019 Connect UI dashboards (agents, terminals, context editors) to the state store and ensure synchronized AGENTS, GEMINI, and CLAUDE files.
- [ ] T020 Execute end-to-end onboarding flow, capturing metrics versus success criteria; adjust quickstart as needed.
- [ ] T021 Validate failover paths when a CLI session crashes or login fails, and confirm one-click rollback restores the pre-session checkpoint before resuming.

## Phase 3.5: Polish and Documentation
- [ ] T022 [P] Establish automated cleanup respecting the agreed log retention quota under `app/state/cleanup/`.
- [ ] T023 [P] Document usage in `docs/supercode-onboarding.md`, highlighting golden path and orchestration flows.
- [ ] T024 Update `AGENTS.md`, `GEMINI.md`, and `CLAUDE.md` with synchronized role prompt summaries once implementation stabilizes.
- [ ] T025 Prepare release validation checklist covering acceptance criteria and regression coverage in `checklists/001-supercode-release.md`.

## Dependencies
- Clarifications (T001) unblock research and design tasks (T002 through T010).
- Checklists and tests (T006 through T010) must exist and fail before implementation tasks (T011 through T017).
- Core implementation (T011 through T017) precedes integration validation (T018 through T021).
- Cleanup and documentation (T022 through T025) follow successful integration testing.

## Parallel Guidance
- Run T004 and T005 concurrently after T003 completes (different directories).
- T009 and T010 can proceed in parallel once contracts draft (T008) is ready.
- T022 and T023 may run together after integration validation (T018 through T021).

## Notes
- Maintain local-first storage and respect CLI login constraints in every task.
- Produce checkpoints before and after high-impact changes to align with Constitution Principle III.
- Update spec-kit artifacts immediately when clarifications close or scope changes occur.





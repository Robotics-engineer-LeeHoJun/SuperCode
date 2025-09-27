# Feature Specification: SuperCode Spec-Driven Orchestrator

**Feature Branch**: `001-supercode`  
**Created**: 2025-09-28  
**Status**: Draft  
**Input**: User description: "Desktop hub that integrates Codex CLI, Gemini CLI, and Claude Code with spec-kit golden path guidance"

## Execution Flow (main)
```
1. Parse user description from Input
   -> If empty: ERROR "No feature description provided"
2. Extract key concepts from description
   -> Identify: actors, actions, data, constraints
3. For each unclear aspect:
   -> Mark with [NEEDS CLARIFICATION: specific question]
4. Fill User Scenarios & Testing section
   -> If no clear user flow: ERROR "Cannot determine user scenarios"
5. Generate Functional Requirements
   -> Each requirement must be testable
   -> Mark ambiguous requirements
6. Identify Key Entities (if data involved)
7. Run Review Checklist
   -> If any [NEEDS CLARIFICATION]: WARN "Spec has uncertainties"
   -> If implementation details found: ERROR "Remove tech details"
8. Return: SUCCESS (spec ready for planning)
```

---

## Quick Guidelines
- Focus on WHAT users need and WHY
- Avoid HOW to implement (no tech stack, APIs, code structure)
- Written for business stakeholders, not developers

### Section Requirements
- Mandatory sections must be completed for every feature
- Optional sections should be included only when relevant to the feature
- When a section does not apply, remove it entirely (do not leave "N/A")

### For AI Generation
When creating this spec from a user prompt:
1. Mark all ambiguities using [NEEDS CLARIFICATION: specific question]
2. Do not guess missing details such as authentication style
3. Think like a tester: vague requirements fail the "testable and unambiguous" test
4. Watch for underspecified areas: user roles, data retention, performance, error handling, integrations, security

---

## User Scenarios & Testing (mandatory)

### Primary User Story
A junior developer launches SuperCode, initializes a new project, signs into the supported CLIs through guided dialogs, and follows the golden-path checklist to generate spec-kit artifacts while seeing role prompts and progress in one dashboard.

### Acceptance Scenarios
1. **Given** a fresh project folder, **When** the user presses "Project Initialize", **Then** SuperCode provisions spec-kit templates, creates baseline context files, and confirms readiness within 5 minutes.
2. **Given** `/tasks` contains at least two actionable items, **When** the user starts orchestration, **Then** SuperCode routes tasks to at least two different CLI sessions, captures their outputs, and updates the shared checklist state.

### Edge Cases
- What happens when a CLI login fails or the browser window is closed before completion?
- How does the system handle running more than six concurrent CLI sessions or exceeding configured resource limits?

## Requirements (mandatory)

### Functional Requirements
- **FR-001**: The system MUST present a golden-path timeline covering `/constitution`, `/specify`, `/clarify`, `/plan`, `/tasks`, and `/implement` with per-stage checklists.
- **FR-002**: The system MUST provide a single-click "Project Initialize" action that prepares spec-kit scaffolding and any missing context files when run in a new folder.
- **FR-003**: The system MUST surface the single next recommended action and explain why it matters to progress completion.
- **FR-004**: The system MUST allow users to launch Codex CLI, Gemini CLI, and Claude Code sessions via dedicated terminal tabs and keep their logs.
- **FR-005**: The system MUST guide users through each CLI's native web login process without capturing credentials directly.
- **FR-006**: The system MUST maintain synchronized content between role prompt files and the AGENTS, GEMINI, and CLAUDE context documents.
- **FR-007**: The system MUST capture pre- and post-session checkpoints for every CLI conversation or task completion and give users one-click rollback to the pre-session state, with diff previews available.
- **FR-008**: The system MUST route tasks to default CLI preferences based on task type while allowing user overrides.
- **FR-009**: The system MUST provide an MCP server manager to register, enable, disable, and test available providers.
- **FR-010**: The system MUST support automated or semi-automated execution of `/tasks` items and report success or failure outcomes.
- **FR-011**: The system MUST enforce the golden-path order, warning users when attempting to skip required steps.
- **FR-012**: The system MUST expose progress metrics such as onboarding completion time, orchestration results, and checkpoint success rate.
- **FR-013**: The system MUST store project data locally and avoid transmitting telemetry without explicit consent.
- **FR-014**: The system MUST provide recovery guidance when a task, login, or CLI session fails.
- **FR-015**: The system MUST support beginner-friendly explanations and examples for each workflow stage.
- **FR-016**: The system MUST retain logs and summaries for each CLI session within a discoverable project structure.
- **FR-017**: The system MUST expose dashboards for workflow progress, agent orchestration, terminal sessions, context editors, MCP configuration, and checkpoints.
- **FR-018**: The system MUST provide acceptance checklists for reviewing CLI-generated outputs before marking tasks complete.
- **FR-019**: The system MUST support task distribution across roles such as orchestrator, backend, frontend, QA, docs, review, and publish.
- **FR-020**: The system MUST allow the user to revisit and edit spec-kit artifacts directly from the UI at any time.
- **FR-021**: The system MUST ship with four routing presets: Document-Centric (Gemini primary, Claude secondary, Codex tertiary), Code-Centric (Claude primary, Codex secondary, Gemini tertiary), Balanced (Codex primary, Claude secondary, Gemini tertiary), and Orchestration (Gemini for documentation, Codex for orchestration/review/complex work, Claude for lightweight edits), and allow users to duplicate or edit them.
- **FR-022**: The system MUST retain checkpoints and session logs for 30 days by default, offer manual purge controls, and allow users to shorten retention in settings.
- **FR-023**: The system MUST provide checklist automation presets for Node.js (`npm test`), Python (`pytest`), and Rust (`cargo test`), while letting users configure additional commands per project.
- **FR-024**: The system MUST operate as a single-user, local-first application for the MVP, with collaboration features explicitly out of scope.
- **FR-025**: The system MUST surface links and guidance to official CLI GitHub Action templates without generating workflow files automatically.

### Key Entities (include if feature involves data)
- **Project State**: Tracks current phase, checklists, configured routing presets, linked artifacts, and metrics.
- **CLI Session**: Represents a spawned terminal context with login state, log artifacts, and routing metadata.
- **Checkpoint**: Captures snapshots of project files and orchestration context for rollback and auditing.
- **Role Prompt**: Defines guidance for specific agent roles and links to corresponding context files.
- **Task Item**: Stores description, owning role, assigned CLI, status (todo, in_progress, blocked, done), and acceptance evidence.

---

## Review & Acceptance Checklist
*Gate: automated checks run during main() execution*

### Content Quality
- [ ] No implementation details (languages, frameworks, APIs)
- [ ] Focused on user value and business needs
- [ ] Written for non-technical stakeholders
- [ ] All mandatory sections completed

### Requirement Completeness
- [ ] No [NEEDS CLARIFICATION] markers remain
- [ ] Requirements are testable and unambiguous
- [ ] Success criteria are measurable
- [ ] Scope is clearly bounded
- [ ] Dependencies and assumptions identified

---

## Execution Status
*Updated by main() during processing*

- [x] User description parsed
- [x] Key concepts extracted
- [x] Ambiguities marked
- [x] User scenarios defined
- [x] Requirements generated
- [x] Entities identified
- [ ] Review checklist passed

---




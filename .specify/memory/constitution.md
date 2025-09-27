# SuperCode Constitution

## Core Principles

### I. Local-First Privacy
SuperCode stores all user and project data locally, runs without telemetry, and isolates CLI processes to their minimum required permissions.

### II. Spec-Driven Golden Path
Every engagement follows the /constitution ? /specify ? /clarify ? /plan ? /tasks ? /implement sequence, and deliverables must be produced or updated before proceeding to the next phase.

### III. Visibility & Rollback
The application preserves checkpoints, timelines, and diffs for each workflow step to make current state transparent and allow instant restoration.

### IV. CLI Respect & Parity
Codex, Gemini, and Claude Code are orchestrated "as is" with their native login flows, context files, and MCP integrations. SuperCode never bypasses quotas, policies, or configuration conventions.

### V. Beginner-Friendly Orchestration
UI and automation must surface the single next action, explain rationale, and provide recovery paths so newcomers can complete the workflow confidently.

## Operating Constraints
- Web login flows remain delegated to each CLI; SuperCode only opens the corresponding browser windows.
- Context files (AGENTS.md, GEMINI.md, CLAUDE.md) stay authoritative, and SuperCode keeps them synchronized with role prompts.
- Security sandboxing is mandatory: limit file and network scopes per CLI session and record any escalation requests in logs.

## Development Workflow
1. Run spec-kit initialization or verification before capturing requirements.
2. Update memory artifacts and checklists as part of each golden-path command.
3. Use the orchestrator to assign tasks to role-specific agents and collect their outputs for review against acceptance criteria.
4. Capture checkpoints before and after automated actions, including PTY-launched CLI sessions.
5. Review outcomes against functional requirements and rerun or redirect tasks when acceptance gates fail.

## Governance
- The constitution supersedes ad-hoc practices; exceptions require documented approval and an amendment roadmap.
- Pull requests and reviews must verify compliance with the golden path, checklist status, and checkpoint integrity.
- Reference SuperCode role prompts and spec-kit artifacts in all Orchestrator-driven sessions to maintain shared context.

**Version**: 1.0.0 | **Ratified**: 2025-09-28 | **Last Amended**: 2025-09-28

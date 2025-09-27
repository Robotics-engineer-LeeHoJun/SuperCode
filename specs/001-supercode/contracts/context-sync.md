# Context Synchronization Contract

## Purpose
Maintain consistency between role prompt files (`agents/roles/*.md`) and CLI context documents (`AGENTS.md`, `GEMINI.md`, `CLAUDE.md`).

## Scope
- Detect updates in role prompt files.
- Propagate relevant sections to the CLI-specific context files.
- Ensure changes are captured in checkpoints for auditability.

## Workflow
1. **Monitor Changes**: File watcher observes `agents/roles/` for modifications.
2. **Extract Content**: Parse each prompt and map key sections (Purpose, Inputs, Execution Rules, Acceptance Checklist, Failure & Recovery).
3. **Transform**: Format extracted guidance per CLI context requirements (e.g., emphasize documentation tasks for Gemini).
4. **Apply Updates**:
   - Update `AGENTS.md` with consolidated view.
   - Update `GEMINI.md` and `CLAUDE.md` with role-specific instructions.
5. **Persist**: Store last synchronization timestamp and checksum in the `RolePrompt` entity.
6. **Checkpoint**: Create a checkpoint whenever context files are updated to allow rollback if content drifts.

## Error Handling
- On parse failures, alert the user and leave previous context intact while logging error details.
- If target file write fails, retry once; otherwise block related tasks and solicit user intervention.

## Audit Requirements
- Record each synchronization event in `.orchestrator/sessions/<date>/system/sync.log` with role name, affected files, and checksum.
- Provide diff preview before applying updates when triggered manually via UI.

## User Controls
- Allow manual sync trigger and the ability to exclude specific roles from automatic propagation.
- Offer preview and confirm dialogs to reassure users before context files are modified.


# Orchestrator Routing Contract

## Purpose
Define how the SuperCode orchestrator selects CLI agents for tasks, applies routing presets, and records overrides in project state.

## Inputs
- Active routing preset from `ProjectState.routing_preset`
- Task metadata (`TaskItem.role`, `TaskItem.description`, `TaskItem.preferred_cli`)
- Manual override instructions supplied by the user through the UI

## Outputs
- CLI assignment for each task execution request
- Log entry referencing the chosen CLI and rationale
- Optional override record captured in project state notes

## CLI Preferences by Preset
| Preset | Priority Order | Notes |
|--------|----------------|-------|
| Document-Centric | Gemini -> Claude -> Codex | Emphasize research, summarization, and documentation tasks. |
| Code-Centric | Claude -> Codex -> Gemini | Favor code-heavy automation and refactoring. |
| Balanced | Codex -> Claude -> Gemini | Default choice; mixes local automation with editing. |
| Orchestration | Gemini (documentation), Codex (orchestration/review/complex), Claude (lightweight edits) | Optimized for multi-agent coordination where Codex handles coordination and code review, Gemini handles documentation, and Claude handles quick edits. |

## Routing Rules
1. Start with `TaskItem.preferred_cli` if provided and compatible with the active preset.
2. If no preference, apply preset priority order; pick the first CLI that is available (not failed/blocked).
3. Record the final choice along with preset name and any manual comments in session log metadata.
4. When user overrides routing, prompt for a short justification and store it in `TaskItem.notes`.
5. If all CLIs in priority order are unavailable, flag the task as blocked and surface recovery steps (e.g., retry login).

## Error Handling
- On CLI failure: attempt reroute to next CLI in preset, notify orchestrator, and record failure in metrics.
- On repeated failure/override conflicts: escalate to user with suggestions to adjust presets or clarify task.

## Checklist Integration
- Ensure that each routing decision updates the task checklist and session metrics.
- Confirm that acceptance checklists include routing verification for QA and review agents.



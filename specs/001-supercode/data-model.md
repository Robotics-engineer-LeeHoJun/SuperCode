# Data Model: SuperCode

## Entities Overview
| Entity | Purpose |
|--------|---------|
| ProjectState | Captures global progress, routing preset, metrics, and links to artifacts for the active workspace. |
| TaskItem | Represents an actionable unit sourced from `tasks/001-supercode/tasks.md`, including status, role, and CLI assignment. |
| Session | Tracks each CLI process (Codex, Gemini, Claude) launched by SuperCode, including PTY configuration and logs. |
| Checkpoint | Stores snapshots of project files and summarizes changes for rollback and auditing. |
| RolePrompt | Synchronizes role guidance across `agents/roles/*.md` and context files (AGENTS/GEMINI/CLAUDE). |
| Metric | Persists onboarding time, task success rates, checkpoint reliability, and other success indicators. |

## ProjectState
| Field | Type | Description |
|-------|------|-------------|
| id | UUID | Unique identifier for the project record. |
| phase | TEXT | Current golden-path phase (`constitution`, `specify`, `clarify`, `plan`, `tasks`, `implement`). |
| step | TEXT | Specific step within the phase (optional detail). |
| routing_preset | TEXT | Active routing preset name (Document-Centric, Code-Centric, Balanced, or custom). |
| checklist_json | JSON | Serialized checklist status (todo, in_progress, blocked, done) with timestamps and owners. |
| clarifications_resolved | BOOLEAN | Indicates whether all clarifications have answers recorded. |
| config_path | TEXT | Link to project configuration (e.g., `supercode.config.json`). |
| created_at | DATETIME | When the project state record was initialized. |
| updated_at | DATETIME | Last update timestamp. |

## TaskItem
| Field | Type | Description |
|-------|------|-------------|
| id | UUID | Unique task identifier. |
| project_state_id | UUID (FK) | References `ProjectState`. |
| task_id | TEXT | Original task label (e.g., T001). |
| description | TEXT | Human-readable task summary. |
| role | TEXT | Assigned role (orchestrator, backend, frontend, qa, docs, review, publish). |
| preferred_cli | TEXT | Default CLI for the task (Codex, Claude, Gemini). |
| status | TEXT | One of `todo`, `in_progress`, `blocked`, `done`. |
| notes | TEXT | Additional context, including reroutes or clarifications. |
| updated_at | DATETIME | Timestamp of last status update. |

## Session
| Field | Type | Description |
|-------|------|-------------|
| id | UUID | Unique session identifier. |
| project_state_id | UUID (FK) | References `ProjectState`. |
| cli | TEXT | `codex`, `gemini`, or `claude`. |
| started_at | DATETIME | Session start time. |
| ended_at | DATETIME | Session end time (null when active). |
| status | TEXT | `active`, `completed`, `failed`. |
| prompt_summary | TEXT | Brief summary of initial prompt injected. |
| log_path | TEXT | Filesystem path under `.orchestrator/sessions/<date>/<cli>/` storing streamed stdout/stderr. |
| checkpoint_before_id | UUID (FK) | Checkpoint taken before session start. |
| checkpoint_after_id | UUID (FK) | Checkpoint captured after completion; enables instant rollback preview and acceptance. |
| rollback_token | TEXT | Deterministic identifier used to expose "restore to pre-session" actions in the UI. |
| retained | BOOLEAN | Indicates whether logs are kept beyond the 30-day retention (manual override). |

## Checkpoint
| Field | Type | Description |
|-------|------|-------------|
| id | UUID | Unique checkpoint identifier. |
| project_state_id | UUID (FK) | References `ProjectState`. |
| session_id | UUID (FK) | Optional link to `Session` that triggered checkpoint creation. |
| label | TEXT | Human-readable marker (e.g., "Pre-T012 CLI Routing Update"). |
| created_at | DATETIME | Checkpoint creation time. |
| snapshot_path | TEXT | Directory under `.orchestrator/checkpoints/<stamp>/`. |
| diff_summary | TEXT | Overview of changes captured. |
| retention_expiry | DATETIME | Scheduled deletion time (created_at + 30 days unless overridden). |
| manual_override | BOOLEAN | True when user pinned the checkpoint to avoid cleanup. |


## RolePrompt
| Field | Type | Description |
|-------|------|-------------|
| id | UUID | Unique role prompt identifier. |
| role | TEXT | Role name (orchestrator, backend, etc.). |
| file_path | TEXT | Source markdown path in `agents/roles/`. |
| context_targets | JSON | Linked context files that must mirror key guidance (AGENTS, GEMINI, CLAUDE). |
| last_synced_at | DATETIME | Last time synchronization job updated context files. |
| checksum | TEXT | Hash of the prompt content to detect changes. |

## Metric
| Field | Type | Description |
|-------|------|-------------|
| id | UUID | Unique metric record identifier. |
| project_state_id | UUID (FK) | References `ProjectState`. |
| name | TEXT | Metric key (e.g., onboarding_duration_minutes, tasks_pass_rate, checkpoint_replay_success). |
| value | REAL | Numeric metric value. |
| captured_at | DATETIME | Timestamp when metric was recorded. |
| notes | TEXT | Additional observations or references to sessions/checkpoints. |

## Relationships
- `ProjectState` 1 — n `TaskItem`
- `ProjectState` 1 — n `Session`
- `ProjectState` 1 — n `Checkpoint`
- `ProjectState` 1 — n `Metric`
- `Session` optionally links to two `Checkpoint` records for before/after snapshots.
- `RolePrompt` synchronizes textual guidance but remains decoupled from `ProjectState` so prompts can persist across projects if needed.



# Session Management Contract

## Purpose
Standardize how SuperCode spawns CLI sessions, captures PTY streams, persists logs, and associates checkpoints.

## Participants
- Orchestrator service (Rust backend)
- Frontend terminal view (React + xterm.js)
- SQLite datastore tracking sessions and checkpoints

## Session Lifecycle
1. **Initialize**: Orchestrator receives a routing decision and prepares PTY using `portable-pty` with `tokio` process management.
2. **Snapshot Before Session**: Create a checkpoint, persist it to SQLite, and associate `checkpoint_before_id` so users can roll back if results are rejected.
3. **Spawn**: Launch CLI executable (`codex`, `gemini`, or `claude`) with environment set according to project settings.
4. **Stream**: Pipe stdout/stderr through IPC to frontend, rendering in the embedded terminal. Also tee output to log file under `.orchestrator/sessions/<date>/<cli>/<run-id>/stdout.log` and `stderr.log`.
5. **Interact**: Frontend injects user or automated prompts via orchestrator, which writes to PTY stdin.
6. **Monitor**: Orchestrator tracks exit codes, heartbeats, and inactivity thresholds; session metadata updates in SQLite.
7. **Finalize**: On completion, orchestrator records result summary, creates a post-session checkpoint, and updates task/checklist status. Provide a UI action referencing `rollback_token` to restore to the pre-session checkpoint.

## Required Fields
- `Session.id`, `Session.cli`, `Session.started_at`, `Session.status`
- `log_path` for session directory
- `checkpoint_before_id` and `checkpoint_after_id`
- `retention_expiry` inherits from checkpoint policy unless manual override is flagged

## Constraints
- No external terminal windows; all interaction remains within SuperCode UI.
- Heartbeat interval defaults to 30 seconds; sessions without output for 5 minutes trigger a warning dialog.
- User-initiated termination must capture a checkpoint and reason for stopping.

## Error Handling
- If spawn fails, mark the session as `failed`, capture stdout/stderr, and prompt user to verify CLI login.
- On PTY errors, attempt a single restart; if it fails again, escalate to user with troubleshooting guide.
- Log retention respects the 30-day rule unless user toggles `retained=true`.

## Checklist & Metrics
- Record session duration, number of prompts exchanged, and exit code for metrics dashboard.
- Update QA checklist when failover or retry logic was exercised.




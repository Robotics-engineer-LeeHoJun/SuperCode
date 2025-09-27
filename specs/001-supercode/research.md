# Research Notes: SuperCode

## Summary
SuperCode will be delivered as a Tauri desktop application with a React UI. The backend responsibilities—CLI orchestration, PTY management, state persistence, and checkpointing—remain inside the Rust runtime to maximize reliability and maintain SuperCode's local-first philosophy.

## Key Decisions
| Area | Decision | Rationale |
|------|----------|-----------|
| GUI Stack | Tauri (Rust backend) + React (frontend) | Aligns with opcode lineage, compiles to lightweight desktop builds, and keeps UI logic familiar for web developers. |
| PTY & Process Control | Use Rust `portable-pty` with `tokio` async runtime inside the Tauri backend | Eliminates an extra Node bridge, keeps process ownership on the Rust side, reduces race conditions, and ensures consistent logging across platforms. |
| CLI Display | Embed terminal panels (e.g., React + xterm.js) that stream PTY output from Rust over Tauri IPC | Prevents external terminal popups, keeps all sessions in-app, and avoids disrupting the user's workspace. |
| State & Checkpoints | Store project state, metrics, and checkpoint manifests in SQLite (file-per-project) | Single-file DB suits offline workflow, supports transactions for checkpoints, and is easy to snapshot/backup. |
| Automation | Run test/checklist automation through configurable script entries (defaulting to `npm test`, `pytest`, `cargo test`) | Matches clarified FR-023 and lets users add custom commands without code changes. |

## Implementation Notes
- PTY Layer: `portable-pty` provides cross-platform pseudo-terminal support; pair with `tokio::process` for async control. Expose session handles through a Rust service that streams stdout/stderr to the frontend and accepts prompt injections.
- Terminal UI: Use `xterm.js` or similar React component to render session feeds. Provide copy/save log controls and tie each session to `.orchestrator/sessions/...` snapshots.
- SQLite Integration: Wrap access in a light ORM (e.g., `sea-orm` or `sqlx`) to track phases, tasks, metrics, and checkpoint metadata. Store raw file diffs inside checkpoint directories; keep DB for metadata only.
- Script Automation: Maintain a configuration file (e.g., `supercode.config.json`) where users can edit or add automation commands per project. The orchestrator will invoke scripts via the Rust backend and capture exit codes/logs.

## Alternatives Considered
- **Node PTY bridge (`node-pty`)**: Simpler for React developers but adds a parallel process layer, increases memory footprint, and complicates logging synchronization between Rust and JS.
- **Pure JSON/flat-file state**: Easier to inspect but harder to query for metrics and progress dashboards; transactional integrity for checkpoints is weaker.
- **External terminal windows**: Provides native shell features but breaks UX (steals focus) and conflicts with the requirement to keep the workspace unobstructed.

## Open Items
- Decide on the React terminal component (`xterm.js` vs. `@alinea/term` etc.) and styling to keep sessions readable in light/dark modes.
- Define the SQLite schema (tables for phases, tasks, sessions, checkpoints, metrics) and migration approach.
- Determine how automation scripts are registered in the UI and persisted to project config.
- Design UI affordances for one-click pre-session rollback, including diff preview and confirmation flow.



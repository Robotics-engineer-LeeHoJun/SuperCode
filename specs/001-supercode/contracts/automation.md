# Task Automation Contract

## Purpose
Define how SuperCode executes scripted automation (tests, linting, health checks) in alignment with routing presets and checklist expectations.

## Sources
- Automation presets: Node.js (`npm test`), Python (`pytest`), Rust (`cargo test`)
- User-defined commands stored in project configuration (`supercode.config.json`)
- Task definitions referencing automation requirements (e.g., T009, T010)

## Execution Steps
1. **Selection**: For each task requiring automation, choose the command from presets or the user-defined list based on project language tags.
2. **Preparation**: Ensure working directory matches project root or task-specific path. Capture pre-execution checkpoint if configured.
3. **Invocation**: Run the script via Rust backend using `tokio::process` without spawning external terminals; stream output to UI panel.
4. **Result Capture**: Record exit status, stdout/stderr logs, and update the relevant TaskItem status.
5. **Post-Processing**: If automation succeeds, mark associated checklist items as done; if it fails, flag task as blocked and attach log excerpt.

## Configuration Schema (`supercode.config.json` excerpt)
```
{
  "automation": {
    "presets": {
      "node": "npm test",
      "python": "pytest",
      "rust": "cargo test"
    },
    "custom": [
      { "label": "lint", "command": "npm run lint" },
      { "label": "format", "command": "cargo fmt -- --check" }
    ]
  }
}
```

## Error Handling
- If command is missing, prompt user to configure it or skip with acknowledgment.
- On failure, capture logs, create a checkpoint, and notify QA agent for follow-up.
- Respect retention and cleanup policies by pruning automation logs after 30 days unless pinned.

## Metrics
- Track automation success rate (`tasks_pass_rate` metric).
- Record average execution time per command for UI dashboard.


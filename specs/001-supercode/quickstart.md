# Quickstart: SuperCode Golden Path

## Goal
Guide a first-time user through launching SuperCode, initializing a project, completing the golden-path stages up to task orchestration, and understanding how to roll back to the state before the last CLI conversation.

## Prerequisites
- Desktop environment with Tauri runtime requirements met (Windows/macOS/Linux).
- Codex CLI, Gemini CLI, and Claude Code installed and ready for web-based login.
- `spec-kit` templates available (SuperCode can bootstrap them during initialization).

## Step 1: Launch & Project Selection
1. Open SuperCode.
2. Choose **Create New Project** or select an existing workspace.
3. Confirm the project path is local; SuperCode warns if remote or network drives are used.

## Step 2: Initialize Spec Kit
1. Press **Project Initialize**.
2. SuperCode runs `specify init` (or `--here` when needed) and reports results in the Activity pane.
3. Verify that `.specify/`, `.codex/`, and context files (AGENTS/GEMINI/CLAUDE) appear in the project tree.

## Step 3: Constitution & Clarifications
1. Navigate to the **Constitution** tab and review the core principles. Adjust only if governance requires it.
2. Proceed to **/specify** to generate or refresh `specs/001-supercode/spec.md`.
3. Open the **Clarifications** panel and answer outstanding items (CL-001 ? CL-005 already resolved in this project template).

## Step 4: Plan Overview
1. Switch to the **Plan** tab and confirm `plans/001-supercode/plan.md` is present.
2. Review Phase 0 and Phase 1 reminders, paying attention to tasks that reference research, data models, contracts, and automation presets.

## Step 5: Login to CLIs (One Time per Session)
1. Codex: Click **Sign in with ChatGPT**. A browser window opens; authenticate and return to SuperCode.
2. Gemini: Click **Login with Google** to complete OAuth.
3. Claude: Launch the CLI session from the terminal panel; follow the Anthropic login prompt.
4. SuperCode reflects login success in the **Terminal** dashboard and stores session metadata in SQLite.

## Step 6: Run Golden Path Actions
1. In the **Workflow Timeline**, select `/constitution`, `/specify`, `/clarify`, `/plan`, and `/tasks` sequentially.
2. For each command:
   - Review the suggested prompt.
   - Press **Run Command** to spawn the corresponding CLI session.
   - Observe the in-app terminal for output; SuperCode creates a checkpoint before and after each session.
3. Complete checklist items as CLI sessions finish.

## Step 7: Orchestrate Tasks
1. Open **Tasks & Agents**.
2. Review the task board generated from `tasks/001-supercode/tasks.md`.
3. Press **Start Orchestration**. SuperCode assigns tasks based on the active routing preset (Balanced by default) and opens dedicated CLI tabs.
4. Track status updates and metrics in real time; adjust routing if needed via the override dialog.

## Step 8: Roll Back if Results Are Unsatisfactory
1. From the **Checkpoints** panel, locate the entry labeled `Pre-<Task/Session>` (created before the most recent CLI conversation).
2. Click **Preview Diff** to review changes.
3. If you want to revert, hit **Restore to Pre-Session**. SuperCode applies the snapshot and updates the task/checklist back to the prior state.
4. A post-restore checkpoint is created so you can undo the rollback if necessary.

## Step 9: Continue or Exit
- Repeat orchestration cycles until `/tasks` are complete.
- When ready for implementation, proceed to `/implement` or hand off to responsible agents.
- Exit SuperCode; all state and logs remain local in `.orchestrator/` and the SQLite database.

## Troubleshooting
- **CLI login issues**: Use the Login panel to re-trigger OAuth; SuperCode links to official documentation under Help.
- **Automation failure**: Inspect `stdout.log` in `.orchestrator/sessions/<date>/<cli>/` and adjust commands via `supercode.config.json`.
- **Rollback conflicts**: If Git state diverges, SuperCode prompts to stash or abort before applying the checkpoint.
- **Retention management**: Use the Settings panel to shorten the 30-day window or manually prune logs/checkpoints.


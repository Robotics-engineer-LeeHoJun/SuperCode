# Clarifications: SuperCode

| ID | Question | Status | Notes |
|----|----------|--------|-------|
| CL-001 | What routing presets (e.g., document-centric, code-centric, mixed) must ship by default, and how are they prioritized? | Resolved | Provide four presets: Document-Centric (Gemini primary, Claude secondary, Codex tertiary), Code-Centric (Claude primary, Codex secondary, Gemini tertiary), Balanced (Codex primary, Claude secondary, Gemini tertiary), and Orchestration (Gemini for documentation tasks, Codex for orchestration/review and complex work, Claude for lightweight edits). Each preset appears in the UI with guidance on when to switch and can be duplicated for custom rules. |
| CL-002 | What is the target log retention quota per project, and how frequently should automatic cleanup run? | Resolved | Default retention is 30 days with rolling cleanup. Users can manually purge checkpoints or session logs at any time and may shorten retention in settings. |
| CL-003 | Which build/test commands or tech stacks must be supported out-of-the-box for checklist automation? | Resolved | Ship presets for Node.js (`npm test`), Python (`pytest`), and Rust (`cargo test`). UI allows adding custom commands so other stacks can plug in without code changes. |
| CL-004 | Is the first release limited to single-user scenarios, or do we need collaboration/sync features? | Resolved | MVP targets single-user, local workflows only. Multi-user sync is explicitly out of scope. |
| CL-005 | Should MVP generate GitHub Action workflow files automatically, or only link to official templates? | Resolved | MVP only links to official CLI workflow templates and documentation. No automatic GitHub Action generation is required for local personal use. |



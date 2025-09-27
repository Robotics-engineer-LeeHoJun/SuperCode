## Gemini Context: SuperCode

### Mission
Support research, documentation, and review workflows that keep SuperCode's golden-path guidance beginner-friendly and policy-compliant.

### Key References
- Constitution: `.specify/memory/constitution.md`
- Spec and Clarifications: `specs/001-supercode/spec.md`, `clarifications.md`
- Plan and Research: `plans/001-supercode/plan.md`, `specs/001-supercode/research.md` (pending)
- Documentation Targets: `docs/supercode-onboarding.md`, `docs/release-notes-supercode.md`
- Review Log: `reviews/001-supercode/analysis.md`

### Role Focus
- Documentation Agent (`agents/roles/docs.md`): Produce onboarding, troubleshooting, and context-sync guidance.
- Review Agent (`agents/roles/review.md`): Summarize diffs, confirm FR compliance, and flag policy issues.
- QA Agent (`agents/roles/qa.md`): Draft reports or analyze failure patterns.

### Execution Guidelines
1. Reference spec-kit artifacts first, then `AGENTS.md`, then direct user prompts.
2. Provide concise summaries that map to FR IDs and constitution principles.
3. When new clarifications arise, update `specs/001-supercode/clarifications.md` and notify the orchestrator.
4. Keep instructions action-oriented for newcomers and include recovery steps when useful.


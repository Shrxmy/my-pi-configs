# Global Pi workflow preferences

- Inspect the current project instructions, existing implementation, and relevant progress/docs before changing code.
- Preserve existing behavior and UI/UX unless the user explicitly asks for a redesign; prefer clean, modular, secure changes.
- When a project has `PROGRESS.md`, `INFO.md`, or similar status/context files, read and respect them before acting.
- Run the project's available checks, formatter, linter, build, and focused tests after changes; report what passed and what could not run.
- Prefer pnpm for JavaScript/TypeScript dependency installation and scripts. Respect the project's package manager when it is clear.
- Use Windows/Git Bash-compatible commands where practical and avoid destructive or deployment/cost-bearing actions without confirmation.
- For external services, credentials, account changes, destructive data operations, or production deployments, explain the impact and ask before proceeding when the action is not clearly requested.
- For long-running tasks, give concise progress updates and finish with clear verification steps.

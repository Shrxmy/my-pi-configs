# Global Pi workflow preferences

- Inspect the current project instructions, existing implementation, and relevant progress/docs before changing code.
- Preserve existing behavior and UI/UX unless the user explicitly asks for a redesign; prefer clean, modular, secure changes.
- When a project has `PROGRESS.md`, `INFO.md`, or similar status/context files, read and respect them before acting.
- Run the project's available checks, formatter, linter, build, and focused tests after changes; report what passed and what could not run.
- Prefer pnpm for JavaScript/TypeScript dependency installation and scripts. Respect the project's package manager when it is clear.
- Use Windows/Git Bash-compatible commands where practical and avoid destructive or deployment/cost-bearing actions without confirmation.
- For external services, credentials, account changes, destructive data operations, or production deployments, explain the impact and ask before proceeding when the action is not clearly requested.
- For long-running tasks, give concise progress updates and finish with clear verification steps.
- For substantial or ambiguous work, align on the desired outcome and important edge cases before implementation. Ask only questions that materially affect the result; do not turn small, clear requests into a discovery interview.
- Treat supplied screenshots, references, and existing screens as design constraints. Preserve the established visual language and keep UI changes scoped to the request unless a wider redesign is explicitly approved.
- For backend changes, verify the full request-to-storage-to-notification flow, authorization boundaries, idempotency, and deployment-specific URL/configuration behavior where relevant. Prefer observable, recoverable behavior over silent success.
- Work in small, verifiable slices. Use failing tests first when practical, then run focused tests plus the project's broader checks. Review the final diff for unintended changes and preserve pre-existing user edits.
- Delegate independent work to Pi subagents when useful. Prefer the Pi harness with `openai-codex/gpt-6-luna` at `high` reasoning effort; use another model or harness only when unavailable or a task-specific capability justifies it, and report that fallback.
- Keep final updates concise: summarize the outcome, key files, verification, and any remaining risks or manual steps.

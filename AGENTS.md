# Project Instructions

- Use English for project documentation, code, identifiers, and comments. User-facing conversations may use the user's preferred language. The app must support Polish and English as described in the specification.
- Consult `docs/product-specification.md` for product requirements and decisions relevant to the task. Use `docs/implementation-roadmap.md` for the agreed order of work. Treat planned features as plans, not as implemented functionality.
- Build the local Android MVP first: Flutter with SQLite, usable without an account or backend. Add the Spring Boot/PostgreSQL backend, accounts and synchronization later, followed by the React coaching dashboard and social features.
- Preserve offline-first operation. Use stable client-generated UUIDs and design persisted data and deletions for future synchronization as specified. Keep domain models independent of local database details.
- Keep planned workouts separate from performed sessions. Changes to or deletion of a plan must not change or remove recorded workout history. Preserve the exercise-sharing and renaming behavior described in the specification.
- Respect the specified consent and access rules when implementing coaching or location features. Do not expose private trainee data merely because a coach relationship exists.
- Keep project documentation aligned with accepted decisions. Mark unresolved details as open rather than inventing product requirements.
- Run checks appropriate to the change and report any that could not be run. Do not commit secrets, tokens, passwords, or private configuration.

## Commits

- Create commits only when requested or otherwise authorized; do not include unrelated or pre-existing user changes.
- Keep each commit focused on one logical change and review the staged diff before committing.
- Write commit messages in English using Conventional Commits, for example `feat: add workout history`, `fix: preserve active session`, or `docs: update product specification`.
- Use a short imperative subject; add a body when context, trade-offs, or breaking changes need explanation.
- Run relevant checks before committing when feasible, and report any checks that were not run or failed.

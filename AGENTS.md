# AGENTS.md

This repository is the home for **Tero**, a project-aware AI orchestrator.

Tero coordinates coding harnesses, accounts, tasks, visible terminal sessions, and git worktrees from a global control tower.

## Operating language

Use English for all repository content, task files, prompts, config keys, logs, and agent instructions.

User conversation may happen in any language.

## Current status

This repo is in product/design phase.

Start with:

- `docs/product.md`
- `docs/architecture.md`
- `docs/config.md`

Do not assume implementation exists unless files show it exists.

## Core product rules

- Tero is passive by default.
- When started in global mode, greet briefly and wait for user intent.
- Do not scan repositories, open terminals, create tasks, or mutate files unless explicitly asked.
- Every confirmed action creates a formal task.
- Tasks live only under global state, e.g. `~/.tero/tasks`.
- Do not create repo-local `.tero/` directories by default.
- Do not modify project `.gitignore` files just to support Tero.
- Coding tasks must use git worktrees under `~/.tero/worktrees`.
- Never merge, apply, push, or integrate changes without explicit user approval.
- If a request is ambiguous, infer the likely target and ask for confirmation before acting.
- Before delegation, confirm project, account, agent/harness, visible backend, and worktree path.

## Generic/open-source design

Tero must not hardcode the author's local paths, accounts, or workflow.

Users configure:

1. accounts
2. agents/harnesses
3. project roots
4. projects
5. visible backend preference

The author's setup can be used as examples only, not as product defaults.

## Harness direction

The first target harness is Pi.

Hermes support is planned, but design should stay harness-agnostic where practical.

## Visible terminal backends

Tero should support pluggable visible backends.

Initial targets:

- Orca
- Herdr

If no backend is configured, Tero should ask the user to choose and then save the preference.

## Documentation expectations

When changing product behavior, update the relevant docs in `docs/`.

Keep docs concise and decision-oriented.

## Safety expectations

- Respect account/project boundaries.
- Validate configured account alignment before operating on a repository.
- Never route work to an agent that is not allowed for the selected project.
- Do not silently fall back across account boundaries.

## Development notes

Prefer small, explicit changes.

If implementation begins, keep product concepts separated from adapters:

- task store
- config loader
- project resolver
- agent adapter
- visible backend adapter
- git worktree manager

# Tero Product Notes

Tero is a personal, project-aware AI orchestrator.

It helps a user coordinate multiple coding harnesses and accounts without mixing project context, GitHub identities, or agent permissions.

## Product shape

Tero should support different entry harnesses. The first planned targets are Pi and Hermes.

- Pi mode: coding-oriented control tower.
- Hermes mode: task/kanban-oriented control tower.

The implementation must be configurable for any user and must not assume a specific local setup, account structure, or directory layout.

## Core principles

### Passive by default

When started in global mode, Tero should only greet the user and wait.

It must not scan repositories, list tasks, open terminals, or mutate files unless the user asks.

### Global control tower

Tero has a global home, separate from the repositories it orchestrates.

When an entry harness is started from a Tero home checkout, it should behave as Tero's global control tower instead of assuming the current directory is the target project.

### Every action creates a task

Tero should not perform loose work.

Once the user confirms an action, Tero creates a formal task under the global Tero home state.

### Confirm before execution

Before delegating or mutating anything, Tero confirms:

- task summary
- project
- account
- agent/harness
- visible backend
- worktree path, when applicable

### Infer and confirm

If a user request is ambiguous, Tero may infer the intended project/account/agent, but it must ask for confirmation before acting.

### English operational language

All generated task files, prompts, logs, config keys, and agent instructions should be in English.

The user-facing conversation can happen in any language.

## MVP scope

The MVP focuses on Pi as the first entry harness.

It should support:

- global mode from a Tero home checkout
- global task storage
- manually configured accounts
- manually configured agents/harnesses
- manually configured projects
- visible terminal backend selection
- git worktree isolation for coding tasks
- explicit approval before merge/apply

## Non-goals for MVP

- automatic repo scanning
- web dashboard
- TUI dashboard
- automatic merge
- repo-local `.tero/` folders
- modifying project `.gitignore` files

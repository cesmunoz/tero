# Tero Product Notes

Tero is a personal, project-aware AI orchestrator.

It helps a user coordinate multiple coding harnesses and accounts without mixing project context, GitHub identities, or agent permissions.

## Product shape

Tero should support different entry harnesses, starting with Pi and later Hermes.

- Pi mode: coding-oriented control tower.
- Hermes mode: task/kanban-oriented control tower.

The implementation should not assume the author's local setup. It must be configurable for any user.

## Core principles

### Passive by default

When started in global mode, Tero should only greet the user and wait.

It must not scan repositories, list tasks, open terminals, or mutate files unless the user asks.

### Global control tower

Tero has a global home, separate from product repositories.

For this repo:

```txt
/home/ces/Work/cm/tero
```

When Pi is started from this repo, it should behave as Tero's global control tower.

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

The MVP focuses on Pi as an entry harness.

It should support:

- global mode from this repo
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

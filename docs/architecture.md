# Tero Architecture

Tero is composed of a global state directory, a repo/home with instructions and skills, and adapters for harnesses, agents, and visible terminal backends.

## High-level model

```txt
User
  ↓
Entry harness: Pi or Hermes
  ↓
Tero control layer
  ↓
Task store + config
  ↓
Project resolver
  ↓
Agent/harness adapter
  ↓
Visible terminal backend
  ↓
Isolated git worktree
```

## Global home

Tero keeps user state outside product repositories:

```txt
~/.tero/
  config.yaml
  accounts.yaml
  agents.yaml
  projects.yaml
  tasks/
  worktrees/
  logs/
```

Tero should not create repo-local state by default.

## Control tower repo

This repository is the orchestrator home during development:

```txt
/home/ces/Work/cm/tero
```

Starting Pi here should put the assistant in global control tower mode.

## Task storage

Tasks live only in the global state directory:

```txt
~/.tero/tasks/<task-id>/
  task.yaml
  brief.md
  evidence.md
  log.md
  artifacts/
```

Suggested meanings:

- `task.yaml`: structured metadata for CLI/web/TUI use.
- `brief.md`: user intent, scope, assumptions, and confirmed execution plan.
- `evidence.md`: outputs, test results, summaries, links, diffs.
- `log.md`: chronological events.
- `artifacts/`: patches, command logs, screenshots, reports.

## Worktree isolation

All coding tasks use git worktrees.

```txt
~/.tero/worktrees/<project-id>/<task-id>/
```

The original checkout should not be modified directly during delegated work.

## Merge/apply policy

Tero may prepare changes, run checks, and summarize results.

Tero must not merge, apply, push, or otherwise integrate changes without explicit user approval.

## Config model

Tero should be generic and open-source friendly.

It should not hardcode `work`, `personal`, local paths, or specific accounts.

Users configure:

1. accounts
2. agents/harnesses
3. project roots
4. projects
5. visible backend preference

## Visible terminal backends

Tero should support pluggable visible backends:

- Orca
- Herdr
- later: tmux, zellij, headless

If no visible backend is configured, Tero asks the user to choose before first delegation and stores the choice.

Backend interface concept:

```txt
createSession(task, worktree, command)
sendMessage(sessionId, text)
readOutput(sessionId)
waitUntilDone(sessionId)
open(sessionId)
```

## Agent routing

Projects define allowed agents and defaults.

Tero never routes a task to an agent that is not allowed for the selected project.

Example policy from the initial user setup:

- work projects: default `claude-work`, fallback `opencode-work`
- personal projects: default `pi-personal`, fallback only when needed

This is user config, not a hardcoded product assumption.

## Account validation

Before operating on a repo, Tero validates the repo/account alignment.

Validation may include:

- expected GitHub account
- `git config user.email`
- active `gh` account, where available
- project allowed agents

On mismatch, Tero blocks and asks the user to fix or explicitly decide what to do.

## Future web dashboard

A web dashboard, inspired by Kody, can read from `~/.tero` and provide a global view of:

- projects
- tasks
- task status
- agents
- evidence
- terminal/session links

This is not required for the first MVP.

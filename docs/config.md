# Tero Configuration

Tero should use explicit user configuration. The examples below are illustrative and not final schema.

## Files

```txt
~/.tero/
  config.yaml
  accounts.yaml
  agents.yaml
  projects.yaml
```

## `config.yaml`

```yaml
version: 1
language: en
visible_backend: orca # or herdr; ask if missing

confirmations:
  before_delegation: true
  before_merge: true

storage:
  tasks_dir: ~/.tero/tasks
  worktrees_dir: ~/.tero/worktrees
```

## `accounts.yaml`

Accounts represent Git/GitHub identities. They are generic; users can define as many as needed.

```yaml
accounts:
  github-primary:
    type: github
    username: primary-user
    git_emails:
      - primary@example.com

  github-secondary:
    type: github
    username: secondary-user
    git_emails:
      - secondary@example.com
```

Account names are user-defined labels. They may represent any boundary the user cares about: different GitHub users, organizations, clients, machines, or credential sets.

## `agents.yaml`

Agents represent configured harness/account combinations.

```yaml
agents:
  pi-primary:
    harness: pi
    account: github-primary
    command: pi

  codex-primary:
    harness: codex-cli
    account: github-primary
    command: codex

  hermes-primary:
    harness: hermes
    account: github-primary
    command: hermes

  claude-secondary:
    harness: claude-code
    account: github-secondary
    command: claude

  opencode-secondary:
    harness: opencode
    account: github-secondary
    command: opencode
```

Agent names are user-defined labels. The important part is the mapping between a harness command and the account it is allowed to use.

## `projects.yaml`

Project roots are manually registered in the MVP. Auto-scan can be added later.

```yaml
project_roots:
  - id: primary-projects
    path: ~/Projects/primary
    default_account: github-primary

  - id: secondary-projects
    path: ~/Projects/secondary
    default_account: github-secondary

projects:
  project-alpha:
    path: ~/Projects/primary/project-alpha
    account: github-primary
    allowed_agents:
      - pi-primary
      - codex-primary
      - hermes-primary
    default_agent: pi-primary
    tags:
      - app

  project-beta:
    path: ~/Projects/secondary/project-beta
    account: github-secondary
    allowed_agents:
      - claude-secondary
      - opencode-secondary
    default_agent: claude-secondary
    fallback_agents:
      - opencode-secondary
    tags:
      - service
```

Projects are explicit. A project chooses one account, a set of allowed agents, and one default agent. Tags are optional and should not carry security meaning.

## Onboarding flow

Initial command:

```bash
tero init
```

MVP flow:

1. Configure accounts.
2. Configure agents/harnesses.
3. Configure project roots.
4. Add projects manually.
5. Ask for visible backend only when first needed, unless configured during init.

## Task metadata example

```yaml
id: 2026-09-17-login-bug
status: created
created_at: 2026-09-17T16:48:35+02:00
project: project-alpha
account: github-primary
agent:
  selected: pi-primary
  attempts: []
visible_backend: orca
worktree: ~/.tero/worktrees/project-alpha/2026-09-17-login-bug
confirmation:
  delegated: true
  merge_approved: false
```

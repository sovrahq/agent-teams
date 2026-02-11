# Agent Teams

Reusable agent team definitions for coordinating Claude Code sessions across projects.

## Setup

Clone this repo alongside your projects:

```bash
git clone git@github.com:sovrahq/agent-teams.git
```

## Usage

Add this to your project's Claude Code memory (MEMORY.md or CLAUDE.md), adjusting the path to where you cloned the repo:

```
## Agent Team (swarm)
Cuando el usuario diga "team-lead <issues>", leé `<path-to>/agent-teams/agent-team.md` y ejecutá el flujo descrito ahí.
Flujo: team lead + coder + reviewer + senior-reviewer.
Se invoca con issue numbers como argumento (ej: `team-lead #96` o `team-lead #96 #49 --auto-merge`).
```

Then invoke from any Claude Code session:

```
team-lead #96
team-lead #96 #49 --auto-merge
```

## Files

| File | Description |
|------|-------------|
| `agent-team.md` | Team workflow: team lead + coder + reviewer + senior-reviewer. Coordinates issue resolution with review loops. |

## How it works

Claude reads `agent-team.md` and acts as team lead from the main session. It uses `TeamCreate`, `Task`, and `SendMessage` tools to spawn and coordinate teammates:

- **Team lead** (main session): git, gh, coordination
- **Coder** (spawned): edits files, runs tests
- **Reviewer** (spawned): functional review with `/pr-review`
- **Senior reviewer** (spawned): consistency review with `/pr-review`

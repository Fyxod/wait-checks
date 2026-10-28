# Wait Checks

> An active wait-and-check workflow for AI agents.

Long-running work needs follow-through: real waits, scheduled inspections, and progress updates grounded in what actually happened.

**Wait Checks** keeps an agent with the requested work until that scope is complete or the user stops it. It works across projects and chats that can load the skill and provide real wait tools.

## What it does

- **Actually waits** for the requested interval while keeping the turn active
- **Checks real progress** through available jobs, artifacts, queues, tests, or services
- **Reports completed and remaining work** without inventing unattended checks
- **Preserves the interval and deadline** when continuing after a question or interruption
- **Handles meaningful completion and error events** without busy polling progress
- **Keeps existing authorization boundaries** while work continues

> A background monitor alone does not fulfill an active wait-check request.

## Operating modes

| Mode | Purpose |
|---|---|
| **Start** | Recover the requested interval, watched scope, and stopping condition |
| **Continue** | Reuse the existing timer and live handle without duplicating work |
| **Steer** | Handle questions, changed intervals, or scope updates while preserving the task |
| **Stop** | End the wait loop when requested, when its scope is complete, or when a real blocker requires user input |

## Example requests

```text
Use $wait-checks. Check every 30 minutes until the build and report are done.

Continue 2 hour wait checks.

Change to 15 minute checks and keep the current jobs running.

Stop wait checks.
```

There is no fixed default interval. The skill uses the user's current request and existing conversation context.

## How it works

```text
Read the current timer and scope
              ↓
Actually wait in the active turn
              ↓
Inspect real state at the scheduled wake
              ↓
Report progress and handle authorized follow-up
              ↓
Repeat until the requested stopping condition
```

Use an attached terminal when available, or a real sleep/wait tool that retains the active turn. Tool yields can keep the conversation responsive without turning them into extra progress checks. Explicit completion or error events may wake the wait early.

If the environment cannot keep a live wait, the agent must say so. The skill does not create background notification capabilities or authorize extra actions.

## Use in Codex

For a fresh user-wide installation, authenticate with a GitHub account that can access this private repository, then clone it into the user skills directory:

```bash
mkdir -p "$HOME/.agents/skills"
gh repo clone Fyxod/wait-checks "$HOME/.agents/skills/wait-checks"
```

Invoke it as `$wait-checks`, or use a matching natural-language request when the skill is loaded.

For another agent or chat environment, load [SKILL.md](SKILL.md) using that environment's skill or instruction support. No Python package, project repository, memory system, or MCP dependency is required to use the instructions.

## Repository

```text
├── README.md
├── SKILL.md
└── agents/
    └── openai.yaml
```

[SKILL.md](SKILL.md) contains the reusable workflow. [agents/openai.yaml](agents/openai.yaml) supplies optional Codex UI metadata.

A useful companion is [Durable Agent Memory](https://github.com/Fyxod/durable-agent-memory) for preserving state across sessions and handoffs. Wait Checks can be used independently.

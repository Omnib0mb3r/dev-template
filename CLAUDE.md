# [Project Name] — Claude Instructions

> Human-readable workflow docs live in [`WORKFLOW.md`](WORKFLOW.md). The rules below are the machine-readable authority for Claude.

## Answering Questions
- When the user asks a question, ANSWER IT AND STOP. Do not make any changes, edits, commits, or other modifications to files or state. Do not barrel ahead into fixes, next steps, or unasked-for actions.
- A question is NOT authorization to act. Sometimes the user wants to talk something out first. Wait for an explicit "proceed", "go ahead", "do it", or equivalent before taking action.
- This applies even when the answer suggests an obvious next action, even when the fix looks trivial, and even when you are confident about what to do. Let the user choose whether and when to take it.

## Bug Tracker
- This project keeps a persistent bug log in [`BUGS.md`](BUGS.md). Read its Index block first before debugging, log every bug there when found, and flip its status (OPEN, SMOKE-TESTING, RESOLVED, DEFERRED) in both the index and detail block as it moves. Full rules: see "Bug Tracker (BUGS.md)" in the global Claude instructions.

## Brainstorming Sessions

At the start of every session, if `OTLC-Brainstorm.md` exists, read it before doing anything else.

Brainstorm work uses the **superpowers pipeline, augmented with OTLC layers**. Follow this exact order — do NOT jump straight into `superpowers:brainstorming` without the PRE steps, and do NOT advance to `writing-plans` without the POST steps.

### PRE — before invoking superpowers brainstorming

1. **Read `OTLC-Brainstorm.md`** in full — load Vision, Architecture, prior Decisions, and Open Questions into context.
2. **Vision check** — if `## Vision` is empty or the user is starting a new direction, ask the user to define/confirm the Vision before going further. The Vision is written once and rarely overwritten.
3. **Visual Companion offer** — if the topic involves layouts, mockups, designs, style choices, flow diagrams, or any visual comparison, offer to launch the Visual Companion before brainstorming begins.

### RUN — superpowers pipeline

1. **Brainstorm** — invoke `superpowers:brainstorming`. Produces a spec in `docs/superpowers/specs/`.
2. **Plan** — `superpowers:writing-plans` is auto-invoked at the end of brainstorm. Produces a plan in `docs/superpowers/plans/`.
3. **Execute** — `superpowers:subagent-driven-development` runs the plan task-by-task with spec + quality review after each task.
4. **Finish** — `superpowers:finishing-a-development-branch` is auto-invoked at the end of execution.

### POST — after the brainstorm spec is approved (BEFORE entering plan phase)

Update `OTLC-Brainstorm.md`:

- **Decisions Made** — append a new dated entry summarizing the choices and trade-offs. Check for conflicts with prior entries before adding.
- **Open Questions** — append any new ones. Mark resolved questions with `[x]` rather than deleting them.
- **Architecture** — update only if structural decisions changed.

### END — after the session ends

- **Session Log** — append a brief dated entry: participants, what happened, what was built, next steps.

### OTLC maintenance rules

- **Vision** — written once. Never overwrite unless explicitly asked.
- **Architecture** — update when structural decisions change.
- **Decisions Made** — append-only. Check for conflicts before adding.
- **Open Questions** — append new ones. Mark resolved with `[x]` — never delete.
- **Session Log** — append at the end of every session.

The goal: a fresh Claude reading `OTLC-Brainstorm.md` plus the project files should understand the true original intent — the why, not just the what.

## devneural.jsonc

- `devneural.jsonc` is the source of truth for project stage and metadata.
- When stage changes, update the file AND call the monday.com MCP `move_project` tool in the same step.
- Never change `localPath` or `githubUrl` manually — managed by the SessionStart hook.

## monday.com Sync Rules

When stage changes in devneural.jsonc:
1. Call `move_project` MCP tool with the new stage.
2. If this is a new project with no .Dev card yet, call `register_project` first.

Stage → .Dev board column:
- `alpha` → Alpha
- `beta` → Beta
- `deployed` → Released
- `archived` → Archived

Pre-Release is a manual promotion only — never move there automatically.

## Adding Tasks

When a bug, task, or idea is identified during a session, call `add_task` MCP tool:
- Default column: Backlog
- Use "To Do" for confirmed next actions
- Use "In Progress" only if actively being worked on this session

## Force Sync

If asked to "sync devneural" or "sync monday", call the DevNeural API:
`POST http://localhost:3747/sync`

This reconciles all devneural.jsonc files against the current monday.com board state.


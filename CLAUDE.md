# [Project Name] — Claude Instructions

## Brainstorming Sessions

At the start of every session, if `OTLC-Brainstorm.MD` exists, read it before doing anything else.

When brainstorming, follow this process:
1. Explore current project context first (files, recent commits, existing decisions in OTLC-Brainstorm.MD)
2. Ask clarifying questions one at a time
3. Propose 2-3 approaches with trade-offs before settling on one
4. Present the design section by section, get approval after each
5. Write a spec to `docs/specs/<topic>.md`
6. Self-review the spec: scan for TBDs, contradictions, ambiguity, scope creep — fix inline
7. Adversarial review: run Codex on the spec for an independent pass — incorporate any issues found
8. Update OTLC-Brainstorm.MD with new decisions and session log
9. Invoke `/writing-plans` to transition directly into the superpowers execution pipeline

Maintain `OTLC-Brainstorm.MD` throughout:
- **Vision** — written once in the first session. Never overwrite unless explicitly asked.
- **Architecture** — update when structural decisions change.
- **Decisions Made** — append after every session. Check for conflicts before adding.
- **Open Questions** — append new questions. Mark resolved with [x] — never delete.
- **Session Log** — append a brief summary at the end of every session.

The goal: a fresh Claude reading OTLC-Brainstorm.MD plus the project files should understand the true original intent — the why, not just the what.

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


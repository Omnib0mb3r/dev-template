# DevNeural Template — Workflow Guide

Starter template for all projects in the DevNeural ecosystem. Cloning this gives you the full session intelligence and brainstorming pipeline out of the box.

## Prerequisites

The **DevNeural API server** must be running for ecosystem context to load during sessions:

```bash
node /path/to/DevNeural/02-api-server/dist/server.js
```

Runs on port `3747`. If the server is offline, sessions still work — you just get no graph context from the hook. All DevNeural hooks are non-blocking and always exit cleanly, so they will never stall or interfere with a Claude session.

## What is included

| File | Purpose |
|------|---------|
| `devneural.jsonc` | Registers the project in the DevNeural graph. Most fields are auto-filled on first session start. |
| `OTLC-Brainstorm.md` | Living project memory — Vision, Architecture, Tools, Feature Workflow, Decisions, Open Questions, Session Log. |
| `BUGS.md` | Persistent bug tracker. Index block (id, status, summary) at the top, full detail entries below. Agents read the index first and check known OPEN bugs before debugging. |
| `CLAUDE.md` | Project-level Claude instructions: PRE/RUN/POST/END brainstorm flow, monday.com sync rules. |
| `README.md` | Placeholder stub — overwrite with the real project README. |
| `WORKFLOW.md` | This file — the human-readable template workflow guide. |

## First session workflow

1. Create a new repo from this template via GitHub's **"Use this template"** button
2. Clone the new repo locally
3. Start a Claude session — the SessionStart hook auto-fills `devneural.jsonc` with `localPath`, `githubUrl`, `name`, and `description`
4. Add your `tags` and `stage` to `devneural.jsonc`
5. Tell Claude what you want to build — Claude will run the augmented brainstorming flow (see below) which populates `OTLC-Brainstorm.md` and feeds straight into the superpowers execution pipeline
6. Ask Claude to write the real `README.md` when the project is ready (the template's `README.md` is a placeholder)

## Feature workflow

Every feature, bug, or improvement runs through the **superpowers pipeline wrapped with OTLC layers**. This is what Claude does on every session:

### PRE — context load
1. Read `OTLC-Brainstorm.md` (Vision, Architecture, prior Decisions, Open Questions)
2. **Vision check** — if blank, Claude asks you to define it before brainstorming starts
3. **Visual Companion offer** — for layout, mockup, design, or diagram work, Claude offers to launch the Visual Companion

### RUN — superpowers pipeline
1. **Brainstorm** — `superpowers:brainstorming` produces a spec in `docs/superpowers/specs/`
2. **Plan** — `superpowers:writing-plans` (auto-invoked) produces a plan in `docs/superpowers/plans/`
3. **Execute** — `superpowers:subagent-driven-development` runs the plan task-by-task with review
4. **Finish** — `superpowers:finishing-a-development-branch` (auto-invoked) merges, opens a PR, or discards

### POST — log decisions (before plan phase)
- Append to `Decisions Made` in `OTLC-Brainstorm.md`
- Append/resolve `Open Questions`
- Update `Architecture` if structure changed

### END — close the session
- Append a dated `Session Log` entry: participants, what happened, what was built, next steps

The full rules live in [`CLAUDE.md`](CLAUDE.md) and [`OTLC-Brainstorm.md`](OTLC-Brainstorm.md).

## devneural.jsonc

```jsonc
{
  "name": "REPLACE_ME",        // auto-filled from folder name
  "localPath": "REPLACE_ME",   // auto-filled from cwd
  "githubUrl": "REPLACE_ME",   // auto-filled from git remote origin
  "stage": "alpha",            // alpha | beta | deployed | archived
  "tags": [],                  // YOU fill these in — drives ecosystem connections
  "description": "REPLACE_ME" // auto-filled from README first line
}
```

Tags are the primary driver of cross-project connections in the DevNeural graph. Use them to describe the domain, tooling, and patterns in the project (e.g. `"autolisp"`, `"api"`, `"claude"`, `"conveyor"`).

## Stage sync

When `stage` changes, Claude will call the monday.com MCP `move_project` tool automatically. For new projects with no board card yet, `register_project` is called first.

To manually reconcile all projects against the monday.com board:

```
sync devneural
```

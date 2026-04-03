# dev-template

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
| `OTLC-Brainstorm.MD` | Living project memory — Vision, Architecture, Decisions, Open Questions, Session Log. |
| `CLAUDE.md` | Project-level Claude instructions: brainstorm workflow, monday.com sync rules. |

## First session workflow

1. Clone the template into a new folder
2. Start a Claude session — the SessionStart hook auto-fills `devneural.jsonc` with `localPath`, `githubUrl`, `name`, and `description`
3. Add your `tags` and `stage` to `devneural.jsonc`
4. Run the brainstorming workflow to populate `OTLC-Brainstorm.MD`
5. At the end of brainstorming, invoke `/writing-plans` — this transitions directly into the superpowers execution pipeline (plan → subagent execution → finish)

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


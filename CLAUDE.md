# [Project Name] — Claude Instructions

> Human-readable workflow docs live in [`WORKFLOW.md`](WORKFLOW.md). The rules below are the machine-readable authority for Claude.

## Answering Questions
- When the user asks a question, ANSWER IT AND STOP. Do not make any changes, edits, commits, or other modifications to files or state. Do not barrel ahead into fixes, next steps, or unasked-for actions.
- A question is NOT authorization to act. Sometimes the user wants to talk something out first. Wait for an explicit "proceed", "go ahead", "do it", or equivalent before taking action.
- This applies even when the answer suggests an obvious next action, even when the fix looks trivial, and even when you are confident about what to do. Let the user choose whether and when to take it.

## Bug Tracker
- This project keeps a persistent bug log in [`BUGS.md`](BUGS.md). Read its Index block first before debugging, log every bug there when found, and flip its status in both the index and detail block as it moves: OPEN (nobody has started it), IN-PROGRESS the moment work starts (never start over on one already in progress), SMOKE-TESTING when the fix lands, then RESOLVED, DEFERRED (on hold) or back to IN-PROGRESS. Full rules: see "Bug Tracker (BUGS.md)" in the global Claude instructions.

## Brainstorming Sessions

At the start of every session, if `OTLC-Brainstorm.md` exists, read it before doing anything else.

Brainstorm work uses the **superpowers pipeline, augmented with OTLC layers**. Follow this exact order — do NOT jump straight into `superpowers:brainstorming` without the PRE steps, and do NOT advance to `writing-plans` without the POST steps.

### PRE — before invoking superpowers brainstorming

1. **Read `OTLC-Brainstorm.md`** in full — load Vision, Architecture, prior Decisions, and Open Questions into context.
2. **Vision check**: if `## Vision` is empty, or the user is starting a new direction or describes the project more broadly than the Vision does, offer to define or update the Vision before going further.
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

- **Vision**: a living statement. Update it whenever the user's picture of the project grows, and add a dated history line saying what changed so the original intent stays readable.
- **Architecture** — update when structural decisions change.
- **Decisions Made** — append-only. Check for conflicts before adding.
- **Open Questions** — append new ones. Mark resolved with `[x]` — never delete.
- **Session Log** — append at the end of every session.

The goal: a fresh Claude reading `OTLC-Brainstorm.md` plus the project files should understand the true original intent — the why, not just the what.

## devneural.jsonc

- `devneural.jsonc` is the source of truth for project stage and metadata.
- Never change `localPath` or `githubUrl` manually — managed by the SessionStart hook.


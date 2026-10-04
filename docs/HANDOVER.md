# Handover

> HOW TO USE THIS FILE
> 1. A fresh session with no memory of the last one reads this file first and must be able to act on it alone.
> 2. Keep the ten headings below, in this order. They match the handover DevNeural asks a worker for when its context fills, so the two never disagree.
> 3. Rewrite it at every stopping point. Replace stale content; do not append a history. Session history belongs in git and OTLC-Brainstorm.md.
> 4. Name files, shas, bug ids and doc paths in full. Nothing implied.
> 5. Instructions the owner gives directly are part of the current task. Carry each one forward here in his terms.

## Current task

What the task is, the goal it serves, and the step you are at. Three short lines.

## Verified state

Checked at the time of writing, one line each:
- Branch:
- HEAD (short sha):
- Pushed: yes, or no with the ahead count
- Tree: clean, or dirty with the dirty files
- Deployed version: the version, or none

## Done this session

One bullet per commit: short sha and subject, then the BUG ids it touched. None if nothing was committed.

## Bugs

One bullet per OPEN or SMOKE-TESTING bug touched or blocking the task: BUG id, status as the [BUGS.md](../BUGS.md) index has it, one line. The BUGS.md index is the source of truth; when this list and the index disagree, fix one of them. None if there are none.

## Smoke tests

One bullet per test: what it checks, pending or passed or failed, and who must run it. Point at SMOKE-TEST.md if the project has one. None if there are none.

## Todos and waiting-on

One bullet per parked item or open owner decision, with who it waits on. None if there are none.

## Next

The next one to three concrete actions, numbered, in the order to do them.

1.

## Decisions in force

One bullet per constraint or decision that carries forward. None if none.

## Stopping point

How the last session stopped safely (committed, servers stopped), and anything still running such as servers, watchers or background jobs. None if nothing is running.

## Read first

Doc paths a fresh session must read before acting, most important first.

- docs/HANDOVER.md (this file)
- BUGS.md (index block only)
- OTLC-Brainstorm.md

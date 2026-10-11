# AI development protocol

This file defines agent behaviour. It is not the architectural or delivery
source of truth; use the documents below for those facts.

## Knowledge index

| Need | Authoritative file |
| --- | --- |
| Architecture and active technical boundaries | [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) |
| Accepted, proposed, and retired decisions | [docs/DECISIONS.md](docs/DECISIONS.md) |
| Phase, backlog, acceptance criteria, and progress | [docs/CURRENT_PLAN.md](docs/CURRENT_PLAN.md) |
| Exact resumable checkout state | [docs/HANDOFF.md](docs/HANDOFF.md) |
| User-facing use and artifact contract | [README.md](README.md) |
| Build inputs | [builder/build.env](builder/build.env) |

## Session start protocol

For a new chat, after context compression, or on another computer:

1. Read applicable `AGENTS.md` files from repository root to the target path.
2. Read `docs/ARCHITECTURE.md`, `docs/CURRENT_PLAN.md`, and
   `docs/HANDOFF.md`; read the relevant entries in `docs/DECISIONS.md` for the
   task.
3. Inspect `git branch --show-current`, `git rev-parse HEAD`, `git status
   --short --branch`, `git diff`, and recent commits.
4. Check the relevant source and configuration before relying on a document.
   Repository facts win over stale documentation; investigate and record a
   material mismatch before continuing.
5. If the state is consistent and the task is already accepted, continue from
   `docs/HANDOFF.md`'s **Next Action** without asking the user to repeat
   context.

## Default execution

For an accepted phase, work autonomously in this order: inspect state,
implement, add or adjust proportionate verification, run verification, inspect
the diff, update plan/handoff when progress materially changed, then report.

Pause for user direction only when a business rule is genuinely ambiguous, an
accepted decision or architecture boundary must change, materially different
implementations have important consequences, a destructive/irreversible action
is required, or a critical environment dependency prevents safe progress.
Unaccepted requirements need a proposed design and explicit confirmation first.

## Change and verification rules

- Preserve unrelated dirty changes. Do not reset, clean, force-push, overwrite,
  or discard work without explicit authorization.
- Keep build inputs centralized in `builder/build.env`; do not alter OpenWrt
  source or silently weaken package-signature verification.
- Run the smallest relevant checks first and report checks that were not run.
- Do not represent a planned, simulated, or unrun check as passed.
- Update `docs/CURRENT_PLAN.md` only for a real progress or scope change.
  Update `docs/HANDOFF.md` after substantive work, before a requested handoff,
  or when the documented recovery point changes; do not churn it for ordinary
  discussion.

## Git and multi-computer handoff

Git is the only reliable synchronization mechanism. Standard handoff is:
develop -> verify -> update handoff -> commit -> push; then on the other
computer fetch/pull -> inspect status -> run Session Start -> continue Next
Action. A dirty checkout is local only, even if `HANDOFF.md` describes it.
When handoff and Git disagree, stop and reconcile the evidence instead of
assuming either is current.

For this repository, the user authorizes one normal commit and push to `dev`
after each completed, verified task slice and its handoff update. Do not commit
or push partial/unverified work merely to synchronize it. Never push directly
to `master`; changes reach it only through the user-directed integration flow.

Use GitHub SSH over port 443 for this repository's Git transport:
`ssh://git@ssh.github.com:443/miozen/OpenwrtBuilder.git`. Before a handoff
push, confirm `origin` still uses this URL; do not silently fall back to HTTPS.

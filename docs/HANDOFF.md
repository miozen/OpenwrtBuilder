# Repository handoff

This is a snapshot, not a substitute for Git. Read it with `git status`, diff,
HEAD, and the source named below.

## Snapshot

- Recorded at: 2026-10-11 (Asia/Shanghai), after moving shared documents into
  the tracked `docs/` directory and switching local development to `dev`.
- Branch: `dev`, tracking `origin/dev`. The pre-documentation base was
  `7cf970cc95fe4e5a65496a452bfa8054e55dab80`; use Git to identify the resulting
  documentation-checkpoint commit.
- Baseline worktree before this documentation work: CLEAN.
- Current local worktree expected by this handoff after the documentation
  checkpoint is committed and pushed: CLEAN.
- Remote synchronization: the user authorized this completed documentation
  slice to be pushed to `origin/dev`. The prior HTTPS push failed before
  authentication because this environment has no usable GitHub username or
  credential. Per user direction, `origin` must instead use GitHub SSH over
  port 443: `ssh://git@ssh.github.com:443/miozen/OpenwrtBuilder.git`. That
  transport was validated and the documentation checkpoints through
  `8306e45` were pushed successfully to `origin/dev`.

## Current phase and task

The repository knowledge and handoff foundation is complete and ready for its
documentation checkpoint. No firmware/business-code task is active.
`docs/CURRENT_PLAN.md` is the source of truth for planned delivery validation.

## Recently completed

- Read-only architecture audit completed: project is a GitHub Actions + official
  ImageBuilder firmware pipeline; no frontend, backend, or database exists.
- Shell syntax was checked for `builder/build.sh`, `scripts/convert-vmdk.sh`,
  and `files/etc/uci-defaults/99-custom-lan-ip`.
- APK public-key PEM validity and SHA-256 pin were checked; old unsigned-APK
  override is absent from the overlay; `git diff --check` and `git fsck` passed
  before this documentation change.
- This session established the five document roles and protocols.
- GitHub SSH over port 443 was configured and verified; the shared-document
  checkpoint is now available on `origin/dev`.

## Verification status

| Check | Status |
| --- | --- |
| Shell syntax / key integrity / repository object integrity | Passed before the documentation edits; final `git diff --check` also passed after migration. |
| Documentation link/path validation and Session Start replay | Passed after moving to `docs/` and switching to `dev`; paths resolve and the actual checkout matches this snapshot. |
| Current signed-repository ImageBuilder build | Not run in this session. |
| Current firmware EFI boot and sysupgrade acceptance | Not run / not evidenced in the current checkout. |

## Known conditions

- `docs/` is now intended to be tracked. Historical documentation that was
  previously removed remains only in Git history and is not a current source;
  do not rely on it without reconciling policy changes.
- `dev` is the active local development branch by user direction. Local refs
  show `master` six commits ahead of `origin/dev`; merge or reconcile only with
  explicit user direction.
- CI artifacts expire after 14 days and package manifests are temporary build
  files; durable provenance is a proposed decision, not an implemented feature.

## Exact next action

Do not start firmware/business development. Wait for the user to select the
next phase described in `CURRENT_PLAN.md`.

## Cross-computer readiness

**Ready.** A second computer can fetch/pull `dev`, confirm a clean checkout,
and run Session Start before continuing.

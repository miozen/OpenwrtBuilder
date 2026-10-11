# Current plan

## Phase

**Repository knowledge and handoff foundation — completed in the working tree,
awaiting user-directed review/commit.** This phase adds durable project context
and does not alter firmware behaviour.

## Completed

- The ImageBuilder pipeline, signed custom repository policy, package manifest
  gate, LAN default overlay, and delivery conversion pipeline are implemented.
- The current checkout has passed repository static checks recorded in
  `HANDOFF.md`; no current-configuration ImageBuilder build or VM/sysupgrade
  acceptance has been run during this documentation phase.
- The five knowledge roles and Session Start/Handoff protocols are now defined
  by `AGENTS.md`, `ARCHITECTURE.md`, `DECISIONS.md`, this file, and `HANDOFF.md`.

## Remaining delivery validation

Before claiming the present signed-repository configuration is release-validated:

1. Run a `master` GitHub Actions build and retain its workflow/artifact evidence.
2. Verify the resulting firmware in an EFI VM: boot, 500 MiB root partition,
   LAN default, LuCI, and package set.
3. Perform a configuration-preserving IMG.GZ sysupgrade between project builds
   and verify the three declared keep-rule paths.

## Acceptance criteria for the next validation phase

- The workflow succeeds without bypassing checksum, manifest, or APK-signature
  gates.
- Artifact contains IMG.GZ, raw IMG, VMDK, and a checksum file covering all
  three images.
- VM and sysupgrade observations are recorded with actual version and artifact
  hashes; unrun checks remain explicitly unrun.

## Next action

**Wait for the user to name the next phase.** If the user authorizes release
validation, start with a read-only check of `master`/remote state, then trigger
or observe the configured GitHub Actions build as directed. Do not begin
firmware validation or business changes merely because this plan exists.

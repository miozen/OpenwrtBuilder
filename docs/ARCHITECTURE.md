# Architecture

## Scope

This repository is a configuration-driven OpenWrt firmware build and delivery
system. It is not an application service: it has no frontend, backend, database,
API server, or database migration layer. The running firmware uses OpenWrt's
own UCI/APK/sysupgrade mechanisms.

## Components and boundaries

| Component | Responsibility | Boundary |
| --- | --- | --- |
| `builder/build.env` | Single source for variable build inputs: release series, target/profile, image shape, package additions, APK trust anchor | POSIX-shell data only; changing it changes firmware policy |
| `builder/build.sh` | Resolve release; fetch and verify ImageBuilder/manifest; plan and validate package set; invoke ImageBuilder | Does not compile or modify OpenWrt source |
| `files/` | ImageBuilder `FILES` overlay: custom repository/key, first-boot LAN UCI default, sysupgrade keep rules | Must not replace official sysupgrade configuration or LuCI package manager |
| `scripts/convert-vmdk.sh` | Turn the generated IMG.GZ into raw IMG and VMDK; validate formats and produce checksums | VMDK is a first-deployment artifact, not a sysupgrade image |
| `.github/workflows/build-openwrt.yml` | Run the one build entry point on GitHub Actions and upload artifacts | Workflow owns orchestration only; build logic belongs in `builder/build.sh` |

## Build data flow

`build.env` -> release directory -> official `sha256sums`, manifest and
ImageBuilder -> planned package manifest -> ImageBuilder image with `files/`
overlay -> built manifest comparison -> IMG.GZ -> raw IMG/VMDK/checksum set ->
GitHub Actions artifact.

The package-set invariant is intentionally strict: no official manifest package
may be absent from the planned set, required additions must appear, and the
built manifest must equal the planned manifest. See `builder/build.sh`.

## Active constraints

- Use official released ImageBuilder for `x86/64`, `generic`, ext4 combined EFI;
  root filesystem is 500 MiB. Inputs live only in `builder/build.env`.
- Select the latest stable patch release in the configured series; RC and
  snapshot releases are excluded.
- Retain standard APK signature verification. The signed custom repository's
  public key is part of the firmware overlay and its SHA-256 is pinned in
  `builder/build.env`.
- Preserve `/etc/shinra/`, the custom repository entry, and its public key on
  sysupgrade. Third-party binaries themselves are not retained.
- Firmware builds only on `master`; `dev` is a development branch. CI uploads
  artifacts for 14 days and does not create releases.

For rationale and superseded choices, use `DECISIONS.md`, not this fact sheet.

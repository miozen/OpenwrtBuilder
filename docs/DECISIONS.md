# Decision register

Statuses: **Accepted** are active constraints; **Superseded** records a
previous decision that must not be revived accidentally; **Proposed** requires
user confirmation before implementation. Commit references are evidence, not a
replacement for current source.

| ID | Status | Decision | Evidence / consequence |
| --- | --- | --- | --- |
| D-001 | Accepted | Build via official OpenWrt ImageBuilder only; do not modify or rebuild OpenWrt source. | `1eb30c2`; current `builder/build.sh` |
| D-002 | Accepted | Build the latest stable `25.12.x` x86/64 generic ext4 combined-EFI image with a 500 MiB root filesystem. | `builder/build.env` |
| D-003 | Accepted | Use the same raw IMG to create VMDK; VMDK is only for first VM deployment, while IMG.GZ is used for sysupgrade. | `README.md`, `scripts/convert-vmdk.sh` |
| D-004 | Accepted | Preserve Shinra configuration and custom APK trust configuration via OpenWrt keep rules; do not replace official `/etc/sysupgrade.conf`. | `files/lib/upgrade/keep.d/openwrt-builder`, `builder/build.sh` |
| D-005 | Superseded | Permit LuCI installation of unsigned local APKs by overriding `package-manager-call`. | Replaced by D-006 in `c53dc89`; the current build rejects that override. |
| D-006 | Accepted | Use a signed custom APK repository with a pinned public-key hash and retain normal APK signature verification. | `c53dc89`, current overlay and `builder/build.sh` |
| D-007 | Accepted | Preserve official package-manifest baseline; add SFTP and the specified LuCI Chinese translations, then require planned and built package sets to match. | `600703e`, `7cf970c`, current build configuration |
| D-008 | Accepted | `master` is the firmware-build branch; `dev` pushes do not build; delivery uses GitHub Actions artifacts rather than GitHub Releases. | `.github/workflows/build-openwrt.yml`, `README.md` |
| D-009 | Proposed | Add durable build provenance (resolved version, package manifests, and verification record) to deliverables or a release record. | Identified during the 2026-10-11 architecture audit; no implementation is authorized. |

New accepted decisions need an ID, status, concise rationale/evidence, and any
affected `CURRENT_PLAN.md` or `HANDOFF.md` update. Do not rewrite
accepted history to conceal a superseded decision.

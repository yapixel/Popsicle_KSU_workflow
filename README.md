# Popsicle KSU Workflow

Automated GitHub Actions pipeline for building GKI kernels targeting the **Xiaomi Popsicle (SM8850 / Snapdragon 8 Elite)** on Android 16.

---

## Overview

This repository provides a modular, dispatcher-based workflow that builds multiple KernelSU variants from a single trigger. Each variant is fully isolated in its own workflow file, making it easy to debug, maintain, and extend independently.

---

## Device

| Property | Value |
|----------|-------|
| Device | Xiaomi Popsicle |
| SoC | Snapdragon 8 Elite (SM8850) |
| Android | 16 |
| Kernel | GKI 6.12.x |
| Architecture | arm64 |
| Toolchain | LLVM Clang 19 (r536225) |

---

## Variants

| Variant | KernelSU Source | SuSFS |
|---------|----------------|-------|
| `kowsu+susfs` | [KOWX712/KernelSU](https://github.com/KOWX712/KernelSU) | [simonpunk/susfs4ksu · gki-android16-6.12](https://github.com/simonpunk/susfs4ksu/tree/gki-android16-6.12) |
| `kowsu` | [KOWX712/KernelSU](https://github.com/KOWX712/KernelSU) | N/A |
| `xxksu` | [backslashxx/KernelSU](https://github.com/backslashxx/KernelSU) | N/A |

---

## Features

- **KernelSU** root solution (variant-dependent)
- **SuSFS** filesystem-level su hiding (KowSU + SuSFS variant only)
- **BBR** congestion control
- **IPSet** suite
- **KMI bypass** for extended module compatibility
- **Identity spoofing** — kernel version string matches stock OEM identity
- **AnyKernel3** flashable `.zip` output

---

## Workflow Structure
.github/workflows/
main.yml                  ← Dispatcher: triggers builds & release
build-kowsu-susfs.yml     ← KowSU + SuSFS build
build-kowsu.yml           ← KowSU (vanilla) build
build-xxksu.yml           ← xxKSU build
release.yml               ← Publishes GitHub Release

### How it works

1. `main.yml` is triggered manually via `workflow_dispatch`
2. A `setup` job resolves which variants to build based on the `ksu_variant` input
3. Each selected variant runs its dedicated build workflow in parallel
4. On success, `release.yml` collects all artifacts and publishes a GitHub Release

---

## Usage

Go to **Actions → Build GKI · Popsicle (SM8850) → Run workflow** and fill in:

| Input | Description | Default |
|-------|-------------|---------|
| `ksu_variant` | Variant(s) to build: `all`, `kowsu+susfs`, `kowsu`, `xxksu` | `all` |
| `sub_level` | Kernel sublevel, e.g. `23` for `6.12.23` | `23` |
| `create_release` | Whether to publish a GitHub Release | `true` |

---

## Output

Each build produces an AnyKernel3 flashable zip named:
kowsu+susfs_popsicle_6.12.<sub_level>-<date>-susfs-<susfs_version>.zip
kowsu_popsicle_6.12.<sub_level>-<date>.zip
xxksu_popsicle_6.12.<sub_level>-<date>.zip

---

## Installation

1. Boot into a custom recovery (e.g. TWRP) or use a root-capable flasher
2. Flash the `.zip` matching your desired variant
3. Reboot and verify root via the KernelSU app

> **Back up your current boot image before flashing.**
> This kernel is provided as-is, without warranty of any kind.

---

## References

- [KOWX712/KernelSU](https://github.com/KOWX712/KernelSU)
- [backslashxx/KernelSU](https://github.com/backslashxx/KernelSU)
- [simonpunk/susfs4ksu](https://github.com/simonpunk/susfs4ksu)
- [osm0sis/AnyKernel3](https://github.com/osm0sis/AnyKernel3)
- [cctv18/android_gki_kernel_common](https://github.com/cctv18/android_gki_kernel_common)
- [cctv18/oneplus_sm8650_toolchain](https://github.com/cctv18/oneplus_sm8650_toolchain)

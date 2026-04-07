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

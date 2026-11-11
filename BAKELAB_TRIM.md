# BakeLab SGLang trim scope

The `trim` branch should simplify SGLang around BakeLab's actual build and
runtime targets while retaining Linux ARM and macOS support. Its scope differs
from BakeLab's vLLM trim branch: platform support must not be removed just
because the current accelerator wheel manifests cover Linux x86_64 only.

## Platform and accelerator targets

| Platform | In-scope target |
| --- | --- |
| Linux x86_64 | NVIDIA CUDA 13.4 / SM120; AMD ROCm 10.1 / gfx1201; Intel XPU BMG / oneAPI 2026.2 |
| Linux aarch64 | Keep platform support; document and validate accelerator combinations separately |
| macOS arm64 | Keep Apple Silicon support and its platform-specific dependencies |

The Linux x86_64 toolchain builds use Python 3.14 and BakeLab's trimmed
PyTorch builds (2.16 for CUDA and ROCm, 2.15 for XPU). Do not impose these
versions on Linux ARM or macOS until their build and runtime toolchains are
defined.

## Cleanup rules

- Preserve Darwin and ARM dependency markers where they enable in-scope builds.
- Remove a backend only after confirming it is outside all three platform
  targets and is not shared by CUDA, ROCm, XPU, Linux ARM, or macOS.
- Keep backend-specific dependencies and CI focused on combinations BakeLab
  builds and validates. Add ARM and macOS jobs before pruning their paths.
- Treat each platform, dependency, and build-system change as a separate
  reviewable step.

# BakeLab SGLang trim scope

The `trim` branch should simplify SGLang around BakeLab's actual build and
runtime targets while retaining Linux ARM and macOS support. Its scope differs
from BakeLab's vLLM trim branch: platform support must not be removed just
because the current accelerator wheel manifests cover Linux x86_64 only.

## Platform and accelerator targets

| Platform | In-scope target |
| --- | --- |
| Linux x86_64 | NVIDIA CUDA 13.4 / SM120; AMD ROCm 10.1 / gfx1201; Intel XPU BMG / oneAPI 2026.2 |
| Linux aarch64 | CUDA 13.4 multi-arch images; CPU package and test workflow |
| macOS arm64 | Apple Silicon MLX/MPS path (`srt_mps`) |

The Linux x86_64 toolchain builds use Python 3.14 and BakeLab's trimmed
PyTorch builds (2.16 for CUDA and ROCm, 2.15 for XPU). The current Apple
Silicon MLX CI uses Python 3.11; Linux ARM has separate CPU and CUDA build
paths. Keep those platform-specific baselines independent of the x86_64 GPU
baseline.

## Cleanup rules

- Preserve Darwin and ARM dependency markers where they enable in-scope builds.
- Remove a backend only after confirming it is outside all three platform
  targets and is not shared by CUDA, ROCm, XPU, Linux ARM, or macOS.
- Keep backend-specific dependencies and CI focused on combinations BakeLab
  builds and validates. Preserve the existing ARM CPU/CUDA and macOS MLX/MPS
  workflows when simplifying shared package metadata.
- Treat each platform, dependency, and build-system change as a separate
  reviewable step.

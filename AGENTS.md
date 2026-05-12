# Flash-Attention 3 Wheels

Pre-built wheels for [Flash Attention 3](https://github.com/Dao-AILab/flash-attention/tree/main/hopper) — Linux/Windows/Arm(CUDA SBSA).

- **Repo**: windreamer/flash-attention3-wheels (main)
- **License**: Apache-2.0
- **Languages**: Python, Shell, PowerShell, Jinja
- **Pages**: https://windreamer.github.io/flash-attention3-wheels/

## Structure

```
scripts/
  build_wheel.sh        # Linux build (args: CUDA_VER TORCH_VER MAX_JOBS)
  build_wheel.ps1       # Windows build
  generate_matrix.py    # CI matrix generator (queries Docker Hub + PyTorch index)
  generate_pages.py     # GitHub Pages index generator (Jinja2)
  verify_wheel_exports.py
  *.patch               # Build patches (cuda_h_alignment, cutlass_alignment, windows_fix)
templates/              # Jinja2 HTML templates for index pages
.github/workflows/
  build_wheels.yml      # Main CI: biweekly + manual dispatch
  build_wheels_test.yml # Test workflow
  update-pages.yml      # Pages deployment
```

## Build & Release

- **CI**: GitHub Actions, biweekly (every 2nd/4th Sun 22:00 UTC) + `workflow_dispatch`
- **Matrix**: CUDA {12.8,13.0} × PyTorch {2.8.0,2.11.0} × {linux, windows, arm}
- **Process**: Clone upstream flash-attention → build wheel → rename with date/cuda/torch/abi hash → upload as GitHub Release
- **Wheel naming**: `flash_attn_3-{ver}+{date}+cu{cuda}torch{torch}cxx11abi{abi}+{hash}-{platform}.whl`
- **Patches applied**: cuda_h_alignment_fix, cutlass_alignment_fix, windows_fix (conditional)

## Key Scripts

- `scripts/build_wheel.sh` — sets up CUDA env, installs torch, clones upstream, builds, renames wheel with `change-wheel-version`
- `scripts/generate_matrix.py` — env `MATRIX_TARGET` (linux/windows/arm), `CUDA_VERSIONS`, `TORCH_VERSIONS`; outputs GitHub Actions matrix JSON
- `scripts/generate_pages.py` — fetches releases via GitHub API, generates pip `--find-links` index pages

## Install

```bash
pip install flash_attn_3 --find-links https://windreamer.github.io/flash-attention3-wheels/cu128_torch280
```
# Implementation Plan: Docker Image Optimization

## Objective
Reduce the total footprint of the `test-code-agents:devcontainer` image while preserving the ability to quickly install binary R packages from P3M.

## Background & Motivation
The current image has several layers exceeding 1GB:
- **System Dependencies (Step 2):** ~1.1GB (Essential for R geospatial binaries).
- **Rust Toolchain (Step 4):** ~1.4GB (Currently using the 'default' profile).
- **User Tools (Step 6):** ~3.3GB total bloat from NVM, Conda, and NPM caches.

## Proposed Solutions
- **Prune Apt:** Remove `tesseract-ocr-eng` and `poppler-data` to save ~200MB.
- **Optimize Rust:** Use `--profile minimal` for `rustup` (saves ~1GB).
- **Optimize Conda:** Run `conda clean -afy` (saves ~500MB).
- **Optimize NVM/NPM:** Clear caches and remove unnecessary source files (saves ~1GB).

## Implementation Steps
1. [x] Modify `.devcontainer/Dockerfile`.
2. [ ] Perform local test build (In progress).
3. [ ] Verify directory sizes.

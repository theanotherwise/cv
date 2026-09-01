# CV Project Instructions

## Purpose

This repository stores a version-controlled CV source and the automation required to render it as a PDF. Keep the source human-editable, the build reproducible, and generated files outside Git history.

## Structure

- [`CV.yaml`](./CV.yaml) is the stable RenderCV input file. Replace its temporary content when the real CV data is available, but keep the filename stable unless all build references are updated.
- [`.github/workflows/build-cv.yaml`](./.github/workflows/build-cv.yaml) contains the complete build, verification, and artifact-upload process. It runs on trusted pushes to `main` and through manual dispatch.
- [`output/pdf`](./output/pdf) is the ignored generated-output location.

## Execution Model

GitHub Actions installs `uv` and Python 3.13, then uses `uvx` with the exact `rendercv[full]==2.8` package version. It renders only `output/pdf/cv.pdf`, verifies that the file is non-empty, and uploads it as the `cv-pdf` workflow artifact from a GitHub-hosted Ubuntu runner with read-only repository contents access.

## Constraints

Keep [`README.md`](./README.md) limited to its project heading and maintain operational documentation here. Do not add local build scripts, commit generated PDF files, add a `pull_request` or `pull_request_target` trigger, introduce secrets, publish releases, or push changes unless the user explicitly requests it. Keep external GitHub Actions pinned to full commit SHAs and update the accompanying version comments when their pins change.

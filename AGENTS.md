# CV Project Instructions

## Purpose

This repository stores a version-controlled CV source and the automation required to render it as a PDF. Keep the source human-editable, the build reproducible, and generated files outside Git history.

## Structure

- [`CV.yaml`](./CV.yaml) is the stable RenderCV input file. Replace its temporary content when the real CV data is available, but keep the filename stable unless all build references are updated.
- [`.github/workflows/build-cv.yaml`](./.github/workflows/build-cv.yaml) contains the complete tag validation, build, verification, and artifact-upload process. It runs only when a matching CV tag is pushed.
- [`output/pdf`](./output/pdf) is the ignored generated-output location.

## Execution Model

The pushed tag must use `cv-YYMMDDHHMMSS-abcdef0`, where the 12 digits encode the local tag-creation timestamp and the final seven lowercase hexadecimal characters equal the short SHA of the commit referenced by the tag. GitHub Actions checks out that tag, validates its timestamp and commit suffix, installs `uv` and Python 3.13, then uses the exact `rendercv[full]==2.8` package version to render `output/pdf/cv.pdf` and upload it as the `cv-pdf` workflow artifact. The required intermediate Typst source is kept under the ignored `output/pdf` directory and is not uploaded.

To build the current commit, run `git-release-cv`; it creates and pushes `cv-$(date +%y%m%d%H%M%S)-$(git rev-parse --short=7 HEAD)`. Pushing that tag is the only supported build trigger.

## Constraints

Keep [`README.md`](./README.md) limited to its project heading and maintain operational documentation here. Do not add local build scripts, commit generated PDF files, add branch, pull-request, manual, or release workflow triggers, introduce secrets, publish releases, or push changes unless the user explicitly requests it. Keep external GitHub Actions pinned to full commit SHAs and update the accompanying version comments when their pins change.

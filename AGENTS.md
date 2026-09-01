# CV Project Instructions

## Purpose

This repository stores a version-controlled CV source and the automation required to render it as a PDF. Keep the source human-editable, the build reproducible, and generated files outside Git history.

## Structure

- [`CV.yaml`](./CV.yaml) is the stable, concise RenderCV source for the real curriculum vitae. Keep each experience entry focused on measurable scope and outcomes and limited to roughly five rendered lines on A4.
- [`.github/workflows/build-cv.yaml`](./.github/workflows/build-cv.yaml) contains the `Verify`, `Build`, and `Push` jobs. It runs only when a matching CV tag is pushed; `Build` depends on `Verify`, and `Push` publishes the generated artifact only after `Build` succeeds.
- [`output/pdf`](./output/pdf) is the ignored generated-output location.

## Execution Model

The pushed tag must use `cv-YYMMDDHHMMSS-abcdef0`, where the 12 digits encode the local tag-creation timestamp and the final seven lowercase hexadecimal characters equal the short SHA of the commit referenced by the tag. The `Verify` job checks the tag timestamp and commit suffix. After it succeeds, `Build` installs `uv` and Python 3.14, then uses the exact `rendercv[full]==2.8` package version to render `output/pdf/cv.pdf` and upload it as the `cv-pdf` workflow artifact. The required intermediate Typst source is kept under the ignored `output/pdf` directory and is not uploaded. `Push` downloads `cv-pdf` and posts it to `https://theanotherwise.com/api/v1/cv` with the repository secret `THEANOTHERWISE_COM_API_KEY`, full commit SHA, tag, and workflow-run source URL.

To build the current commit, run `git-release-cv`; it creates and pushes `cv-$(date +%y%m%d%H%M%S)-$(git rev-parse --short=7 HEAD)`. Pushing that tag is the only supported build trigger.

## Constraints

Keep [`README.md`](./README.md) limited to its project heading and maintain operational documentation here. Do not add local build scripts, commit generated PDF files, add branch, pull-request, manual, or release workflow triggers, introduce secrets, publish releases, or push changes unless the user explicitly requests it. Keep external GitHub Actions pinned to full commit SHAs and update the accompanying version comments when their pins change.

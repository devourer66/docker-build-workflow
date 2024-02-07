# Template: Docker Build GitHub Actions Workflow

[![Lint Code Base](https://github.com/devourer66/docker-build-workflow/actions/workflows/call-super-linter.yaml/badge.svg)](https://github.com/devourer66/docker-build-workflow/actions/workflows/call-super-linter.yaml)
[![Docker Build](https://github.com/devourer66/docker-build-workflow/actions/workflows/call-local-docker-build.yaml/badge.svg)](https://github.com/devourer66/docker-build-workflow/actions/workflows/call-local-docker-build.yaml)

A Reusable Workflow of the Docker GitHub Actions steps. Enhanced with learnings from production use.
AUTHOR: [Bret Fisher](https://github.com/BretFisher/docker-build-workflow)

## Reasons to use this workflow

1. Easier to start with than hand-building all the Docker Actions into a single workflow.
2. Provides inline docs based on real-world usage of this workflow.
3. Gives you inputs so you can reuse this workflow across many repositories and only needing the full workflow stored in a central repository.
4. New in 2023: Adds [SBOM and Provenance](https://docs.docker.com/build/attestations/) metadata to your images.
5. New in 2023: [Example template](./templates/call-docker-build-promote.yaml) to use the reusable workflow twice, in an "image promotion" style of dual registries (one for devs and PRs, one for production after PR merges)

## Steps to adopt this workflow

1. Copy "calling" workflow [`templates/call-docker-build.yaml`](templates/call-docker-build.yaml) to all the repositories you want to build images in and change it to point to the forked workflow above.

## "But what does this workflow really do beyond just `docker build`?"

1. Clone the repository
2. Setup QEMU for multi-platform building (via buildx) via docker/setup-qemu-action
3. Setup buildx for awesome and fast building via docker/setup-buildx-action
4. Log into Docker Hub and/or GHCR
5. Add labels and tags via docker/metadata-action
6. Build and push image via docker/build-push-action with GitHub-based layer caching
7. Reports tags and labels in the PR comments

## What other ways can I use this workflow?

A more advanced example of using this reusable workflow to do a "promotion" style workflow of:

1. On PR creation, build and push to a "dev" registry (GHCR)
2. On PR merge, build and push to a "prod" registry (Docker Hub)
3. Create a GitOps YAML update PR to update image tags
4. Notify of GitOps PR creation in Slack

Link to the example in [github-actions-templates](https://github.com/BretFisher/github-actions-templates) repository. It calls the reusable `reusable-docker-build.yaml` file in this repository.

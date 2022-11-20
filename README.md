#  Build and push a Docker image GitHub action

Builds a Docker image and pushes it into a registry

# Usage


## Inputs

* `registry_address` - Docker registry address
* `registry_username` - Username to authorise in registry
* `registry_token` - Token to authorise in registry
* `name` - Name of the Docker image (incl. repository, excl. version)
* `build_args` - Additional build arguments to pass to docker build command
* `planned_version` - Version to assign to the image
* `default_branch` - Default branch name that would not be added to version tag part
* `fail_on_error` - Fail the pipeline on error

## Outputs

* `image_full_name` - Full name of the pushed image

# Example

```yaml
---
name: Release

on:
  push:
    branches:
      - "*"

jobs:
  build_docker_image:
    name: Build Docker image
    runs-on: ubuntu-latest
    outputs:
      image_full_name: ${{ steps.build_docker_image.outputs.image_full_name }}

    steps:

      - name: Check out code
        uses: actions/checkout@v2
        with:
          fetch-depth: 0

      - name: Build Docker image
        id: build_docker_image
        uses: reinvented-stuff/build-docker-image-action@v2.0.0
        with:
          registry_address: "ghcr.io"
          registry_username: "${{ github.actor }}"
          registry_token: "${{ secrets.GITHUB_TOKEN }}"
          name: "${{ github.repository }}/${{ env.APP_NAME }}"
          planned_version: "1.0.0"

...

```
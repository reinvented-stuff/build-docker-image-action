#  Build and push a Docker image GitHub action

Builds a Docker image and pushes it into a registry

# Usage


## Inputs

* `github_token` - Pass the secrets.GITHUB_TOKEN variable
* `name` - Docker image name
* `planned_version` — Version to assign to the image

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
      image_full_name: ${{ steps.validate_new_version.outputs.branch_name }}

    steps:

      - name: Check out code
        uses: actions/checkout@v2
        with:
          fetch-depth: 0

      - name: Build Docker image
        id: build_docker_image
        uses: reinvented-stuff/build-docker-image-action@master
        with:
          github_token: "${{secrets.GITHUB_TOKEN}}"
          name: "WonkasFactory"
          planned_version: 9.0.0.0
...

```
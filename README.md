# Reusable Actions and Workflows

This repository holds a variety of workflows and composite actions to be used in projects.
**Note: this is a public repository due to GitHub's policy on reusing in Actions. Be sure no sensitive information or secrets are stored here**

## Workflows

### Actions Versioning (`actions-versioning`)

Used specifically to version this repository, not to be used in other workflows.

- Inputs:
  - None
- Outputs:
  - None

### Azure Deployment (`azure-deployment.yaml`)

Uploads single modules as layered deployments to GitHub Actions

- Inputs:
  - `image-name`: the full name of the Docker image to be deployed
    - must include registry, name, and version
  - `module`: the module name in Azure

### Iot Module Build (`iot-dev.yaml`)

Versions, names, builds, and pushes a Docker container for IoT

- Inputs:

  - `image`: base image name (i.e. jetsonradarmodule)
  - `image-registry`: the URL of the registry to push to

- Outputs:
  - `image-name`: image name with registry and version

### IoT Module Production (`iot-prod.yaml`)

Versions, names, builds, and pushes a "production ready" Docker Image

- Inputs:
  - `image`: the base image name
  - `image-registry`: the docker registry to be pushed to
- Outputs:
  - `image-name`: the final image name (with registry, name, and tag)

## Composite Actions

### Docker Build and Push (`docker-build-push`)

Names, builds, and pushes a Docker image

- Inputs:
  - `image`: image base name
  - `image-registry`: the registry to push the image to
  - `platforms`: platforms and architectures to build/push to
    - can be a string: `'linux/arm64'`
    - can also be a comma-separated string: `'linux/arm64,linux/amd64'`
    - note: The base architecture does _not_ need to be one of the desired platforms or architectures, however it will often be quicker
  - `version` the version tag to be pushed (defaults to "latest")
- Outputs:
  - `image-name`: the full image name, including repository and version

### IoT Docker Login (`iot-docker-login`)

Login to the development and production docker registries

- Inputs:
  - `deploy_token_courier`: deploy token for courier, used to pull courier as a dependency
    - **should be provided as a repo or organization secret**
  - `dev_container_registry_password`: registry password for development
    - **should be provided as a repo or organization secret**
  - `prod_container_registry_password`: registry password for production
    - **should be provided as a repo or organization secret**

### Semantic Versioning (`semantic-versioning`)

Run semantic versioning/release on the repository. Must have the `.releaserc` properly configured.

- Inputs:
  - `github_token`: the GitHub token for the repository
    - Most likely `${{ secrets.GITHUB_TOKEN }}`
- Outputs:
  - `version`: the semantic version

### Short SHA Versioning (`sha-versioning`)

Takes the SHA of the latest commit on the branch and outputs the first seven characters, as well as the image name with registry.

- Inputs:
  - None
- Outputs:
  - `version`: short sha version

### Upload Azure Deployment Layer (`upload-azure-deployment`)

Uploads a deployment manifest to Azure for a single module

- Inputs:
  - `deployment-name`: name of the deployment
  - `layered`: if the deployment is layered
    - should be a boolean surrounded by quotes (`"true"` or `"false"`)
  - `priority`: deployment priority
  - `devices`: list of devices to deploy to
  - `module-name`: name of the module being deployed
  - `labels`: labels to include
  - `deployment-manifest`: the actual manifest being deployed
  - `connection-string`: string to be used for connecting
    - **should be provided as a repo or organization secret**

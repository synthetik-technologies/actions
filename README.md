# Reusable Actions and Workflows

This repository holds a variety of workflows and composite actions to be used in projects.
**Note: this is a public repository due to GitHub's policy on reusing in Actions. Be sure no sensitive information or secrets are stored here**

## Workflows

### Azure Deployment (`azure-deployment.yaml`)

_Uploads single modules as layered deployments to GitHub Actions_

#### Inputs

- `image-name`: The full name of the Docker image to be deployed
  - Must include registry, name, and version
- `module`: The module name in Azure

### Iot Module Build (`iot-dev.yaml`)

_Versions, names, builds, and pushes a Docker container for IoT_

#### Inputs

- `image`: Base image name (i.e. jetsonradarmodule)
- `image-registry`: URL of the registry to push to

#### Outputs

- `image-name`: Image name with registry and version

## Composite Actions

### Docker Build and Push (`docker-build-push`)

Builds and pushes a Docker image

- Inputs:
  - `image-name`: Image name with registry and version
  - `platforms`: Platforms and architectures to build/push to
    - Can be a string: `'linux/arm64'`
    - Can also be a comma-separated string: `'linux/arm64,linux/amd64'`
    - Note: The base architecture does _not_ need to be one of the desired platforms or architectures, however it will often be quicker

### IoT Docker Login (`iot-docker-login`)

Login to the development and production docker registries

- Inputs:
  - `deploy_token_courier`: deploy token for courier, used to pull courier as a dependency
    - **should be provided as a repo or organization secret**
  - `dev_container_registry_password`: registry password for development
    - **should be provided as a repo or organization secret**
  - `prod_container_registry_password`: registry password for production
    - **should be provided as a repo or organization secret**

### Short SHA Versioning (`sha-versioning`)

Takes the SHA of the latest commit on the branch and outputs the first seven characters, as well as the image name with registry.

- Inputs:
  - `image`: the base image name
  - `image-registry` the target registry for the image
- Outputs:
  - `version`: short sha version
  - `image-name`: full image name

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

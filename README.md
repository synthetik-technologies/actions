# Reusable Actions and Workflows

This repository holds a variety of workflows and composite actions to be used in projects.

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

# Before You Start

Before running AGU, ensure that the required host environment, IBM product entitlement, registry access, and network connectivity are available.

AGU operations require connectivity to external sources or to resources within the customer's private network. The required connectivity depends on the operation being performed.

The requirements are grouped into the following areas:

- **System Requirements** — Host operating system, disk space, shell, and required base utilities, More about [System Requirements](#system-requirements)
- **IBM Entitlement Key** — Access required to retrieve entitled IBM product images, More about [IBM Entitlement Key](#ibm-entitlement-key)
- **Private Registry** — Target registry where AGU publishes images and operator catalog artifacts, More about [Private Registry](#private-registry)
- **Network Access** — Connectivity required for each AGU operation, More about [Network Access](#network-access)

## System Requirements

| Requirement | Minimum |
|---|---|
| OS | Linux (kernel 3.10+) or Windows WSL2 |
| Disk space | 50 GB (ELM HC) · 30 GB (AI Hub, Rhapsody SE) |
| Shell | bash 3.x |
| Host tools | `curl`, `tar` |

> `podman`, `skopeo`, `opm`, and `yq` are downloaded and cached automatically by the **Prerequisite** operation.

## IBM Entitlement Key

An IBM entitlement key is required to retrieve product images from the IBM Container Registry (`cp.icr.io`).

- Obtain the key from [MyIBM Container Software Library](https://myibm.ibm.com/products-services/containerlibrary).
- Ensure the entitlement key provides access to the product images you intend to download:
  - IBM Engineering Lifecycle Management on Hybrid Cloud
  - IBM Engineering AI Hub
  - IBM Rhapsody Systems Engineering

## Private Registry

A private container registry is required as the target location for container images and operator catalog artifacts prepared by AGU for use in the air-gapped environment.

The registry can be any supported container registry available in the customer's environment, such as **JFrog Artifactory, Harbor, or another enterprise container registry**.

The registry must:

- Be reachable from the host running AGU.
- Provide a target namespace or repository for AGU artifacts.
- Provide credentials with permission to push images and operator catalogs.

## Network Access

Network connectivity requirements depend on the AGU operation being performed.

| AGU Operation | Connectivity Required | Description |
|---|---|---|
| **Prerequisite** | Local | Performs environment and required tool validation. |
| **Download Case Bundle** | Internet | Downloads the required Cloud Pak case bundle and related resources. |
| **Extract Container Images** | Internet | Retrieves required container images from external registries. |
| **Publish Operator Catalog** | Private Network | Publishes the operator catalog to the customer's private registry. |
| **Publish Registry Images** | Private Network | Publishes container images to the customer's private registry. |
| **Rebuild Operator Catalog** | Private Network | Rebuilds the operator catalog using images available in the private registry. |

> **Note:** Internet connectivity is required only for operations that retrieve resources from external sources. Operations that interact with the private registry require connectivity to the customer's private network and do not require external internet access.
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

The bastion/utility host running AGU must have outbound HTTPS connectivity (TCP/443) to the required external domains.

> **Network Allowlist Requirement:** Customers should allow connectivity to the domains listed below, including any subdomains used by the respective services. Where applicable, wildcard domain patterns (`*.<domain>`) should be permitted to accommodate service endpoints that may vary or change over time.

| AGU Operation | Connectivity | Required Domain Access |
|---|---|---|
| **Prerequisite** | Local | No network connectivity required |
| **Download Case Bundle** | Internet | `github.com`, `api.github.com`, `github.ibm.com`, `raw.githubusercontent.com` |
| **Extract Container Images** | Internet | `*.icr.io`, `quay.io` |
| **Publish Operator Catalog** | Private Network | Customer's private container registry |
| **Publish Registry Images** | Private Network | Customer's private container registry |
| **Rebuild Operator Catalog** | Private Network | Customer's private container registry |

### External Domain Allowlist

The following domains must be reachable from the bastion/utility host:

| Domain | Purpose |
|---|---|
| `*.icr.io` | IBM Container Registry and associated image delivery endpoints |
| `quay.io` | Red Hat Quay Container Registry |
| `github.com` | GitHub — CLI binaries and Cloud Pak case bundle resources |
| `api.github.com` | GitHub REST API — case bundle version discovery |
| `github.ibm.com` | IBM GitHub — Cloud Pak case bundle repository |
| `raw.githubusercontent.com` | GitHub raw content — files and resources referenced by GitHub repositories |

All external connections are **outbound HTTPS over TCP/443**. No inbound firewall rules are required. DNS resolution for the allowed domains must also be available from the bastion/utility host.

> **Note:** Internet connectivity is required only for operations that retrieve resources from external sources. Operations that interact with the private registry require connectivity to the customer's private network and do not require external internet access.
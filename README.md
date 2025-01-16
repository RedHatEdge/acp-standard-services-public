# ACP Standard Services
This repo contains helm charts and general automation for installing and configuring the [standard services](https://github.com/RedHatEdge/patterns/blob/main/patterns/rh-acp-standard-services/README.md) that run on an [ACP](https://github.com/RedHatEdge/patterns/blob/main/patterns/acp-standardized-architecture-ha/README.md).

This repo is intended to be used with the declarative state management service on an ACP or hub. Bootstrapping that service may be required.

[Helm](https://helm.sh/) is used to offer templating functionality around the installation and configuration of the services. Helm clients can be downloaded [here](https://helm.sh/docs/intro/install/).

## Bootstrapping the Declarative State Management Service
If using a standalone ACP, such as a disconnected ACP or without a hub, bootstrapping the declarative state management service is required to begin deploying services to the platform.

To begin, bootstrap the declarative state management service:
```
helm install declarative-state-management charts/declarative-statemanagement/
```

The installation should only take a few moments.

## Structure
Within the charts directory, a parent chart named 'acp-standard-services' will create the [parent application](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/) which then installs the child applications, which are the individual services.

The deployment of these services is controlled by template variables, allowing for fine-tuning of what services should be deployed.

## Services
Below are the services deployed by the parent `acp-standard-services` application.

### Local Storage for Converged Storage
[The local storage for converged service](https://github.com/RedHatEdge/patterns/blob/main/blocks/local-storage-for-converged-storage/README.md) is used as setup for the converged service. To deploy this service, define the following:

```yaml
localStorageForConvergedStorage:
  # What nodes to look for local storage on - match to node names
  nodes:
    - node0
    - node1
    - node2
  # Storage class to put the located storage into
  storageClassName: local-disks
  volumeMode: Block
  # How to find storage devices
  deviceInclusionSpec:
    deviceTypes:
      - disk
    deviceMechanicalProperties:
      - NonRotational
```

### Converged Storage
Converged storage is provided by [Red Hat ODF](https://www.redhat.com/en/technologies/cloud-computing/openshift-data-foundation), and consumes the local storage provided by the local storage for converged storage service.

To deploy, use the following vars:

```yaml
convergedStorage:
  # Match to node names that should be storage nodes
  nodes:
    - node0
    - node1
    - node2
  # Number of disks across all servers
  totalNumDisks: 12
  # Replica 3 is the default
  replicas: 3
  # For limited resource clusters, use lean. For more powerful clusters, use balanced or performance.
  resourceProfile: lean
  # Match this to your cluster version
  version: 4.17
```

### Local Storage
If not using converged storage, local storage can be provided by using local disks.

Define the following to setup this storage option:

```yaml
localStorage:
  forceWipeDrives: true
  deviceClasses:
    - name: node0-local-storage
      nodes:
        - node0
      deviceSelectorPaths:
        - /dev/sda
      default: true
    - name: node1-local-storage
      nodes:
        - node1
      deviceSelectorPaths:
        - /dev/sdb
    - name: node2-local-storage
      nodes:
        - node2
      deviceSelectorPaths:
        - /dev/sdb
```
 
### Virtualization

```yaml

```
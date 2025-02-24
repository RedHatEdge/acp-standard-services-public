# Converged Storage Service
This chart deploys the converged storage service, which provides converged storage using local storage across the nodes of an ACP.

## Required Values
This chart requires a few inputs to configure the service, see below for more information:

```yaml
# Ensure the local-storage-for-converged-storage service is also installed before installing the converged-storage service
# Some values are required from the local-storage-for-converged-storage service configuration:
localStorageForConvergedStorage:
  # List of nodes to find disks on
  nodes:
    - node0
    - node1
    - node2
  # Name of the storage class 
  storageClassName: local-disks
  # Mode for the volume requests
  volumeMode: Block
  # What devices to include
  # This will be applied as stated in your values.yaml file
  deviceInclusionSpec:
    deviceTypes:
      - disk
    deviceMechanicalProperties:
      - NonRotational

# Set this to trigger installation of the service
convergedStorage:
  # Match to the version of the platform
  version: 4.17
  # What nodes to run the converged storage service on
  nodes:
    - node0
    - node1
    - node2
  # Total number of disks across all nodes
  totalNumDisks: 12
  # Number of replicas of data - 3 is the default, 2 available via support exception
  replicas: 3
```

## Service Deployment
This service can be deployed individually, however, it's recommended to deploy it alongside other ACP services using the acp-standard-services parent application.

To deploy it individually, use the following command:
```
helm install -f /path/to/values.yaml converged-storage-service charts/converged-storage/
```
# Local Storage for Converged Storage Service
This chart deploys the local storage for converged storage service, which is used to identify local disks that will be used by the converged storage service. This service "presents" these disks in a predictable way so the converged storage service can consume them.

## Required Values
This chart requires a few inputs to configure the service, see below for more information:

```yaml
# Define this to trigger installation of the service
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
```

## Service Deployment
This service can be deployed individually, however, it's recommended to deploy it alongside other ACP services using the acp-standard-services parent application.

To deploy it individually, use the following command:
```
helm install -f /path/to/values.yaml local-storage-for-converged-storage-service charts/local-storage-for-converged-storage/
```
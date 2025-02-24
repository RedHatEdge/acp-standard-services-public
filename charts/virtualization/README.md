# Virtualization Service
This chart deploys the virtualization service, which is used to create and run virtual machines on an ACP. In addition, this template provides functionality around virtual machine templates, importing of virtual machines, snapshotting, and other virtual machine related functions.

## Required Values
This chart requires a few inputs to configure the service, see below for more information:

```yaml
# Define this to trigger installation of the service
virtualization:
  # If bridging virtual machines to a network, define vlanNetworkAttachmentDefinitions as a list
  vlanNetworkAttachmentDefinitions:
    - vlan: 106
      bridgeInterface: bridge106
```

## Service Deployment
This service can be deployed individually, however, it's recommended to deploy it alongside other ACP services using the acp-standard-services parent application.

To deploy it individually, use the following command:
```
helm install -f /path/to/values.yaml virtualization-service charts/virtualization/
```
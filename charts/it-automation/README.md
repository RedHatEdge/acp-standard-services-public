# IT Automation Service
This chart deploys the IT automation service, which is used to manage and run procedural automation against targets on the ACP and outside of the ACP, such as network switches, routers, Windows and Linux systems, and more.

## Required Values
This chart requires a few inputs to configure the service, see below for more information:

```yaml
# Define this to trigger installation of the service
itAutomation:
  # If AAP2.5+ is desired, specify ansibleAutomationPlatform
  # Defaults to stable-2.5, can be overriden with:
  # overrideAnsibleAutomationPlatformVersion: "stable-2.6"
  ansibleAutomationPlatform:
    database:
      storageClass: ocs-storagecluster-ceph-rbd
    hub:
      storageType: file
      storageClass: ocs-storagecluster-cephfs
      storageSize: 100Gi
    lightspeed:
      disabled: true

  # If AAP2.4 is desired, specify automationController
  automationController:
    storageClass: ocs-storagecluster-ceph-rbd
    replicas: 1
  
  # This chart can optionally apply an initial controller configuration.
  # Populate the corresponding vars according to: https://github.com/redhat-cop/infra.aap_configuration/tree/devel
  # Only use if installing AAP2.5 for now
  controllerSetup: |
    manifest_url: http://your-manifest.example.com
    aap_organizations:
      - name: Your Organization
        description: Custom created organization
    
  automationHub:
    storageType: file
    fileStorage:
      size: 100Gi
      storageClass: ocs-storagecluster-cephfs
      accessMode: ReadWriteMany
    postgres:
      storage:
        request: 8Gi
        limit: 50Gi
      resources:
        limits:
          cpu: 1000m
          memory: 8Gi
        requests:
          cpu: 500m
          memory: 2Gi
```

## Service Deployment
This service can be deployed individually, however, it's recommended to deploy it alongside other ACP services using the acp-standard-services parent application.

To deploy it individually, use the following command:
```
helm install -f /path/to/values.yaml it-automation-service charts/it-automation/
```
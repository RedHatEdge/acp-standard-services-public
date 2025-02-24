# IT Automation Service
This chart deploys the IT automation service, which is used to manage and run procedural automation against targets on the ACP and outside of the ACP, such as network switches, routers, Windows and Linux systems, and more.

## Required Values
This chart requires a few inputs to configure the service, see below for more information:

```yaml
# Define this to trigger installation of the service
itAutomation:
  # Version 2.4 for now, 2.5 not ready for primetime.
  version: 2.4
  # Vars specific to controller
  automationController:
    storageClass: ocs-storagecluster-ceph-rbd
    replicas: 1
```

## Service Deployment
This service can be deployed individually, however, it's recommended to deploy it alongside other ACP services using the acp-standard-services parent application.

To deploy it individually, use the following command:
```
helm install -f /path/to/values.yaml it-automation-service charts/it-automation/
```
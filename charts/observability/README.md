# Observability Service
This chart deploys the Observability service, which is based on [Red Hat's Observability operator](https://www.redhat.com/en/topics/devops/what-is-observability), on an ACP or on a hub cluster. This service can be used to automatically gather, store, and visualize important information about a system running on an ACP, or optionally, consolidate metrics that have been processed through the telemetry service.

## Required Values
This operator watches all namespaces for various resources, and thus doesn't require many inputs at installation time:

```yaml
# Define this to trigger installation of the service
observability:
  # What channel to use, where 'latest' is the default 
  channel: latest
  # The upgrade strategy, where 'default' is the default
  upgradeStrategy: Default
```

## Service Deployment
This service can be deployed individually, however, it's recommended to deploy it alongside other ACP services using the acp-standard-services parent application.

To deploy it individually, use the following command:
```
helm install -f /path/to/values.yaml telemetry-service charts/telemetry/
```
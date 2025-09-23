# Telemetry Service
This chart deploys the telemetry service, which is based on [Red Hat's build of OTEL](https://www.redhat.com/en/blog/red-hat-openshift-opentelemetry-otlp-native-platform), on an ACP or on a hub cluster. This service can be used to gather, collect, curate, aggragrate, and export metrics according to the [OpenTelemetry framework](https://opentelemetry.io/docs/).

## Required Values
This operator watches all namespaces for various resources, and thus doesn't require many inputs at installation time:

```yaml
# Define this to trigger installation of the service
telemetry:
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
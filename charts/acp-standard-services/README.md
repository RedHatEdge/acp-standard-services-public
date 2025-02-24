# ACP Standard Services Chart
This is a parent chart used to deploy the core services of an ACP. The templates contained within create applications that correspond to the various services.

## Triggering Service Deployment
Services are triggered by defining a top level key, causing this application to render the approriate application definition and apply it via the desired state configuration service.

For example, to deploy the `certificate-management` service, define the appropriate key in a values.yaml file:
```yaml
---
certificateManagement:
  key1: value1
  key2: value2
```

The templates within this chart will automatically pass values along to the rendered applications for services.

## Available Services and Configurations
| Service Name | Description | Repository Path | Required Values |
| --- | --- | --- | --- |
| Certificate Management | Manages the issuing of certificates for core platform functions and other applications | `charts/certificate-management` | [Link](../certificate-management/README.md#required-values) |

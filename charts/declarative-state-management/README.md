# Declarative State Management Bootstrapping
This chart is used to bootstrap declarative state management/GitOps functionality on an ACP. Typically, this service is used to drive the application and configuration of other services, so the installation of this should be done before attempting to install the other services.

## Installing the Chart
To install this chart, run the following command:
```
helm install declarative-state-management charts/declarative-state-management/
```

The service will take a few moments to start.

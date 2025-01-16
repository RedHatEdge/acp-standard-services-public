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


# Network Interface Management Service
This chart deploys the network interface management service, which is used to create and modify physical and logical interfaces of an ACP. This service can be used to create VLAN interfaces, bridges for virtual machine connectivity, and more.

> Note:
>
> DO NOT use this service to modify the cluster interface used during install. Doing so will break the node connectivity and require a re-install. Creating sub-interfaces on top of the cluster interface are fine. For example, if a bond is used as the primary cluster link, VLAN interfaces can be created "on top" of it.

## Required Values
This chart requires a few inputs to configure the service, see below for more information:

```yaml
# Define this to trigger installation of the service
networkInterfaceManagement:
  # Create a list of interfaces that should be managed by the service
  - name: node0-vlan106
    # Interfaces are node specific, ensure the .node value matches the node to apply the confiruation on
    node: node0
    # List of interfaces to manage on the node
    # For interface configuration, use the nmstate definition
    # Refer to the docs fow a full list of interface types and examples: https://nmstate.io/
    # Interfaces do not need to have IP addresses, but can if desired
    interfaces:
      # Create a vlan interface on a parent bond interface
      - name: bond0.106
        state: up
        type: vlan
        vlan:
          base-iface: bond0
          id: 106
      # Create a linux bridge on a vlan interface for virtual machine bridged traffic
      - name: bridge106
        type: linux-bridge
        state: up
        bridge:
          port:
            - name: bond0.106
              vlan: {}
          options:
            stp:
              enabled: false
  # Repeat for node1
  - name: node1-vlan106
    node: node1
    interfaces:
      - name: bond0.106
        state: up
        type: vlan
        vlan:
          base-iface: bond0
          id: 106
      - name: bridge106
        type: linux-bridge
        state: up
        bridge:
          port:
            - name: bond0.106
              vlan: {}
          options:
            stp:
              enabled: false
  # Repeat for node2
  - name: node2-vlan106
    node: node2
    interfaces:
      - name: bond0.106
        state: up
        type: vlan
        vlan:
          base-iface: bond0
          id: 106
      - name: bridge106
        type: linux-bridge
        state: up
        bridge:
          port:
            - name: bond0.106
              vlan: {}
          options:
            stp:
              enabled: false
```

## Service Deployment
This service can be deployed individually, however, it's recommended to deploy it alongside other ACP services using the acp-standard-services parent application.

To deploy it individually, use the following command:
```
helm install -f /path/to/values.yaml interface-configuration-service charts/interface-configuration/
```
# Network — Topology

All documentation-safe addressing below uses `192.0.2.0/24` (TEST-NET) as examples.
Real subnets are `TODO(T7)`.

## Physical topology

```mermaid
flowchart TB
    ISP[ISP / Dynamic public IP] --> ROUTER[Main router]
    ROUTER --> SWITCH[8-port switch]
    SWITCH --> C[nova-ctrl]
    SWITCH --> N1[nova-01]
    SWITCH --> N2[nova-02]
    SWITCH --> N3[nova-03]
    SWITCH --> N4[nova-04]
    C --- NB[NetBird overlay]
    N1 -. k3s .- N2
    N2 -. k3s .- N3
    N1 -. k3s .- N3
    C --- TS[Tailscale overlay]
```

## Logical view

```mermaid
flowchart LR
    subgraph Internet
        A[Remote user]
    end
    A -- NetBird / Tailscale --> NB1[NetBird/Tailscale endpoint]
    NB1 --> C[nova-ctrl - VPN LXC x2]
    C --> LAN[Proxmox cluster network]
    LAN --> N1[nova-01 - k3s]
    LAN --> N2[nova-02 - k3s]
    LAN --> N3[nova-03 - k3s]
    LAN --> N4[nova-04 - workloads]
```

## Description

- The router is the single L3 gateway; all nodes hang off the 8-port switch.
- **k3s traffic** (control plane + workload data plane) flows over the cluster network
  between nova-01, nova-02, nova-03. `TODO(T2)` defines which node is the server.
- **Remote access** rides the NetBird and Tailscale overlays, so inbound connections
  are tunnelled through the VPN LXCs on nova-ctrl (`TODO(T3)`) rather than port-forwarded
  through the consumer router. This matches a dynamic-IP home link.
- **Public exposure** is limited to services explicitly marked internet-facing
  (`TODO(T10)`); Immich and Dokploy are known candidates.

## Subnets and VLANs

Segmentation uses VLANs; IDs and purposes are `TODO(T7)`. Which VLAN carries the nova
nodes is `TODO(T8)`. Local DNS/ad-blocking, if any, is `TODO(T9)`. See
[vlans-subnets.md](vlans-subnets.md).

Related: [vlans-subnets.md](vlans-subnets.md) · [vpn.md](../services/vpn.md) ·
[README](../README.md)
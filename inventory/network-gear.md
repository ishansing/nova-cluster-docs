# Inventory — Network Gear

Small consumer setup, consistent with a residential internet connection in India
(dynamic public IP, no SLA). Network details beyond gear are in
[network/topology.md](../network/topology.md).

## Gear table

| Device | Role | Model | Ports | Notes |
|---|---|---|---|---|
| Main router | Gateway / NAT / DHCP | Consumer-grade (model `TODO`) | — | Dynamic public IP; no static IP available |
| Network switch | L2 fabric | 8-port (model `TODO`) | 8× Gigabit | Connects all nova nodes |

## Knowns and unknowns

- **Router** is the single upstream device between the cluster and the ISP. It is
  consumer-grade: expect no SLA-backed uptime and no static addressing (`TODO`).
- **Switch** is a plain 8-port unit — no managed VLAN features assumed. VLAN
  segmentation is performed elsewhere if present (`TODO(T7)`).
- **VPN overlays**: NetBird and Tailscale provide remote access across the dynamic-IP
  link, so no port forwarding is required for those paths. See
  [services/vpn.md](../services/vpn.md).
- **No UPS** is currently listed. Power-loss resilience is a gap worth tracking:
  `TODO(T15)` if this becomes a recurring concern.

## Ports and physical wiring

Physical port mapping (which switch port feeds which node) is not yet documented —
mark it as needed during the next physical visit (`TODO`).

Related: [servers.md](servers.md) · [topology.md](../network/topology.md) ·
[README](../README.md)
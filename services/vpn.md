# Service — VPN & Overlays

Remote access into the homelab via NetBird and Tailscale mesh overlays.

## Tailscale LXC (observed)

| Field | Value |
|---|---|
| Host | nova-04 — lxc/102 (running, HA-managed) |
| vCPU / RAM | 1 / 512 MB (swap 512 MB) |
| Disk | 4 GB on `local` storage |
| Network | vmbr0, DHCP, firewall on; unprivileged Ubuntu container |
| Exposure | VPN-only by default; `TODO(T10)` |

## NetBird

Placement unknown — no NetBird guest exists on any online node. It may run on
the offline nova-ctrl, on k3s, or outside the cluster (`TODO(T3)`).

## nova-ctrl LXCs

Unknown — the node was offline at snapshot time (`TODO(T3)`).

## Why overlays over port forwarding

The home connection has a dynamic public IP and a consumer-grade router — no static
address and no SLA. NetBird/Tailscale give stable, authenticated remote access over the
dynamic link, and keep inbound attack surface small. See
[network/topology.md](../network/topology.md).

## Operations notes

- The Tailscale LXC is currently the only verified remote-admin path into the
  cluster. Keep it updated (`TODO(T15)`).
- Reconnect/restart steps for each overlay are `TODO(T3)` until the stacks are
  identified.

Related: [index.md](index.md) · [network/topology.md](../network/topology.md) ·
[README](../README.md)
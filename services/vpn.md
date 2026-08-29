# Service — VPN LXCs & Overlays

Remote access into the homelab. Two VPN LXCs run on **nova-ctrl**; their names, stacks,
and roles are `TODO(T3)`.

## Overlays

| Overlay | Use | Notes |
|---|---|---|
| NetBird | Mesh VPN overlay | Route to services without port forwarding |
| Tailscale | Mesh VPN overlay | Route to services without port forwarding |

## Facts

| Field | Value |
|---|---|
| Host | nova-ctrl (2× LXC) |
| Purpose | Remote access / tunneling |
| LXC names/stacks | `TODO(T3)` |
| Exposure | VPN-only by default; `TODO(T10)` |

## Why overlays over port forwarding

The home connection has a dynamic public IP and a consumer-grade router — no static
address and no SLA. NetBird/Tailscale give stable, authenticated remote access over the
dynamic link, and keep inbound attack surface small. See
[network/topology.md](../network/topology.md).

## Operations notes

- The VPN LXCs are the primary remote-admin path into the cluster. Keep them updated
  and note them in the k3s upgrade cadence too (`TODO(T15)`).
- Reconnect/restart steps for each overlay are `TODO(T3)` until the stacks are
  identified.

Related: [index.md](index.md) · [network/topology.md](../network/topology.md) ·
[README](../README.md)
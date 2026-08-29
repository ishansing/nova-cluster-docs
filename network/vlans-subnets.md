# Network — VLANs & Subnets

VLAN-based segmentation is in use, but the concrete scheme has not been documented yet.
This file records the agreed structure once confirmed; until then every row is a
`TODO(T7)`/`TODO(T8)`.

## VLAN table

| VLAN ID | Purpose | Subnet | Hosts | Status |
|---|---|---|---|---|
| `TODO(T7)` | Management (Proxmox) | `TODO(T7)` | nova nodes (`TODO(T8)`) | Open |
| `TODO(T7)` | k3s / cluster traffic | `TODO(T7)` | nova-01..03 | Open |
| `TODO(T7)` | Services / workloads | `TODO(T7)` | nova-04 VMs & LXCs | Open |
| `TODO(T7)` | Guest / untrusted | `TODO(T7)` | — | Open |

Nothing in this table is a confirmed fact yet — do not use these rows for
troubleshooting until the IDs are filled in.

## Intended segmentation model

The design goal (from [security/model.md](../security/model.md)) is to keep management,
cluster, and workload traffic separated so that:

- The Proxmox management plane is not directly reachable from service VMs.
- k3s control-plane and data-plane traffic have their own path.
- Internet-facing services (Immich, Dokploy — see [exposure](../services/index.md))
  are isolated from the rest of the LAN.

## DNS & ad-blocking

No local DNS or ad-blocking is documented yet — `TODO(T9)`.

Related: [topology.md](topology.md) · [network-gear.md](../inventory/network-gear.md) ·
[README](../README.md)
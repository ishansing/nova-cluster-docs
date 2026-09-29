# Services — Index

Every Proxmox guest in the cluster, as observed via the read-only API on
2026-09-27. Per-service pages hold the detail; this page is the catalog.

## Catalog

| Service | Host | Type | Guest ID | Purpose | Exposure | Page |
|---|---|---|---|---|---|---|
| Immich | nova-04 | VM | 101 (running) | Photo management | Exposed | [immich.md](immich.md) |
| Tailscale | nova-04 | LXC | 102 (running) | Mesh VPN endpoint | `TODO(T10)` | [vpn.md](vpn.md) |
| Navidrome | nova-04 | LXC | 103 (stopped) | Music streaming | `TODO(T10)` | [navidrome.md](navidrome.md) |
| Hermes agent | nova-03 | VM | 111 (running) | AI agent stack | `TODO(T10)` | [hermes-agent.md](hermes-agent.md) |

## Not in Proxmox

- **Dokploy / Gitea** — no such guests exist in the cluster. Earlier docs listed
  them on nova-04; those pages have been removed. If they are re-provisioned,
  re-add them here with their new guest IDs.
- **nova-ctrl guests** — unknown; the node was offline at snapshot time
  (`TODO(T3)`).

## Common service template

Every service page records, where known:

- **Purpose** — what it does and who uses it.
- **URL/domain** — `TODO(T11)` until confirmed.
- **Dependencies** — storage, platform, other services.
- **Exposure** — internet-facing vs. VPN-only vs. LAN-only — `TODO(T10)`.
- **Data location** — ZFS dataset, VM disk, or share — `TODO(T12)`.
- **Backup & restore** — `TODO(T13)`.

## Placement summary

- nova-ctrl: unknown (offline 2026-09-27).
- nova-04: Immich (101), Tailscale (102), Navidrome (103, stopped).
- nova-03: Hermes agent (111) alongside k3s (`TODO(T2)`).
- nova-01..02: k3s only — no Proxmox guests; k3s workloads themselves are not
  yet catalogued (`TODO(T2)`).

Related: [storage](../storage/zfs-and-pools.md) · [backup-and-dr.md](../backup-and-dr.md)
· [README](../README.md)
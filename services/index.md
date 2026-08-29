# Services — Index

All self-hosted services. Per-service pages hold the detail; this page is the catalog.

## Catalog

| Service | Host | Type | Purpose | Exposure | Page |
|---|---|---|---|---|---|
| VPN LXC ×2 | nova-ctrl | LXC | Remote access / tunneling | `TODO(T10)` | [vpn.md](vpn.md) |
| Navidrome | nova-04 | LXC | Music streaming | `TODO(T10)` | [navidrome.md](navidrome.md) |
| Immich | nova-04 | VM | Photo management | Exposed | [immich.md](immich.md) |
| Dokploy | nova-04 | VM | App hosting / deployment | Exposed | [dokploy.md](dokploy.md) |
| Gitea | nova-04 | VM | Self-hosted Git | `TODO(T10)` | [gitea.md](gitea.md) |
| Hermes agent | nova-04 | VM | AI agent stack | `TODO(T10)` | [hermes-agent.md](hermes-agent.md) |

## Common service template

Every service page records, where known:

- **Purpose** — what it does and who uses it.
- **URL/domain** — `TODO(T11)` until confirmed.
- **Dependencies** — storage, platform, other services.
- **Exposure** — internet-facing vs. VPN-only vs. LAN-only — `TODO(T10)`.
- **Data location** — ZFS dataset, VM disk, or share — `TODO(T12)`.
- **Backup & restore** — `TODO(T13)`.

## Placement summary

- nova-ctrl: VPN LXCs only.
- nova-04: the application VMs/LXCs (Immich, Dokploy, Gitea, Navidrome, Hermes).
- nova-01..03: k3s-hosted workloads (via Dokploy or direct manifests) — any k3s
  workloads are not yet catalogued (`TODO(T2)`).

Related: [storage](../storage/zfs-and-pools.md) · [backup-and-dr.md](../backup-and-dr.md)
· [README](../README.md)
# Inventory — Servers

All five nodes are members of the single **nova** Proxmox VE cluster with HA enabled.
All nodes run the same Proxmox VE release; the exact version is `TODO(T1)`.

## Node table

| Node | Role | CPU | RAM | Storage | Notes |
|---|---|---|---|---|---|
| nova-ctrl | Infra management / control-plane utilities | Intel i7 (older gen) | 8 GB DDR3 | 1 TB HDD | Runs 2 VPN LXCs `TODO(T3)` |
| nova-01 | k3s node | Intel i5, 8th gen | 16 GB DDR4 | 256 GB SSD + 1 TB HDD (ZFS) | Server or worker `TODO(T2)` |
| nova-02 | k3s node | Intel i5, 8th gen | 16 GB DDR4 | 256 GB SSD + 1 TB HDD (ZFS) | Server or worker `TODO(T2)` |
| nova-03 | k3s node | Intel i5, 8th gen | 16 GB DDR4 | 256 GB SSD + 1 TB HDD (ZFS) | Server or worker `TODO(T2)` |
| nova-04 | Workload host | Intel i5, 8th gen | 16 GB DDR4 | 256 GB SSD + 1 TB HDD (ZFS) | Hosts most application VMs/LXCs |

## Responsibilities by node

- **nova-ctrl** — management utilities and remote-access VPN LXCs. The lightest node
  (8 GB RAM); it runs no k3s or application workloads.
- **nova-01 .. nova-03** — the k3s cluster. Control-plane placement is
  `TODO(T2)`.
- **nova-04** — the primary workload host: Immich, Dokploy, Gitea, Navidrome, and the
  Hermes agent all live here. See [services/index.md](../services/index.md).

## Disk layout convention

Per worker node: the 256 GB SSD holds operating-system and boot disks; the 1 TB HDD is
dedicated to ZFS for bulk data. See [zfs-and-pools.md](../storage/zfs-and-pools.md).

## Quick reference

- Console access: Proxmox web UI or SSH (keys stored in password manager entry
  `nova-ssh` — see [security/model.md](../security/model.md)).
- OS on every node: Proxmox VE (version `TODO(T1)`).

Related: [network-gear.md](network-gear.md) · [README](../README.md)
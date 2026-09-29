# Inventory — Servers

All five nodes are members of the single **nova** Proxmox VE cluster with HA enabled.
All nodes run the same Proxmox VE release; the exact version is `TODO(T1)`.

## Node table

| Node | IP (local only) | Role | CPU | RAM | Storage | Notes |
|---|---|---|---|---|---|---|
| nova-ctrl | unknown (offline) | Infra management / control-plane utilities | Intel i7 (older gen) | 8 GB DDR3 | 1 TB HDD | Offline 2026-09-27; guests unknown (`TODO(T3)`) |
| nova-01 | 10.0.0.101 | k3s node | Intel i5, 8th gen | 16 GB DDR4 | 256 GB SSD + 1 TB HDD (ZFS) | Server or worker `TODO(T2)`; no guests observed 2026-09-27 |
| nova-02 | 10.0.0.102 | k3s node | Intel i5, 8th gen | 16 GB DDR4 | 256 GB SSD + 1 TB HDD (ZFS) | Server or worker `TODO(T2)`; no guests observed 2026-09-27 |
| nova-03 | 10.0.0.103 | k3s node | Intel i5, 8th gen | 16 GB DDR4 | 256 GB SSD + 1 TB HDD (ZFS) | Server or worker `TODO(T2)`; hosts qemu/111 `hermes` (running) |
| nova-04 | 10.0.0.104 | Workload host | Intel i5, 8th gen | 16 GB DDR4 | 256 GB SSD + 1 TB HDD (ZFS) | Hosts qemu/101 `immich`, lxc/102 `tailscale`, lxc/103 `navidrome` (observed 2026-09-27) |

Node IPs from live `GET /cluster/status` 2026-09-29; all on 10.0.0.0/24. No public IPs recorded. VLAN placement is `TODO(T8)`.

## Responsibilities by node

- **nova-ctrl** — management utilities. Offline as of 2026-09-27, so its guests
  are unknown (`TODO(T3)`).
- **nova-01 / nova-02** — k3s only. Control-plane placement is `TODO(T2)`;
  neither node hosts any Proxmox guests.
- **nova-03** — k3s (`TODO(T2)`) plus the `hermes` agent VM (qemu/111).
- **nova-04** — application guests: `immich` (qemu/101), `tailscale` (lxc/102),
  `navidrome` (lxc/103, stopped). See [services/index.md](../services/index.md).

## Disk layout convention

Per worker node: the 256 GB SSD holds operating-system and boot disks; the 1 TB HDD is
dedicated to ZFS for bulk data. See [zfs-and-pools.md](../storage/zfs-and-pools.md).

## Quick reference

- Console access: Proxmox web UI or SSH (keys stored in password manager entry
  `nova-ssh` — see [security/model.md](../security/model.md)).
- OS on every node: Proxmox VE (version `TODO(T1)`).

Related: [network-gear.md](network-gear.md) · [README](../README.md)
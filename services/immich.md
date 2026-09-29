# Service — Immich

Photo-management platform (self-hosted alternative to Google Photos).

## Facts

| Field | Value |
|---|---|
| Host | nova-04 — qemu/101 (running) |
| vCPU / RAM | 4 / 8 GB |
| Disk | 20 GB on `local` storage (`local:101/vm-101-disk-0.qcow2`) |
| Network | vmbr0, firewall on |
| Purpose | Photo & video management, backups from phones |
| Exposure | Internet-facing (confirmed) |
| URL | `TODO(T11)` |
| Data location | `TODO(T12)` |
| Backup | `TODO(T13)` |

## Dependencies

- Photo data lives on the VM's 20 GB disk on `local` storage; if the library
  outgrows it, plan a move to the ZFS pools (`TODO(T4)`/`TODO(T12)`).
- Needs a reverse proxy / tunnel for its public URL (`TODO(T11)`); inbound path via
  VPN overlays or port forward on the consumer router is `TODO(T10)` detail.

## Operations notes

- Storage: photos/videos are the bulk of the services pool. Plan for growth and back
  up independently of the app (`TODO(T13)`).
- Health check: login and confirm the "Job Status" page shows recent backup jobs.

Related: [index.md](index.md) · [zfs-and-pools.md](../storage/zfs-and-pools.md) ·
[README](../README.md)
# Service — Navidrome

Self-hosted music streaming server (Subsonic-compatible).

## Facts

| Field | Value |
|---|---|
| Host | nova-04 — lxc/103 (stopped) |
| vCPU / RAM | 1 / 1 GB (swap 512 MB) |
| Disk | 8 GB on `local` storage (`local:103/vm-103-disk-0.raw`) |
| Network | vmbr0, static IP (host octet masked), firewall on; unprivileged Ubuntu container |
| Purpose | Music streaming |
| Exposure | `TODO(T10)` |
| URL | `TODO(T11)` |
| Data location | `TODO(T12)` |
| Backup | `TODO(T13)` |

## Dependencies

- Root filesystem is the 8 GB disk on `local` storage. Where the music library
  itself lives is `TODO(T12)` — if it is meant to sit on the ZFS HDDs, that
  wiring is `TODO(T4)`.
- Clients connect via Subsonic API; if used remotely, route through the VPN overlays
  (`TODO(T10)`).

## Operations notes

- Media library is the main storage consumer; keep it on HDD ZFS, not SSD
  (`TODO(T6)` is already resolved to OS-on-SSD).
- Health check: open the web UI (`TODO(T11)`) and confirm a library scan ran.

Related: [index.md](index.md) · [zfs-and-pools.md](../storage/zfs-and-pools.md) ·
[README](../README.md)
# Service — Navidrome

Self-hosted music streaming server (Subsonic-compatible).

## Facts

| Field | Value |
|---|---|
| Host | nova-04 (LXC) |
| Purpose | Music streaming |
| Exposure | `TODO(T10)` |
| URL | `TODO(T11)` |
| Data location | `TODO(T12)` |
| Backup | `TODO(T13)` |

## Dependencies

- Runs as an LXC on nova-04; media library typically on the services ZFS pool
  (`TODO(T4)`/`TODO(T12)`).
- Clients connect via Subsonic API; if used remotely, route through the VPN overlays
  (`TODO(T10)`).

## Operations notes

- Media library is the main storage consumer; keep it on HDD ZFS, not SSD
  (`TODO(T6)` is already resolved to OS-on-SSD).
- Health check: open the web UI (`TODO(T11)`) and confirm a library scan ran.

Related: [index.md](index.md) · [zfs-and-pools.md](../storage/zfs-and-pools.md) ·
[README](../README.md)
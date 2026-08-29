# Backup & Disaster Recovery

Strategy for keeping nova recoverable on a dynamic-IP home link with no SLA. Per-service
backup plans are `TODO(T13)`; this page sets the shared principles.

## Current primitives

| Layer | Tool | Coverage |
|---|---|---|
| ZFS | Snapshots + send/recv | All pools (k3s-storage, services) — `TODO(T4)` |
| Proxmox | `vzdump` | VMs/LXCs (schedule `TODO(T13)`) |
| k3s | etcd snapshot | Control plane (`TODO(T2)`) |
| Offsite | Not yet defined | `TODO(T13)` |

## Principles

1. **Local first, cheap and frequent** — ZFS snapshots on the HDD pools are the base
   layer: near-zero cost, incremental, fast to restore.
2. **Offsite second** — media can be lost; Git repos and Immich photos cannot. Repos and
   photos need an offsite copy `TODO(T13)`.
3. **Backup per data class** — VM disks (data), Git repos (irreplaceable), media
   (re-derivable), agent state (`TODO(T12)`/`TODO(T13)`).

## DR scenarios

### Node failure

- HA is enabled; VMs/LXCs on a failed node restart on survivors where possible.
- Fencing hardware (IPMI/iLO) is unconfirmed — verify `TODO(T15)` before trusting HA
  failover.

### Pool failure

- Pools are mirror/RAIDZ (`TODO(T5)`), so a single-disk loss survives locally.
- Restore the VM/LXC from `vzdump`/snapshots.

### Whole-site loss

- Rebuild from offsite copies (media + Git + photos). Since the public IP is dynamic,
  re-point the VPN overlays and any DNS after rebuild.

## Restore drill

A restore drill (restore one VM from a snapshot into a test environment) is a valuable
`TODO(T15)` item — run it before trusting the backup story.

Related: [zfs-and-pools.md](storage/zfs-and-pools.md) ·
[services/index.md](services/index.md) · [README.md](README.md)
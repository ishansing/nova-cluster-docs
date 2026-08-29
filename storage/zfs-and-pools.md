# Storage — ZFS Pools & Datasets

Each worker node (nova-01..04) dedicates a **1 TB HDD to ZFS**. Two logical storage
areas exist:

- a ZFS pool for **k3s workloads** (cluster storage), and
- a ZFS pool for **self-hosted services** (VMs/LXCs).

Pools are **not** single-disk — they use mirror/RAIDZ redundancy (`TODO(T5)`). Which
node(s) own which pool is `TODO(T4)`.

## Pool layout

| Pool | Purpose | Redundancy | Owned by | Status |
|---|---|---|---|---|
| k3s-storage pool | k3s workloads / persistent volumes | mirror/RAIDZ (`TODO(T5)`) | `TODO(T4)` | Open |
| services pool | VM/LXC disks for self-hosted apps | mirror/RAIDZ (`TODO(T5)`) | `TODO(T4)` | Open |

## Disk split (SSD vs. HDD)

Per worker node (`TODO(T6)` resolved):

- **256 GB SSD** — operating system, boot, and root disks for VMs/LXCs.
- **1 TB HDD (ZFS)** — bulk data: databases, media, application data.

Expect VM disk layouts to follow this split; per-service data locations are
`TODO(T12)`.

## Key commands

Run on the node owning the pool as `root`:

```bash
# List pools and redundancy
zpool list

# List datasets
zfs list -r <pool>

# Pool health
zpool status

# Show most recent snapshots
zfs list -t snapshot
```

## Backup notes

ZFS snapshots are the natural backup primitive here (cheap, local, incremental). The
overall strategy is documented in [backup-and-dr.md](../backup-and-dr.md); per-service
strategy is `TODO(T13)`.

## Open questions

- Pool-to-node ownership — `TODO(T4)`
- Exact mirror vs. RAIDZ topology and vdev layout — `TODO(T5)`

Related: [k3s.md](../platforms/k3s.md) · [proxmox.md](../platforms/proxmox.md) ·
[README](../README.md)
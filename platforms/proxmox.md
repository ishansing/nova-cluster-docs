# Platform — Proxmox VE

The nova cluster is a single Proxmox VE cluster. All five nodes joined the cluster with
**HA enabled**. All nodes run the same Proxmox VE release — version number `TODO(T1)`.

## Cluster layout

| Node | Proxmox role | Workload |
|---|---|---|
| nova-ctrl | Member | VPN LXCs (`TODO(T3)`) |
| nova-01 | Member | k3s (`TODO(T2)`) |
| nova-02 | Member | k3s (`TODO(T2)`) |
| nova-03 | Member | k3s (`TODO(T2)`) |
| nova-04 | Member | Application VMs/LXCs |

## Key commands

Run on any cluster node as `root` (via `nova-ssh` credentials in the password manager):

```bash
# Cluster health
pvecm status

# List nodes and their state
pvesh get /cluster/resources --type node

# List all VMs/LXCs across the cluster
pvesh get /cluster/resources --type vm

# Show cluster config (HA groups, fencing, etc.)
cat /etc/pve/corosync.conf

# HA status
ha-manager status
```

## Storage on Proxmox

- Workers expose the 1 TB ZFS HDDs to Proxmox as ZFS pools (mirror/RAIDZ —
  `TODO(T5)`); the 256 GB SSDs carry OS/boot disks (`TODO(T6)`). See
  [zfs-and-pools.md](../storage/zfs-and-pools.md).
- Proxmox storage IDs and the pool-to-node mapping are `TODO(T4)`.

## HA & fencing

HA is enabled cluster-wide. Fencing hardware (IPMI/iLO) is not confirmed — verify before
relying on HA failover (`TODO(T15)`). Realistic expectation: a small home cluster with a
consumer router; HA helps for VMs that can run on more than one node.

## Upgrades

Follow [ops/runbooks/proxmox-upgrade.md](../ops/runbooks/proxmox-upgrade.md). Never
upgrade every node at once.

Related: [k3s.md](k3s.md) · [servers.md](../inventory/servers.md) ·
[README](../README.md)
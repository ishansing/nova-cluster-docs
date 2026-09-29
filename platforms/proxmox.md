# Platform — Proxmox VE

The nova cluster is a single Proxmox VE cluster. All five nodes joined the cluster with
**HA enabled**. All nodes run the same Proxmox VE release — version number `TODO(T1)`.

## Cluster layout

| Node | Proxmox role | Workload |
|---|---|---|
| nova-ctrl | Member | VPN LXCs (`TODO(T3)`) |
| nova-01 | Member | k3s (`TODO(T2)`) — no guests observed 2026-09-27 |
| nova-02 | Member | k3s (`TODO(T2)`) — no guests observed 2026-09-27 |
| nova-03 | Member | k3s (`TODO(T2)`) + qemu/111 `hermes` (running) |
| nova-04 | Member | Application VMs/LXCs (see below) |

## Observed state — 2026-09-27 (Proxmox API, read-only)

Snapshot via `GET /cluster/status`, `/nodes`, `/cluster/resources?type=vm`,
per-guest `config`, per-node `storage`, and `cluster/ha/resources`.
Cluster `Nova`: 5 members, quorate, version 9. Nodes live on `10.0.0.0/24`
(host octets masked per redaction policy).

| Node | Node ID | Status | Guests |
|---|---|---|---|
| nova-01 | 2 | online | none |
| nova-02 | 3 | online | none |
| nova-03 | 4 | online | qemu/111 `hermes` (running) |
| nova-04 | 5 | online | qemu/101 `immich` (running), lxc/102 `tailscale` (running), lxc/103 `navidrome` (stopped) |
| nova-ctrl | 1 | offline | unknown — node offline |

### Guests

| Guest | Node | Type | Status | vCPU | RAM | Disk | Network |
|---|---|---|---|---|---|---|---|
| `immich` (101) | nova-04 | qemu (Linux) | running | 4 | 8 GB | 20 GB on `local` | vmbr0, firewall on |
| `tailscale` (102) | nova-04 | lxc (Ubuntu, unprivileged) | running | 1 | 512 MB | 4 GB on `local` | vmbr0, DHCP, firewall on |
| `navidrome` (103) | nova-04 | lxc (Ubuntu, unprivileged) | stopped | 1 | 1 GB | 8 GB on `local` | vmbr0, static IP (masked), firewall on |
| `hermes` (111) | nova-03 | qemu (Linux) | running | 4 | 8 GB | 32 GB on `local` | vmbr0, firewall on |

All four guest disks live on the `local` directory storage, not on the ZFS
pools. There are no Dokploy or Gitea guests anywhere in the cluster.

### Storage IDs (identical on nova-01..04)

| ID | Type | Content |
|---|---|---|
| `local` | dir | iso, images, vztmpl, backup, rootdir |
| `nova-orbit` | zfspool | images, rootdir |
| `nova-vault` | zfspool | images, rootdir |

Pool-to-purpose mapping (which pool backs k3s vs. services) is still
`TODO(T4)`. Storage on nova-ctrl is unverifiable while it is offline.

### HA

No HA groups exist. HA-managed resources: `ct:102` and `vm:111` (state
started). `immich` (101) and `navidrome` (103) are not HA-managed.

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

# Same inventory via the API (read-only token, no SSH needed)
curl -sk -H "Authorization: PVEAPIToken=$PVE_TOKEN" \
  "$PVE_URL/api2/json/cluster/resources?type=vm" | jq
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
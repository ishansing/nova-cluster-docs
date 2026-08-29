# Runbook — Add a Node

Add a new Proxmox node to the nova cluster and (optionally) join it to k3s.

## Pre-checks

1. Physical install: new machine booted into Proxmox VE (same release as the cluster —
   `TODO(T1)`), with networking configured (`TODO(T8)` VLAN).
2. Time sync correct on the new node (cluster requires it).
3. Node name unique and resolvable from existing nodes.
4. Credentials: root password for the new node, and `nova-ssh` access to an existing
   cluster member.

## Steps

```bash
# 1. On the new node, join the Proxmox cluster
#    (replace <existing-node> and 192.0.2.x with a real cluster member/address)
pvecm add <existing-node>

# 2. On any existing node, verify
pvecm status            # new node now listed, cluster still quorate
```

If the node should run k3s (see `TODO(T2)` for the expected layout):

```bash
# 3. Join k3s as an agent (server URL and token from the control plane, TODO(T2))
curl -sfL https://get.k3s.io | K3S_URL=https://<server>:6443 \
  K3S_TOKEN=<token> sh -

# 4. Verify
kubectl get nodes -o wide
```

If the node adds ZFS storage (1 TB HDD):

```bash
# 5. Create the pool per the mirror/RAIDZ design (TODO(T5)), then register it in Proxmox
zpool create <pool> <vdev...>
# In the Proxmox UI: Datacenter -> Storage -> Add -> ZFS
```

## Rollback

- Remove from k3s first: `kubectl delete node <name>`, then on the node
  `systemctl disable --now k3s-agent`.
- Leave the cluster: on the node `pvecm delnode <name>` (run from another node) and
  wipe the corosync config.
- Confirm quorum unaffected: `pvecm status`.

## Post-add

- Update `inventory/servers.md`, the pool table (`storage/zfs-and-pools.md`), and
  `CHANGELOG.md`.

Related: [servers.md](../../inventory/servers.md) · [k3s.md](../../platforms/k3s.md)
# Platform — k3s

k3s runs on **nova-01, nova-02, nova-03**. The exact control-plane layout (single server
vs. HA control plane, and which node is the server) is `TODO(T2)`.

## Node layout

| Node | k3s role | Status |
|---|---|---|
| nova-01 | Server or agent — `TODO(T2)` | Open |
| nova-02 | Server or agent — `TODO(T2)` | Open |
| nova-03 | Server or agent — `TODO(T2)` | Open |
| nova-04 | Not a k3s node | — |

## Storage model

- k3s workloads use a dedicated ZFS pool (the **k3s-storage** pool). Pool ownership and
  which node(s) back it: `TODO(T4)`.
- Self-hosted services use a separate **services** pool, also `TODO(T4)`. See
  [zfs-and-pools.md](../storage/zfs-and-pools.md).

## Key commands

Run against the k3s cluster (the kubeconfig lives on the control-plane node —
`TODO(T2)`):

```bash
# Nodes and roles
kubectl get nodes -o wide

# Workloads across all namespaces
kubectl get pods -A

# Storage classes available to workloads
kubectl get sc

# Persistent volumes
kubectl get pv,pvc -A
```

## Workload placement

Workloads are expected to land on nova-01..03 (they are the k3s members). Node labels /
taints are not yet documented (`TODO(T2)` will pin down roles).

## Upgrades

Follow [ops/runbooks/k3s-upgrade.md](../ops/runbooks/k3s-upgrade.md). Always snapshot
etcd before a control-plane upgrade.

Related: [proxmox.md](proxmox.md) · [services/index.md](../services/index.md) ·
[README](../README.md)
# Runbook — k3s Upgrade

Upgrade the k3s cluster. Control-plane layout is `TODO(T2)`; always snapshot etcd before
touching the server node.

## Pre-checks

1. Layout: `kubectl get nodes -o wide` → confirm server vs. agents (`TODO(T2)`).
2. Health: `kubectl get pods -A` → no stuck/non-Running pods that matter.
3. Record current versions: `kubectl version` and `k3s --version` on the server.

## Steps

```bash
# 1. Snapshot etcd (run on the server node, TODO(T2))
k3s etcd-snapshot save

# 2. Upgrade the server first
curl -sfL https://get.k3s.io | sh -s - --version <target-version>
systemctl restart k3s

# 3. Verify the server is healthy
kubectl get nodes
kubectl get pods -A

# 4. For each agent, drain then upgrade
kubectl drain <node> --ignore-daemonsets --delete-emptydir-data
curl -sfL https://get.k3s.io | sh -s - --version <target-version>
systemctl restart k3s-agent
kubectl uncordon <node>
```

## Rollback

- Reinstall the previous `k3s` version on the server and agents.
- If the control plane is broken, restore etcd from the snapshot saved in step 1
  (`k3s server --cluster-reset` flow).

## Post-upgrade

- `kubectl get nodes` → all Ready with the target version.
- Spot-check one workload end-to-end.
- Record versions and the layout (`TODO(T2)`) in `CHANGELOG.md`.

Related: [k3s.md](../../platforms/k3s.md) · [backup-and-dr.md](../../backup-and-dr.md)
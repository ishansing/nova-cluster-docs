# Runbook — Proxmox VE Upgrade

Upgrade all nodes to the same release. Version currently in use: `TODO(T1)`.

## Pre-checks

1. Confirm cluster is healthy: `pvecm status` → `Quorate` on all nodes.
2. Note the current version: `pveversion -v` (`TODO(T1)`).
3. Review the release notes for the target version.
4. Confirm no migrations/backup jobs (`vzdump`) are running:
   `pvesh get /cluster/tasks --typefilter vzdump`.
5. Ensure backups/snapshots exist for VMs/LXCs on the node being upgraded
   ([backup-and-dr.md](../../backup-and-dr.md)).

## Steps (one node at a time)

```bash
# On the node being upgraded
apt update
apt list --upgradable        # review what will change
apt dist-upgrade
pveam update                 # update appliance templates
reboot
```

After reboot, verify:

```bash
pveversion                   # version TODO(T1)
pvecm status                 # node re-joined, quorate
pvesh get /cluster/resources --type vm   # VMs/LXCs present
```

Repeat on the next node. Never upgrade all nodes before confirming each rejoins.

## Rollback

- Proxmox keeps previous kernels; if the new boot fails, pick the previous kernel in the
  GRUB menu (`Advanced options for Proxmox VE`).
- Downgrade packages only as a last resort: `apt install proxmox-ve=<version>` and
  confirm the previous kernel still boots.

## Post-upgrade

- Record the new version in `TODO-REGISTER.md` (resolves `T1`) and `CHANGELOG.md`.

Related: [proxmox.md](../../platforms/proxmox.md) · [CHANGELOG.md](../../CHANGELOG.md)
# CHANGELOG

Chronological record of infrastructure changes to the **nova** homelab. Newest first.

## 2026-09-27 — Docs rewritten to observed Proxmox state

- Old placement claims dropped; the API is now the source of truth (see
  `platforms/proxmox.md` § Observed state).
- Guest specs recorded: immich (4 vCPU/8 GB/20 GB), hermes (4 vCPU/8 GB/32 GB),
  tailscale (1 vCPU/512 MB/4 GB), navidrome (1 vCPU/1 GB/8 GB, stopped); all
  disks on `local` storage.
- Storage IDs observed on every online node: `local` (dir), `nova-orbit` and
  `nova-vault` (zfspool) — `TODO(T4)` narrowed to purpose mapping.
- HA: no groups; HA-managed guests are `ct:102` and `vm:111` only.
- **Removed:** `services/dokploy.md`, `services/gitea.md` — no such guests exist
  in the cluster. Re-add if re-provisioned.

## 2026-09-27 — Proxmox API reconciliation (read-only)

- Queried the cluster via the Proxmox API (`cluster/status`, `nodes`,
  `cluster/resources?type=vm`): cluster `Nova`, 5 members, quorate; nova-01..04
  online, **nova-ctrl offline**.
- Observed guests: nova-04 → qemu/101 `immich` (running), lxc/102 `tailscale`
  (running), lxc/103 `navidrome` (stopped); nova-03 → qemu/111 `hermes`
  (running); nova-01/02 → none.
- **Discrepancies for the owner:** `hermes` is on nova-03 (docs said nova-04);
  Dokploy/Gitea absent from the snapshot (kept in catalog, verify);
  `tailscale` LXC on nova-04 vs. two `TODO(T3)` LXCs attributed to nova-ctrl.
- No TODOs resolved; no infrastructure or permission changes made.

## 2026-08-29 — Documentation project started

- Created this documentation set per `SPEC.md` (structure: README, inventory, network,
  platforms, storage, services, security, backup-and-dr, runbooks, changelog, register).
- Created `TODO-REGISTER.md` mirroring `CONTEXT.md` § 8; all 16 items open.
- **Resolved:** `T5` (ZFS pools are mirror/RAIDZ, not single-disk) and `T6` (OS on
  SSD, bulk data on HDD ZFS) — confirmed by the owner on 2026-08-29.
- **Clarified:** `T1` — all nodes run the same Proxmox VE release (version number still
  open).
- Confirmed service catalog: Immich (VM, exposed), Dokploy (VM, exposed), Gitea (VM),
  Navidrome (LXC), Hermes agent (VM) on nova-04; 2 VPN LXCs on nova-ctrl (`TODO(T3)`);
  k3s on nova-01..03 (`TODO(T2)`).

No infrastructure changes recorded yet — this log will be updated as the homelab
evolves (upgrades, node adds, incidents, service changes).
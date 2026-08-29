# CHANGELOG

Chronological record of infrastructure changes to the **nova** homelab. Newest first.

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
# TODO-REGISTER.md — Open Questions Tracker

Mirror of `CONTEXT.md` § 8. This is the single source of truth for open questions.
Update the **Status** column here and in `CONTEXT.md` when answers come in. Never
re-ask an item marked `Resolved`.

Docs reference these IDs as `TODO(T#)` rather than restating the question.

| ID | Topic | Question | Status |
|---|---|---|---|
| T1 | Proxmox | What Proxmox VE version runs on each node? (All nodes confirmed on the same release; version number TBD.) | Open |
| T2 | k3s | Which node is the k3s server (control plane) vs. pure worker? | Open |
| T3 | VPN LXCs | What are the two VPN LXCs on nova-ctrl (names, stacks, roles)? | Open |
| T4 | ZFS | Which node(s) own the k3s-storage pool vs. the services pool? | Partial — storage IDs observed 2026-09-27: `local` (dir) + `nova-orbit` / `nova-vault` (zfspool) on every online node; all guest disks live on `local`; purpose mapping still open |
| T5 | ZFS | Are pools single-disk, or mirror/RAIDZ? | Resolved — mirror/RAIDZ (not single-disk); exact mirror-vs-RAIDZ topology folded into T4 |
| T6 | ZFS | How are VM disks split across SSD vs. HDD? | Resolved — OS/boot on 256 GB SSD, bulk data on 1 TB ZFS HDD |
| T7 | Network | Main subnets and VLAN IDs? | Open |
| T8 | Network | Which VLAN hosts the nova nodes? | Open |
| T9 | Network | Local DNS / ad-blocking setup, if any? | Open |
| T10 | Exposure | Which services are internet-facing vs. VPN-only vs. LAN-only? | Open |
| T11 | Services | URLs/domains for each service? | Open |
| T12 | Services | Data location per service (ZFS dataset, VM disk, NFS share)? | Open |
| T13 | Services | Backup strategy per service? | Open |
| T14 | Security | Auth model for SSH and web services (keys, passwords, SSO, MFA)? | Open |
| T15 | Ops | Recurring maintenance tasks worth turning into runbooks? | Open |
| T16 | Ops | Any past outages worth writing up as incident reports? | Open |
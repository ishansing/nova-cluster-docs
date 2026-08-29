# CONTEXT.md — nova Homelab: Background & Working Knowledge

## Purpose

This file gives an AI documentation agent **background knowledge** about the `nova`
homelab cluster, plus guidance on what to clarify with the owner before generating docs.

Treat this as project context, not a complete source of truth — confirm anything marked
`TODO` before writing it into published documentation.

Companion file: `SPEC.md` defines what the agent should produce and how it should behave.
Read both files at the start of every session.

## Document status

| Field | Value |
|---|---|
| Maintained by | Ishan Singh |
| Last updated | 2026-08-29 |
| Status | Living document — update as facts are confirmed |

---

## 1. Owner

- **Name:** Ishan Singh
- **Location:** Indore, Madhya Pradesh, India
- **Profile:** Engineering student and individual developer. Strong focus on Linux,
  cloud/Kubernetes, AI agents, cybersecurity, and self-hosting.

## 2. Homelab goals

This homelab should be:

- A realistic mini-production environment on **Proxmox + k3s + ZFS**.
- A stable platform for self-hosted services (media, photos, Git, AI agents, VPN) and for
  experimenting with agents, automation, and infra-as-code.
- A **portfolio project** to show Indian and international companies for infra/SRE/DevOps roles.

## 3. Physical cluster overview

Cluster name: **nova**

| Node | Role | CPU | RAM | Storage | OS |
|---|---|---|---|---|---|
| nova-ctrl | Infra management / control-plane utilities | Intel i7 (older gen) | 8 GB DDR3 | 1 TB HDD | Proxmox VE (version `TODO`) |
| nova-01 | k3s node (server or worker — `TODO`) | Intel i5, 8th gen | 16 GB DDR4 | 256 GB SSD + 1 TB HDD (ZFS) | Proxmox VE |
| nova-02 | k3s node (server or worker — `TODO`) | Intel i5, 8th gen | 16 GB DDR4 | 256 GB SSD + 1 TB HDD (ZFS) | Proxmox VE |
| nova-03 | k3s node (server or worker — `TODO`) | Intel i5, 8th gen | 16 GB DDR4 | 256 GB SSD + 1 TB HDD (ZFS) | Proxmox VE |
| nova-04 | Hosts most application VMs/LXCs | Intel i5, 8th gen | 16 GB DDR4 | 256 GB SSD + 1 TB HDD (ZFS) | Proxmox VE |

Cluster state:

- All 5 nodes are joined into a single Proxmox cluster with HA enabled.
- **k3s** runs on nova-01, nova-02, nova-03 — exact control-plane layout is `TODO`.
- **nova-ctrl** runs two VPN LXCs (names/stacks `TODO`).
- **nova-04** currently hosts most application VMs and LXCs.

## 4. Storage overview

- Each worker node (nova-01..04) has a 1 TB HDD dedicated to ZFS.
- Two logical storage areas exist:
  - A ZFS pool for **k3s workloads** (cluster storage).
  - A ZFS pool for **self-hosted services** (VMs/LXCs).
- Open questions (see TODO register below): pool-to-node ownership, single-disk vs.
  RAIDZ/mirror, and how VM disks split between SSD and HDD.

## 5. Network & VPN

- Main router → 8-port switch.
- VLANs are used for segmentation; exact IDs and purposes are not yet documented.
- Remote access via **NetBird** and **Tailscale** overlays.

## 6. Services & self-hosted apps

| Service | Host | Type | Purpose | External exposure |
|---|---|---|---|---|
| VPN LXC (×2) | nova-ctrl | LXC | Remote access / tunneling | `TODO` |
| Navidrome | nova-04 | LXC | Music streaming | `TODO` |
| Immich | nova-04 | VM | Photo management | Yes (exposed) |
| Dokploy | nova-04 | VM | App hosting / deployment | Yes (exposed) |
| Gitea | nova-04 | VM | Self-hosted Git | `TODO` |
| Hermes agent | nova-04 | VM | AI agent stack | `TODO` |

## 7. Documentation preferences

- **Layered docs:** hardware → network → cluster → storage → services → security → operations.
- **SRE-style runbooks:** pre-checks, commands, rollback notes.
- **Portfolio-ready structure:** a reader should immediately see this is a serious homelab,
  not a pile of random VMs.
- Comfortable with terminal tools and Markdown.
- Fine publishing non-sensitive parts publicly (architecture, service list, diagrams).
- Secrets, exact IPs, and passwords must never appear in the docs — see `SPEC.md` §
  Redaction convention for the exact rules.

## 8. Known unknowns — TODO register

Single source of truth for open questions. The agent should update the **Status** column
as answers come in, and must not re-ask anything already marked `Resolved`. Generated docs
should reference these IDs (e.g. `TODO(T7)`) rather than restating the question in prose.

| ID | Topic | Question | Status |
|---|---|---|---|
| T1 | Proxmox | What Proxmox VE version runs on each node? (All nodes confirmed on the same release; version number TBD.) | Open |
| T2 | k3s | Which node is the k3s server (control plane) vs. pure worker? | Open |
| T3 | VPN LXCs | What are the two VPN LXCs on nova-ctrl (names, stacks, roles)? | Open |
| T4 | ZFS | Which node(s) own the k3s-storage pool vs. the services pool? | Open |
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

## 9. Relationship to SPEC.md

- `SPEC.md` = **what** to produce and **how** to behave.
- `CONTEXT.md` = **what** the nova cluster currently looks like and **where** more detail
  is needed.

Read both files at the start of every session. When a `TODO` in the register above is
resolved, update this file — not just the generated docs — so future sessions inherit
the answer instead of re-asking it.

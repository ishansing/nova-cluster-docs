# nova — Homelab Documentation

The **nova** homelab is a 5-node Proxmox VE cluster running k3s, ZFS, and a set of
self-hosted services (photos, music, Git, app hosting, AI agents). It is a
mini-production environment used both as a stable platform for experimentation and as a
portfolio project for infra/SRE/DevOps roles.

## What it runs

| Layer | Technology | Nodes |
|---|---|---|
| Management / control plane | Proxmox VE cluster with HA | nova-ctrl + all nodes |
| Remote access | 2× VPN LXC, NetBird + Tailscale overlays | nova-ctrl |
| Orchestration | k3s | nova-01, nova-02, nova-03 |
| Workloads | Application VMs/LXCs | nova-04 (+ Hermes agent VM on nova-03) |
| Storage | `local` dir + `nova-orbit` / `nova-vault` ZFS pools | nova-01..04 |

Services running: [Immich](services/immich.md), [Navidrome](services/navidrome.md),
[Hermes agent](services/hermes-agent.md), and [Tailscale](services/vpn.md)
— the full guest list per the Proxmox API (see
[services/index.md](services/index.md)).

## Documentation map

| Topic | Files |
|---|---|
| Inventory | [servers.md](inventory/servers.md) · [network-gear.md](inventory/network-gear.md) |
| Network | [topology.md](network/topology.md) · [vlans-subnets.md](network/vlans-subnets.md) |
| Platforms | [proxmox.md](platforms/proxmox.md) · [k3s.md](platforms/k3s.md) |
| Storage | [zfs-and-pools.md](storage/zfs-and-pools.md) |
| Services | [index.md](services/index.md) + per-service pages |
| Security | [model.md](security/model.md) |
| Backups | [backup-and-dr.md](backup-and-dr.md) |
| Operations | [runbooks](ops/runbooks/) (proxmox-upgrade · k3s-upgrade · add-node) |
| History | [CHANGELOG.md](CHANGELOG.md) · [TODO-REGISTER.md](TODO-REGISTER.md) |

## How to read this

Read top-down for architecture; jump straight to a runbook during an outage — every
page is written to stand alone. Anything not yet confirmed is tagged `TODO(T#)`,
referencing [TODO-REGISTER.md](TODO-REGISTER.md). No real secrets, passwords, or public
IPs appear anywhere in these docs.

## Open questions that matter most right now

- k3s control-plane layout — `TODO(T2)`
- VLAN/subnet scheme — `TODO(T7)`, `TODO(T8)`
- Service exposure and URLs — `TODO(T10)`, `TODO(T11)`

Full list: [TODO-REGISTER.md](TODO-REGISTER.md).

## Maintained by

Ishan Singh · living document, updated from `CONTEXT.md` and `TODO-REGISTER.md`.
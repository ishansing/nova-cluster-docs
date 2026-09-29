# Service — Hermes Agent

AI-agent stack: hosts agents / agent tooling on the homelab.

## Facts

| Field | Value |
|---|---|
| Host | nova-03 — qemu/111 (running, HA-managed) |
| vCPU / RAM | 4 / 8 GB |
| Disk | 32 GB on `local` storage (`local:111/vm-111-disk-0.qcow2`) |
| Network | vmbr0, firewall on |
| Purpose | AI agent stack (agents, tools, automations) |
| Exposure | `TODO(T10)` |
| URL | `TODO(T11)` |
| Data location | `TODO(T12)` |
| Backup | `TODO(T13)` |

## Dependencies

- Agent runtimes and tooling on the nova-03 VM; persistent state location is
  `TODO(T12)`.
- Likely needs outbound internet for LLM APIs; inbound access should stay
  VPN/LAN-only unless explicitly exposed (`TODO(T10)`).

## Operations notes

- Agents can consume significant CPU/RAM — keep an eye on nova-03 resource usage.
- Any API keys for LLM providers live in the agent's secret store / password manager,
  never in these docs (redaction per [SPEC](../SPEC.md) § Redaction convention).

Related: [index.md](index.md) · [security/model.md](../security/model.md) ·
[README](../README.md)
# Service — Hermes Agent

AI-agent stack: hosts agents / agent tooling on the homelab.

## Facts

| Field | Value |
|---|---|
| Host | nova-04 (VM) |
| Purpose | AI agent stack (agents, tools, automations) |
| Exposure | `TODO(T10)` |
| URL | `TODO(T11)` |
| Data location | `TODO(T12)` |
| Backup | `TODO(T13)` |

## Dependencies

- Agent runtimes and tooling on the nova-04 VM; state/data on the services ZFS pool
  (`TODO(T4)`/`TODO(T12)`).
- Likely needs outbound internet for LLM APIs; inbound access should stay
  VPN/LAN-only unless explicitly exposed (`TODO(T10)`).

## Operations notes

- Agents can consume significant CPU/RAM — keep an eye on nova-04 resource usage.
- Any API keys for LLM providers live in the agent's secret store / password manager,
  never in these docs (redaction per [SPEC](../SPEC.md) § Redaction convention).

Related: [index.md](index.md) · [security/model.md](../security/model.md) ·
[README](../README.md)
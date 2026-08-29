# Service — Dokploy

Self-hosted PaaS for deploying and managing applications and Docker containers.

## Facts

| Field | Value |
|---|---|
| Host | nova-04 (VM) |
| Purpose | App hosting / deployment (Docker, VPS-style deploys) |
| Exposure | Internet-facing (confirmed) |
| URL | `TODO(T11)` |
| Data location | `TODO(T12)` |
| Backup | `TODO(T13)` |

## Dependencies

- Hosts the app-deploy environment; its projects' data may live on the services ZFS
  pool (`TODO(T4)`/`TODO(T12)`).
- Uses Docker on the nova-04 VM. Its deployment networks should be segmented per
  [security/model.md](../security/model.md).

## Operations notes

- Applications deployed via Dokploy inherit Dokploy's VM resources — watch disk usage on
  nova-04.
- Credentials for deployed apps must live in Dokploy's secrets store, not in docs —
  redaction per [SPEC](../SPEC.md) § Redaction convention.

Related: [index.md](index.md) · [README](../README.md)
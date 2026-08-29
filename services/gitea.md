# Service — Gitea

Self-hosted Git hosting (repositories, issues, CI via Actions where enabled).

## Facts

| Field | Value |
|---|---|
| Host | nova-04 (VM) |
| Purpose | Self-hosted Git for projects and experiments |
| Exposure | `TODO(T10)` |
| URL | `TODO(T11)` |
| Data location | `TODO(T12)` |
| Backup | `TODO(T13)` |

## Dependencies

- Repository data typically on the services ZFS pool (`TODO(T4)`/`TODO(T12)`).
- If exposed publicly, pair with SSH keys + HTTPS (`TODO(T14)`).

## Operations notes

- Git data is irreplaceable user data — prioritise `TODO(T13)` (backup) for the
  repositories: ZFS snapshot + `gitea dump`.
- Health check: `curl -I` the Gitea URL (`TODO(T11)`), or browse the repo list.

Related: [index.md](index.md) · [backup-and-dr.md](../backup-and-dr.md) ·
[README](../README.md)
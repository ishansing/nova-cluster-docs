# Security — Threat Model & Access

Home environment behind a consumer-grade router with a dynamic public IP. The security
model favours small attack surface and least privilege over enterprise tooling.

## Threat model

Threats, in rough priority order for a home cluster:

1. **Internet-facing services compromised** (Immich, Dokploy are exposed). Each exposed
   service is a path into the cluster.
2. **Exposed management plane** — Proxmox/k3s/SSH reachable from the internet is a
   critical finding.
3. **Lost/stolen device** carrying credentials.
4. **Insider/guest** on the LAN — untrusted devices reaching management.

## Segmentation

- VLANs exist to separate management, cluster, and workload traffic — concrete IDs are
  `TODO(T7)`/`TODO(T8)`.
- Internet-facing services (Immich, Dokploy) should be isolated from the management
  network; exact exposure per service is `TODO(T10)`.

## Access model

| Surface | Method | Status |
|---|---|---|
| SSH to nodes | Key-based, `nova-ssh` entry in password manager | `TODO(T14)` |
| Web services | Auth model (SSO / password / MFA) | `TODO(T14)` |
| Remote access | NetBird + Tailscale overlays via nova-ctrl VPN LXCs | Confirmed in use |

## Hardening basics

- No public exposure for Proxmox, k3s API, or SSH — remote admin goes through the VPN
  overlays.
- Rotate service passwords and review exposed-service auth `TODO(T14)`.
- Keep nodes patched (see [proxmox-upgrade](../ops/runbooks/proxmox-upgrade.md) and
  [k3s-upgrade](../ops/runbooks/k3s-upgrade.md)).

## Redaction guarantee

These docs never contain secrets, keys, tokens, passwords, or the public IP. Secrets
are referenced by password-manager entry names only (e.g. `nova-ssh`) per
[SPEC](../SPEC.md) § Redaction convention.

Related: [vlans-subnets.md](../network/vlans-subnets.md) ·
[services/index.md](../services/index.md) · [README](../README.md)
# Security Policy

NEXORA is a local-first Windows utility: it touches your game launchers,
system sensors, and (optionally) records your screen. It takes that trust
seriously.

## Reporting a vulnerability

**Please use [private vulnerability
reporting](https://github.com/gazahyp/NEXORA/security/advisories/new)** —
do not open a public issue.

We aim to:

- **Acknowledge** every report within **72 hours**
- **Ship a patch release** for confirmed issues within **14 days**
- **Credit you** in the release notes (if you want it)

## Our security stance (summary)

| Area | Design |
| --- | --- |
| Privilege | Runs with your user rights; anything needing more asks UAC explicitly. Nothing elevates silently. |
| Sensors | Read-only (WMI/ETW/counters/SMART). NEXORA never writes fan curves or embedded-controller settings. |
| Booster/optimizer | Every system mutation records the prior value and is reversible; process kills limited to a curated list with a protected-name denylist (AV, drivers, csrss/wininit, NEXORA itself). |
| Execution | External processes spawn with explicit argv — never shell strings — so metacharacters can't inject commands. |
| Credentials | Passwords: Argon2id + random salt, never plaintext, never logged. Session tokens: Windows DPAPI-wrapped locally; the DB stores only a SHA-256 hash. |
| Input | All IPC args validated in Rust: charset allowlists, canonicalized paths confined to owned roots, clamped numerics, https-only URLs, 100% parameterized SQL. |
| Downloads | Installer URLs checked against a structural host allowlist (official launcher CDNs only); filename sanitized; single-flight HTTPS. |
| CSP | `default-src 'self'`, `script-src 'self'` (no unsafe-inline/eval), `object-src 'none'`, `form-action 'none'`. |

## Update integrity

Every release installer is signed with a minisign (Ed25519) key; the public
key ships embedded in the app. The updater **refuses** any payload whose
signature does not verify — artifacts hosted in this repo cannot be swapped
or tampered with in transit. SHA-256 checksums accompany every release.

## Known accepted limitations

- Builds are not yet Authenticode-signed (personal project). SmartScreen may
  warn “Unknown publisher”; verify the release checksum, then proceed.
- The optional in-app cloud features are opt-in and off by default — see
  [Privacy Policy](docs/PRIVACY-POLICY.md).

## Scope

This policy covers the shipped desktop application and this repository's
release artifacts. It does not cover user-created configs or third-party
launchers NEXORA integrates with.

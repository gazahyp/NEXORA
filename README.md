<div align="center">

<pre>
 ███╗   ██╗███████╗██╗  ██╗███████╗ ██████╗  █████╗
 ████╗  ██║██╔════╝╚██╗██╔╝██╔════╝██╔═══██╗██╔══██╗
 ██╔██╗ ██║█████╗   ╚███╔╝ █████╗  ██║   ██║███████║
 ██║╚██╗██║██╔══╝   ██╔██╗ ██╔══╝  ██║   ██║██╔══██║
 ██║ ╚████║███████╗██╔╝ ██╗███████╗╚██████╔╝██║  ██║
 ╚═╝  ╚═══╝╚══════╝╚═╝  ╚═╝╚══════╝ ╚═════╝ ╚═╝  ╚═╝
</pre>

**Update infrastructure for NEXORA — the Windows gaming control hub**

[![Latest release](https://img.shields.io/github/v/release/gazahyp/nexora-updates?label=latest&color=8b5cf6)](https://github.com/gazahyp/nexora-updates/releases/latest)
[![Updater endpoint](https://img.shields.io/badge/updater-latest.json-16a34a)](https://github.com/gazahyp/nexora-updates/releases/latest/download/latest.json)
[![Download page](https://img.shields.io/badge/download-page-2563eb)](https://gazahyp.github.io/nexora-updates/)
[![Platform](https://img.shields.io/badge/platform-Windows%20x64-0078d4)](#supported-platforms)

</div>

---

## What is this repository?

`nexora-updates` is the **release and auto-update host** for
**NEXORA** — a Windows gaming control hub built with Tauri 2 + Rust + React.
It exists so every installed NEXORA app has exactly one trusted place to
check for, verify, and download new versions.

It contains **no application source code** — only:

| Path | Role |
| --- | --- |
| [`latest.json`](https://github.com/gazahyp/nexora-updates/releases/latest/download/latest.json) | Tauri updater manifest served to all installed clients |
| [`index.html`](https://gazahyp.github.io/nexora-updates/) | GitHub Pages download landing page |
| [`latest.json`](latest.json) *(in-tree copy)* | Same-origin manifest mirror used by the landing page |
| [Releases](../../releases) | Signed installers, portable archives, checksums |

> [!IMPORTANT]
> Files in this repository are **machine-generated**. Do not edit them by
> hand — everything is published by `npm run publish:update` in the NEXORA
> source repository (see [Release pipeline](#release-pipeline)).

## Endpoints

```text
Updater manifest (clients poll this)
  https://github.com/gazahyp/nexora-updates/releases/latest/download/latest.json

Download landing page (GitHub Pages)
  https://gazahyp.github.io/nexora-updates/

Latest release assets
  https://github.com/gazahyp/nexora-updates/releases/latest
```

## Supported platforms

| Target key | OS / arch | Artifact |
| --- | --- | --- |
| `windows-x86_64` | Windows 10 / 11 x64 | NSIS setup (`.exe`), portable (`.zip`), MSI |

## Manifest schema

`latest.json` follows the static multi-platform format expected by
[`tauri-plugin-updater`](https://v2.tauri.app/plugin/updater/) v2:

```jsonc
{
  "version": "1.0.30",                  // semver — clients update when this is newer
  "notes": "NEXORA 1.0.30",             // shown in the in-app update dialog
  "pub_date": "2026-09-13T21:59:22.963Z",
  "platforms": {
    "windows-x86_64": {
      "url": "https://github.com/gazahyp/nexora-updates/releases/download/v1.0.30/NEXORA-Setup-1.0.30.exe",
      "signature": "dW50cnVzdGVk..."   // base64 minisign signature of the .exe bytes
    }
  }
}
```

## Release pipeline

Every publish runs through `scripts/publish-update.mjs` in the NEXORA repo,
which is **atomic and self-verifying**:

```text
npm run release            npm run publish:update
─────────────────         ─────────────────────────────────────────
Tauri build (NSIS+MSI) →  1. locate signed bundle (installer + .sig)
minisign signature        2. build latest.json
                          3. VERIFY signature against the exact bytes
                             (Ed25519 over BLAKE2b-512, same scheme as
                              the client) — abort on mismatch
                          4. upload NEXORA-Setup-<v>.exe + latest.json
                             as GitHub Release v<v>, marked "latest"
                          5. sync the Pages latest.json mirror
```

Guarantees:

- **Idempotent** — republishing the same version deletes and recreates the
  tag, so a retried release can never leave duplicates or drift.
- **Fail-closed** — a build whose signature does not verify against the
  configured pubkey is never uploaded; installed apps can not be poisoned by
  a broken publish.
- **Version-pinned assets** — every release carries its own copy of the
  installer (`NEXORA-Setup-<version>.exe`), so old clients can always fetch
  the exact bytes their manifest points to.

## Security model

- Installers are signed with a **minisign (Ed25519) key**; the public key
  ships embedded in every NEXORA build (`plugins.updater.pubkey`).
- The updater **rejects any manifest entry whose signature does not match**
  the downloaded artifact — artifacts served from this repo can not be
  swapped or tampered with in transit.
- Release bodies include **SHA-256 checksums** for manual verification:
  `Get-FileHash .\NEXORA-Setup-<version>.exe -Algorithm SHA256`
- Builds are not Authenticode-signed (personal project). First run may show
  a SmartScreen “Unknown publisher” warning; the checksum above confirms the
  file is the one published here.

## Manual operations

Only needed when the automated pipeline misbehaves:

```bash
# Promote an existing release to "latest" (what /releases/latest/ resolves to)
gh release edit v1.0.30 --latest -R gazahyp/nexora-updates

# Re-point the Pages mirror at a specific release's manifest
gh release download v1.0.30 -p latest.json -D . -R gazahyp/nexora-updates --clobber
git commit -am "chore(pages): point latest.json at v1.0.30" && git push

# Remove a bad release (prevents clients from resolving it)
gh release delete v1.0.30 --yes -R gazahyp/nexora-updates
```

## License

Release artifacts are distributed under the NEXORA end-user license. This
repository's tooling and landing page: © Gaza Ibrahim, all rights reserved
unless otherwise noted.

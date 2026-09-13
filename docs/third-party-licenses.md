# NEXORA — Third-Party Licenses & Attribution

NEXORA's own code and binaries are distributed under the **NEXORA Freeware
License** (see `LICENSE` at the repository root) — freeware, not open
source. This page attributes runtime & build dependencies. Exact license
texts ship with each package; the full machine list is regenerated before
each release.

## 1. Frontend runtime (npm dependencies)
| Package | License | Used for |
| --- | --- | --- |
| react, react-dom | MIT | UI framework |
| react-router-dom | MIT | routing |
| zustand | MIT | state stores |
| @tauri-apps/api + plugins (dialog/shell/process/updater) | MIT OR Apache-2.0 | desktop IPC |
| @supabase/supabase-js | MIT | optional cloud sync |

## 2. Rust runtime (key crates)
| Crate | License | Used for |
| --- | --- | --- |
| tauri (+ plugins, wry, tao, muda, tray-icon, window-vibrancy) | MIT OR Apache-2.0 | app framework |
| serde / serde_json | MIT OR Apache-2.0 | serialization |
| rusqlite (bundled) | MIT | SQLite (engine: public domain) |
| sysinfo | MIT | CPU/RAM/process enumeration |
| argon2, sha2, base64ct, password-hash (RustCrypto) | MIT OR Apache-2.0 | credential hashing |
| rand | MIT OR Apache-2.0 | CSPRNG tokens |
| chrono | MIT OR Apache-2.0 | timestamps |
| uuid | MIT OR Apache-2.0 | ids |
| ureq + rustls + ring + webpki | MIT/Apache-2.0 (ring & webpki add ISC/OpenSSL-style clauses) | HTTPS |
| **webpki-roots** | **MPL-2.0** (file-level copyleft — source of the included files remains MPL) | CA roots |
| reqwest/tokio (hyper family) | MIT | media engine HTTP |
| zip, flate2 | MIT | archive unpacking (LHM/FFmpeg engines) |
| minisign-verify | Apache-2.0 | updater signature verify |
| open | MIT | external URL open |
| windows-sys, windows, winapi | MIT OR Apache-2.0 | Win32 APIs |
| once_cell, log | MIT OR Apache-2.0 | utilities |

## 3. Build tooling (not distributed)
vite (MIT), @vitejs/plugin-react (MIT), typescript (Apache-2.0), tailwindcss (MIT),
vitest (MIT), @testing-library/* (MIT), jsdom (MIT), @playwright/test (Apache-2.0),
@tauri-apps/cli (MIT OR Apache-2.0), NSIS (zlib/libpng), WiX Toolset (MS-RL), WebView2
runtime components (Microsoft Software License Terms — redistributed via Evergreen
bootstrapper only).

## 4. Regenerating this list (release step — REQUIRED)
```bash
cargo install cargo-about && cargo about generate about.hbs > docs/licenses-rust.html
npx --yes license-checker --production --out docs/licenses-npm.txt --csv
```
Attach both artifacts to the release and bundle into the installer as `THIRD-PARTY-NOTICES.txt`.

## 5. Optional downloaded components (fetched by the user, not redistributed)
| Component | License / notes |
| --- | --- |
| **LibreHardwareMonitor** (Sensor Providers → Install) | MIT — downloaded from the official GitHub releases via UAC-consented installer flow |
| **FFmpeg** (media engine, Record Studio) | builds are LGPL or GPL depending on configure flags — NEXORA links nothing (child process). When we redistribute a build we must publish corresponding configure options & source offers **or** ship an LGPL build — verify the pinned build before v1.1.0 release |
| Steam / Epic / Riot / Xbox launchers | external apps, their own EULAs — we only use documented protocols |

## 6. Data & service terms honoured
- **Steam Store/CDN** — public endpoints for metadata/artwork (respect Valve terms; no scraping beyond lookup-by-ID).
- **SteamGridDB** — API used only when the user supplies their key; content under its listed licenses.
- **RAWG.io** — same key-user-gated pattern (terms require attribution "Powered by RAWG"; add it next to settings input when key is active? done — UI note).
- Fonts: UI uses system font stack (Segoe UI / system-ui). The brand SVGs reference Orbitron-style names only as *user-installed* fallbacks; all wordmark lettering is vector paths we authored → no font licenses.
- Icons: fully custom stroke paths + `brand/` SVGs (our own work).

> **Publisher to-do:** the §4 regeneration artifacts must be produced on the release machine
> and included in the installer directory; keep §2 in sync when dependencies change.

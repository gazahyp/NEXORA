# NEXORA Privacy Policy

*Last updated: 2026-09-09 · Applies to NEXORA desktop 1.0.x+ · Language: EN (mirror ID copy required before EU release).*

NEXORA is a **local-first** gaming utility. This policy states exactly what the app does and does
not do with data. There are no ads and no sale of personal data.

## 1. Data we process

### Stored locally on your PC (never uploaded by the app)
- **Account:** email + Argon2id password hash (local sign-in), profile display name/email, avatar
  and banner image files. Passwords are never stored in plaintext and never written to browser storage.
- **Library & activity:** detected/installed games (name, launcher, install path, App ID), favorites,
  play counts, per-game play time and last-played timestamps.
- **Settings & history:** UI preferences, Game Booster session history (game, mode, start/end,
  measured FPS/RAM figures), optimization records, Record Studio clips/screenshots metadata.
- **Session token:** kept encrypted (Windows DPAPI) in your app-data profile; the database stores only
  its SHA-256 hash. Update backups exclude the token.

### Transmitted over the network — only what a feature needs, when you use it
- **Authentication:** your credentials/email to your own NEXORA backend; if you click *Continue with
  Google*, standard Google OAuth 2.0 (PKCE) exchange with Google.
- **Game launcher integration:** game names/App IDs to official endpoints to fetch metadata — Steam
  Store/CDN, SteamGridDB and/or RAWG (only if you enable/enter a key there), Epic/Riot/etc. metadata.
- **Cover artwork** downloads from those image CDNs into `appdata/artwork`.
- **Updates:** version query to our release endpoint (GitHub Releases); payload verified with a pinned
  update key. The release channel/country is the only extra signal.
- **Telemetry (optional, OFF-by-default):** aggregate crash/usage diagnostics to an HTTPS endpoint
  **only after you enable it in Settings → Privacy**. You can revoke anytime; the app then stops sending.

## 2. What we do NOT do
No third-party advertising trackers, no sale/rental/sharing of personal data, no reading of files
outside the directories the app created or that you explicitly point it at, no keylogging, no uploading
of game saves, documents, screenshots, or browsing history.

## 3. Your rights (GDPR-aligned)
Access, rectification, erasure, portability, restriction and objection. Because data is local you can
exercise most rights by deleting the app data folder or specific records in the UI. For
backend/cloud data removal, request it via the contact below; we respond within 30 days.

## 4. Retention & deletion
Local records persist until you delete them, remove a game from the library, or uninstall. Our backend
retains account rows until deletion (in-app *Delete account* or a contact request) with a grace period
for account recovery. Logs rotate on-device (≈5 MB cap) and are only transmitted if you open a support
ticket and attach them.

## 5. Minors
NEXORA is a system utility not directed at children under 13/16 (region-dependent); we do not knowingly
collect their personal data.

## 6. Changes
Material changes are announced in-app and this page is re-dated. Continued use after a change is the
basis of consent for users who enabled optional telemetry.

## 7. Controller & contact
NEXORA — controller: *[legal entity name & registered address — REQUIRED before public EU release]* ·
Data/privacy: **privacy@nexora.app** · Security: **security@nexora.app**.

> **Publisher to-do (blocker):** fill the bracketed controller fields and provide the hosted URL of this
> policy, referenced by the installer and updater metadata (see `docs/RELEASE-CHECKLIST.md`).

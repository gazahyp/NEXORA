# Contributing to NEXORA

NEXORA is a **freeware** (closed-source) Windows gaming control hub. This
repository hosts its signed releases and update infrastructure — the place
where users get builds, checksums, and the updater manifest.

Thanks for wanting to help! Here's what contributing means for each kind of
interest.

## 🐛 Bug reports & update failures

Open an [issue](https://github.com/gazahyp/NEXORA/issues) — there's a
template for updater failures. Good reports have:

- Installed version → offered version (Settings → About)
- The stage where it failed (check / download / signature / install)
- Exact error text; a "signature invalid" error is high priority
- Windows build (`winver`) and, ideally, app logs

Security issues go through [private reporting](SECURITY.md) instead.

## 💡 Feature requests & feedback

Issues welcome, tagged with what you're trying to do. NEXORA's roadmap is
personal, but the loudest, most-used ideas get heard — reactions help rank
them.

## 🩹 Patches & code

The application source is not public yet. If you've built something around
NEXORA (integrations, artwork, translations) or you're offering to help open
the source, open an issue first — don't send blind PRs against the release
repo; it's machine-generated and force-pushed by the publish pipeline.

## ✍️ Docs & translations

The app ships EN + ID dictionaries; corrections to either (typos, tone,
localization) are the easiest way to contribute meaningfully. Comment on the
relevant issue or open one titled `docs: …`.

## What this repo is (and isn't)

| | |
| --- | --- |
| ✅ | GitHub Releases with signed installers + `SHA256SUMS.txt` |
| ✅ | `latest.json` Tauri updater manifest (machine-published) |
| ✅ | GitHub Pages download landing (`index.html`) |
| ✅ | Issue tracker for app & updater bugs |
| ❌ | Application source code |
| ❌ | Anything edited by hand — releases overwrite the tree |

By participating, you agree to the [Code of Conduct](CODE_OF_CONDUCT.md).

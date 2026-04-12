# Torikomi Extensions Catalog

Catalog and release APK repository for Torikomi extension packages.

## Purpose

This repository stores:
- `index.json` / `index.min.json` — extension catalog consumed by the app
- `/apk/` — released extension APK binaries
- `/icon/` — extension icons

The Torikomi Android app fetches `index.min.json`, shows available extensions, and installs selected APKs.

## Structure

```text
torikomi-extensions/
|-- index.json       # Human-readable catalog (edit this)
|-- index.min.json   # Minified catalog (auto-generated from index.json)
|-- apk/             # Signed release APK files
`-- icon/            # Extension icon PNGs (<pkg>.png)
```

## Catalog Files

- `index.json` — formatted catalog for editing and code review
- `index.min.json` — minified version consumed by the app at runtime

> Both files must always be in sync. Regenerate `index.min.json` with PowerShell:
> ```powershell
> (Get-Content index.json -Raw | ConvertFrom-Json | ConvertTo-Json -Depth 10 -Compress) | Set-Content index.min.json -Encoding UTF8 -NoNewline
> ```

## Current Extensions

| Name | Package | Platform | Version | Code |
|---|---|---|---|---|
| Torikomi: Musicaldown | `com.torikomi.extension_musicaldown` | TikTok | 1.0.0 | 2 |
| Torikomi: SnapSave Twitter | `com.torikomi.extension_snapsave_twitter` | Twitter/X | 1.0.0 | 2 |
| Torikomi: SnapSave Instagram | `com.torikomi.extension_snapsave_instagram` | Instagram | 1.0.0 | 2 |
| Torikomi: YT1S | `com.torikomi.extension_yt1s` | YouTube | 1.0.0 | 2 |

## Index Entry Format

```json
{
  "name": "Torikomi: Musicaldown",
  "pkg": "com.torikomi.extension_musicaldown",
  "icon": "https://raw.githubusercontent.com/univzy/torikomi-extensions/master/icon/com.torikomi.extension_musicaldown.png",
  "apk": "torikomi-multi.musicaldown-v1.0.0.apk",
  "lang": "multi",
  "code": 2,
  "version": "1.0.0",
  "nsfw": 0,
  "sources": [
    {
      "name": "Musicaldown (TikTok)",
      "lang": "multi",
      "id": "1001",
      "baseUrl": "https://www.tiktok.com",
      "versionId": 1
    }
  ]
}
```

### Field Reference

| Field | Description |
|---|---|
| `name` | Display name shown in the extension catalog |
| `pkg` | Android package name of the extension APK |
| `icon` | URL to the extension icon PNG |
| `apk` | APK filename in `/apk/` |
| `lang` | Language tag (`multi` = multilingual) |
| `code` | Integer version code — bump this on every APK update |
| `version` | Semver version string |
| `nsfw` | `0` = safe, `1` = NSFW (hidden by default) |
| `sources` | List of source entries with `id`, `baseUrl`, `versionId` |

## APK Naming Convention

```text
torikomi-{lang}.{module_id}-v{version}.apk
```

Examples:
- `torikomi-multi.musicaldown-v1.0.0.apk`
- `torikomi-multi.snapsave_instagram-v1.0.0.apk`
- `torikomi-multi.yt1s-v1.0.0.apk`

## Catalog URLs

| Format | URL |
|---|---|
| Minified (app) | `https://raw.githubusercontent.com/univzy/torikomi-extensions/master/index.min.json` |
| Readable | `https://raw.githubusercontent.com/univzy/torikomi-extensions/master/index.json` |

## Publishing a New Version

1. Build new APK via `torikomi-source/build_all.ps1 -CatalogPath "..\torikomi-extensions"`.
2. Bump `code` (integer) in `index.json` for the updated extension entry.
3. Regenerate `index.min.json` (command above).
4. Commit and push: `git add apk/ index.json index.min.json && git commit -m "chore(release): ..." && git push`.

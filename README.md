<p align="center">
  <img src="torikomi.png" width="250" alt="Torikomi" />
</p>

<h1 align="center">Torikomi Extensions Catalog</h1>
<p align="center">
    Catalog and release APK repository for all Torikomi extension packages.
    <br>
    This repository is used by the Torikomi app to discover, display, and install downloader extensions. Extensions are packaged as separate Android APKs that communicate with the main app via `ContentProvider` IPC.
</p>

## Repository Contents

```text
torikomi-extensions/
├── index.json        # Human-readable catalog (edit this file)
├── index.min.json    # Minified catalog (auto-generated from index.json)
├── apk/              # Signed release APK files
└── icon/             # Extension icons (<package_name>.png)
```

## How It Works

1. The Torikomi app fetches `index.min.json` from the raw GitHub URL.
2. The extension list is shown in the app's catalog page.
3. User selects an extension → app downloads the APK from `/apk/`.
4. The APK is installed as a separate app and discovered via manifest metadata.
5. Scraping communication happens through `ContentProvider` IPC:
   ```
   content://torikomi.extension.<id>/scrape?url=...&cfCookies=...
   ```

## Available Extensions

| Name | Package | Platform | Downloader | Source ID | Version |
|---|---|---|---|---|---|
| Torikomi: Musicaldown | `com.torikomi.extension_musicaldown` | TikTok | MusicalDown | 1001 | 1.0.0 |
| Torikomi: SnapSave Twitter | `com.torikomi.extension_snapsave_twitter` | Twitter/X | SnapSave | 1012 | 1.0.0 |
| Torikomi: SnapSave Instagram | `com.torikomi.extension_snapsave_instagram` | Instagram | SnapSave | 1013 | 1.0.0 |
| Torikomi: SnapSave Threads | `com.torikomi.extension_snapsave_threads` | Threads | SnapSave | 1016 | 1.0.0 |
| Torikomi: SnapSave Facebook | `com.torikomi.extension_snapsave_facebook` | Facebook | SnapSave | 1015 | 1.0.0 |
| Torikomi: YTDown | `com.torikomi.extension_ytdown` | YouTube | YTDown | 1014 | 1.0.0 |
| Torikomi: Spotmate | `com.torikomi.extension_spotmate` | Spotify | Spotmate Downloader | 1017 | 1.0.0 |

## Catalog URLs

| Format | URL |
|---|---|
| Minified (used by app) | `https://raw.githubusercontent.com/univzy/torikomi-extensions/master/index.min.json` |
| Human-readable | `https://raw.githubusercontent.com/univzy/torikomi-extensions/master/index.json` |

## Catalog Entry Format

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

| Field | Type | Description |
|---|---|---|
| `name` | string | Display name shown in the extension catalog |
| `pkg` | string | Android package name of the extension APK |
| `icon` | string | URL to the extension icon PNG |
| `apk` | string | APK filename in `/apk/` |
| `lang` | string | Language tag (`multi` = multilingual) |
| `code` | int | Integer version code — increment on every APK update |
| `version` | string | Semver version string (e.g. `1.0.0`) |
| `nsfw` | int | `0` = safe, `1` = NSFW (hidden by default) |
| `sources[].id` | string | Unique source ID used by the app for routing |
| `sources[].baseUrl` | string | Base URL of the platform |
| `sources[].versionId` | int | Internal source entry version |

## APK Naming Convention

```
torikomi-{lang}.{module_id}-v{version}.apk
```

Examples:
- `torikomi-multi.musicaldown-v1.0.0.apk`
- `torikomi-multi.spotmate-v1.0.0.apk`
- `torikomi-multi.ytdown-v1.0.0.apk`

## Managing the Catalog

### Syncing index.min.json

Whenever `index.json` is edited, regenerate `index.min.json` with PowerShell:

```powershell
(Get-Content index.json -Raw | ConvertFrom-Json | ConvertTo-Json -Depth 10 -Compress) | Set-Content index.min.json -Encoding UTF8 -NoNewline
```

### Adding a New Extension

1. Build the APK via `torikomi-source`:
   ```powershell
   cd ..\torikomi-source
   .\build_all.ps1 -Extensions "extension_name" -CatalogPath "..\torikomi-extensions"
   ```
2. Add a new entry to `index.json` with the appropriate `code` and `version`.
3. Copy the icon to `/icon/<package_name>.png`.
4. Regenerate `index.min.json` (command above).
5. Commit and push:
   ```powershell
   git add apk/ icon/ index.json index.min.json
   git commit -m "feat: add <name> extension v<version>"
   git push
   ```

### Releasing a New Version of an Existing Extension

1. Rebuild the APK: `.\build_all.ps1 -Extensions "<id>" -CatalogPath "..\torikomi-extensions"`
2. Increment the `code` (integer) in `index.json` for that extension.
3. Update the `version` string if needed.
4. Regenerate `index.min.json`.
5. Commit and push.

## Source ID Registry

Source IDs are used by the app to identify and route requests to the correct extension.

| Source ID | Extension | Platform |
|---|---|---|
| 1001 | musicaldown | TikTok |
| 1012 | snapsave_twitter | Twitter/X |
| 1013 | snapsave_instagram | Instagram |
| 1014 | ytdown | YouTube |
| 1015 | snapsave_facebook | Facebook |
| 1016 | snapsave_threads | Threads |
| 1017 | spotmate | Spotify |

> Next available ID: **1018**

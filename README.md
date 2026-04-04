# Torikomi Extension Repository

This repository contains the extension catalog and APK releases for [Torikomi Downloader](https://github.com/univzy/torikomi-dev).

## How it works

Torikomi Downloader uses a **Tachiyomi/Mihon-inspired extension system**. Each downloader is a separate APK app.
The main app fetches `index.min.json` to discover and install individual platform extensions.

When an extension APK is installed, the main app communicates with it via Android **ContentProvider IPC** to perform scraping.

## Repository Structure

```
torikomi-extensions/
├── index.json          ← Extension catalog (formatted)
├── index.min.json      ← Extension catalog (minified, used by app)
├── icon/               ← Extension icons ({pkg}.png)
└── apk/                ← Compiled extension APKs
    ├── torikomi-multi.tiktok-v1.0.0.apk
    ├── torikomi-multi.youtube-v1.0.0.apk
    ├── torikomi-multi.instagram-v1.0.0.apk
    ├── torikomi-multi.facebook-v1.0.0.apk
    ├── torikomi-multi.twitter-v1.0.0.apk
    ├── torikomi-multi.threads-v1.0.0.apk
    ├── torikomi-multi.pinterest-v1.0.0.apk
    ├── torikomi-multi.spotify-v1.0.0.apk
    ├── torikomi-multi.soundcloud-v1.0.0.apk
    ├── torikomi-zh.douyin-v1.0.0.apk
    ├── torikomi-zh.bilibili-v1.0.0.apk
    └── torikomi-multi.whatsapp_status-v1.0.0.apk
```

## Index Format

Each entry in `index.json` / `index.min.json` follows this schema:

```json
{
  "name":    "Torikomi: TikTok",
  "pkg":     "com.torikomi.extension_musicaldown",
  "apk":     "torikomi-multi.tiktok-v1.0.0.apk",
  "lang":    "multi",
  "code":    1,
  "version": "1.0.0",
  "nsfw":    0,
  "sources": [
    {
      "name":      "TikTok",
      "lang":      "multi",
      "id":        "1001",
      "baseUrl":   "https://www.tiktok.com",
      "versionId": 1
    }
  ]
}
```

| Field | Description |
|-------|-------------|
| `name` | Display name shown in the app (prefixed with `Torikomi: `) |
| `pkg` | Android package name |
| `apk` | APK filename in the `apk/` directory |
| `lang` | Language code (`multi`, `en`, `zh`, etc.) |
| `code` | Integer version code |
| `version` | Version string |
| `nsfw` | `0` = safe, `1` = adult content |
| `sources` | Array of download sources (one per extension) |

## Available Extensions

| Name | Package | Language | APK |
|------|---------|----------|-----|
| Torikomi: TikTok | com.torikomi.extension_musicaldown | multi | torikomi-multi.tiktok-v1.0.0.apk |
| Torikomi: YouTube | com.torikomi.extension_youtube | multi | torikomi-multi.youtube-v1.0.0.apk |
| Torikomi: Instagram | com.torikomi.extension_instagram | multi | torikomi-multi.instagram-v1.0.0.apk |
| Torikomi: Facebook | com.torikomi.extension_facebook | multi | torikomi-multi.facebook-v1.0.0.apk |
| Torikomi: Twitter | com.torikomi.extension_twitter | multi | torikomi-multi.twitter-v1.0.0.apk |
| Torikomi: Threads | com.torikomi.extension_threads | multi | torikomi-multi.threads-v1.0.0.apk |
| Torikomi: Pinterest | com.torikomi.extension_pinterest | multi | torikomi-multi.pinterest-v1.0.0.apk |
| Torikomi: Spotify | com.torikomi.extension_spotify | multi | torikomi-multi.spotify-v1.0.0.apk |
| Torikomi: SoundCloud | com.torikomi.extension_soundcloud | multi | torikomi-multi.soundcloud-v1.0.0.apk |
| Torikomi: Douyin | com.torikomi.extension_douyin | zh | torikomi-zh.douyin-v1.0.0.apk |
| Torikomi: Bilibili | com.torikomi.extension_bilibili | zh | torikomi-zh.bilibili-v1.0.0.apk |
| Torikomi: WhatsApp Status | com.torikomi.extension_whatsapp_status | multi | torikomi-multi.whatsapp_status-v1.0.0.apk |

## Catalog URL

The main app fetches:

```
https://raw.githubusercontent.com/univzy/torikomi-extensions/main/index.min.json
```

Or the formatted version:

```
https://raw.githubusercontent.com/univzy/torikomi-extensions/main/index.json
```

## APK Naming Convention

```
torikomi-{lang}.{id}-v{version}.apk
```

Examples:
- `torikomi-multi.tiktok-v1.0.0.apk`
- `torikomi-zh.douyin-v1.0.0.apk`

## Icon Naming Convention

Icons are placed in `icon/` and named after the package:

```
icon/{pkg}.png
```

Example: `icon/com.torikomi.extension_musicaldown.png`

## For Developers

### Adding a new extension

1. Create a new folder under `extensions/<extension-id>/`
2. Add `extension.json` with the required metadata:

```json
{
  "id": "your-extension-id",
  "name": "Extension Display Name",
  "version": "1.0.0",
  "lang": "multi",
  "description": "What this extension can download",
  "author": "YourGitHubUsername",
  "icon": "https://raw.githubusercontent.com/univzy/torikomi-extensions/main/extensions/<id>/icon.png",
  "categories": ["video", "audio", "image"],
  "supportedUrls": ["example.com"],
  "minAppVersion": "1.0.0",
  "nsfw": false,
  "changelog": "Initial release"
}
```

3. Add your extension entry to `index.json`
4. Add the corresponding scraper code to the main Torikomi Downloader app

### Fields reference

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Unique lowercase identifier (e.g. `tiktok`, `youtube`) |
| `name` | string | Display name shown in the app |
| `version` | string | Semantic version (x.y.z) |
| `lang` | string | Language code (`multi`, `en`, `zh`, `ja`, etc.) |
| `description` | string | Short description of what the extension downloads |
| `author` | string | GitHub username of the author |
| `icon` | string | URL to the extension icon (PNG, 128×128 recommended) |
| `categories` | array | Content types: `video`, `audio`, `image` |
| `supportedUrls` | array | Domain list the extension handles |
| `minAppVersion` | string | Minimum Torikomi Downloader version required |
| `nsfw` | boolean | Whether the extension is marked for adult content |
| `changelog` | string | Changes in this version |

## License

This project is licensed under the [MIT License](LICENSE).
"# torikomi-extensions" 

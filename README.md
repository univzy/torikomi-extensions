# AIO Extension Repository

This repository contains the extension catalog and APK releases for [AIO Downloader](https://github.com/TobyG74/AIO-Downloader).

## How it works

AIO Downloader uses a **Tachiyomi/Mihon-inspired extension system**. Each downloader is a separate APK app.
The main app fetches `index.min.json` to discover and install individual platform extensions.

When an extension APK is installed, the main app communicates with it via Android **ContentProvider IPC** to perform scraping.

## Repository Structure

```
torikomi-extensions/
├── index.json          ← Extension catalog (formatted)
├── index.min.json      ← Extension catalog (minified, used by app)
├── icon/               ← Extension icons ({pkg}.png)
└── apk/                ← Compiled extension APKs
    ├── aio-multi.tiktok-v1.0.0.apk
    ├── aio-multi.youtube-v1.0.0.apk
    ├── aio-multi.instagram-v1.0.0.apk
    ├── aio-multi.facebook-v1.0.0.apk
    ├── aio-multi.twitter-v1.0.0.apk
    ├── aio-multi.threads-v1.0.0.apk
    ├── aio-multi.pinterest-v1.0.0.apk
    ├── aio-multi.spotify-v1.0.0.apk
    ├── aio-multi.soundcloud-v1.0.0.apk
    ├── aio-zh.douyin-v1.0.0.apk
    ├── aio-zh.bilibili-v1.0.0.apk
    └── aio-multi.whatsapp_status-v1.0.0.apk
```

## Index Format

Each entry in `index.json` / `index.min.json` follows this schema:

```json
{
  "name":    "AIO: TikTok",
  "pkg":     "com.tobz.aio_extension_tiktok",
  "apk":     "aio-multi.tiktok-v1.0.0.apk",
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
| `name` | Display name shown in the app (prefixed with `AIO: `) |
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
| AIO: TikTok | com.tobz.aio_extension_tiktok | multi | aio-multi.tiktok-v1.0.0.apk |
| AIO: YouTube | com.tobz.aio_extension_youtube | multi | aio-multi.youtube-v1.0.0.apk |
| AIO: Instagram | com.tobz.aio_extension_instagram | multi | aio-multi.instagram-v1.0.0.apk |
| AIO: Facebook | com.tobz.aio_extension_facebook | multi | aio-multi.facebook-v1.0.0.apk |
| AIO: Twitter | com.tobz.aio_extension_twitter | multi | aio-multi.twitter-v1.0.0.apk |
| AIO: Threads | com.tobz.aio_extension_threads | multi | aio-multi.threads-v1.0.0.apk |
| AIO: Pinterest | com.tobz.aio_extension_pinterest | multi | aio-multi.pinterest-v1.0.0.apk |
| AIO: Spotify | com.tobz.aio_extension_spotify | multi | aio-multi.spotify-v1.0.0.apk |
| AIO: SoundCloud | com.tobz.aio_extension_soundcloud | multi | aio-multi.soundcloud-v1.0.0.apk |
| AIO: Douyin | com.tobz.aio_extension_douyin | zh | aio-zh.douyin-v1.0.0.apk |
| AIO: Bilibili | com.tobz.aio_extension_bilibili | zh | aio-zh.bilibili-v1.0.0.apk |
| AIO: WhatsApp Status | com.tobz.aio_extension_whatsapp_status | multi | aio-multi.whatsapp_status-v1.0.0.apk |

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
aio-{lang}.{id}-v{version}.apk
```

Examples:
- `aio-multi.tiktok-v1.0.0.apk`
- `aio-zh.douyin-v1.0.0.apk`

## Icon Naming Convention

Icons are placed in `icon/` and named after the package:

```
icon/{pkg}.png
```

Example: `icon/com.tobz.aio_extension_tiktok.png`

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
4. Add the corresponding scraper code to the main AIO Downloader app

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
| `minAppVersion` | string | Minimum AIO Downloader version required |
| `nsfw` | boolean | Whether the extension is marked for adult content |
| `changelog` | string | Changes in this version |

## License

This project is licensed under the [MIT License](LICENSE).
"# torikomi-extensions" 

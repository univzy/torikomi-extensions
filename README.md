# Torikomi Extensions Catalog

Catalog and release APK repository for Torikomi extension packages.

## Purpose

This repository stores:
- extension index files consumed by the app
- released extension APK artifacts

The Android app fetches `index.min.json` and installs selected extension APKs.

## Structure

```text
torikomi-extensions/
|-- index.json
|-- index.min.json
|-- apk/
`-- icon/
```

## Catalog Files

- `index.json`: formatted catalog for editing/review
- `index.min.json`: minified catalog consumed by app

Both files must contain the same entries.

## Current Extensions

| Name | Package | Lang | APK |
|---|---|---|---|
| Torikomi: Musicaldown | com.torikomi.extension_musicaldown | multi | torikomi-multi.musicaldown-v1.0.0.apk |
| Torikomi: SnapSave Twitter | com.torikomi.extension_snapsave_twitter | multi | torikomi-multi.snapsave_twitter-v1.0.0.apk |

## Index Entry Format

```json
{
  "name": "Torikomi: Musicaldown",
  "pkg": "com.torikomi.extension_musicaldown",
  "apk": "torikomi-multi.musicaldown-v1.0.0.apk",
  "lang": "multi",
  "code": 1,
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

## Naming Convention

APK file naming:

```text
torikomi-{lang}.{id}-v{version}.apk
```

Examples:
- `torikomi-multi.musicaldown-v1.0.0.apk`
- `torikomi-multi.snapsave_twitter-v1.0.0.apk`

## Catalog URL

- `https://raw.githubusercontent.com/univzy/torikomi-extensions/main/index.min.json`
- `https://raw.githubusercontent.com/univzy/torikomi-extensions/main/index.json`

<p align="center">
  <h1 align="center">CloudTube</h1>
  <p align="center">
    Upload videos to YouTube from <b>Google Drive</b>, <b>Direct URLs</b>, or <b>Torrents</b> — entirely free, from your phone.
  </p>
  <p align="center">
    <a href="https://colab.research.google.com/"><img src="https://img.shields.io/badge/Runs%20on-Google%20Colab-F9AB00?logo=googlecolab&logoColor=white" alt="Google Colab"></a>
    <a href="https://core.telegram.org/bots"><img src="https://img.shields.io/badge/Telegram-Bot-26A5E4?logo=telegram&logoColor=white" alt="Telegram Bot"></a>
    <img src="https://img.shields.io/badge/Cost-$0%20forever-brightgreen" alt="Free">
    <img src="https://img.shields.io/badge/Files-up%20to%2025GB+-blue" alt="25GB+">
  </p>
</p>

---

## What is CloudTube?

CloudTube lets you upload videos to YouTube without downloading them to your computer. Everything runs on Google Colab (free cloud VM), so you can do it from your phone.

```
Source (Drive / URL / Torrent)  →  Colab VM (/tmp)  →  YouTube
```

Two ways to use it:

| | Colab Notebook | Telegram Bot |
|---|---|---|
| **File** | `youtube_cloud_uploader.ipynb` | `telegram_bot.ipynb` |
| **How** | Fill a form, click play | Message the bot from Telegram |
| **Best for** | Quick one-off uploads | Uploading on the go from your phone |

---

## Features

| Feature | Colab | Telegram Bot |
|---------|:-----:|:------------:|
| Upload from Google Drive | :white_check_mark: | :white_check_mark: |
| Upload from direct URL | :white_check_mark: | :white_check_mark: |
| Upload from torrent / magnet link | :white_check_mark: | :white_check_mark: |
| Batch upload (Drive folder) | :white_check_mark: | :white_check_mark: |
| Resumable uploads with auto-retry | :white_check_mark: | :white_check_mark: |
| Progress tracking | :white_check_mark: | :white_check_mark: |
| Privacy control (private/unlisted/public) | :white_check_mark: | :white_check_mark: |
| Persistent auth (saved to Google Drive) | :white_check_mark: | :white_check_mark: |
| Works from mobile | :white_check_mark: | :white_check_mark: |

---

## Quick Start

### Option A: Colab Notebook (form-based)

1. Open [`youtube_cloud_uploader.ipynb`](youtube_cloud_uploader.ipynb) in [Google Colab](https://colab.research.google.com/)
2. **Runtime → Run all** (2 setup cells)
3. Authenticate with YouTube (one-time — token saves to your Google Drive)
4. Pick an upload cell (Drive / URL / Torrent / Batch), fill in the form, run it

### Option B: Telegram Bot

1. Create a bot via [@BotFather](https://t.me/BotFather) on Telegram — copy the token
2. Get your user ID from [@userinfobot](https://t.me/userinfobot)
3. Open [`telegram_bot.ipynb`](telegram_bot.ipynb) in [Google Colab](https://colab.research.google.com/)
4. Paste your bot token + user ID in the config cell
5. **Runtime → Run all** (3 cells)
6. Message your bot on Telegram:

```
/upload_url      — Upload from a direct video URL
/upload_drive    — Upload from Google Drive link
/upload_torrent  — Upload from torrent or magnet link
/batch_drive     — Upload all videos from a Drive folder
/quota           — Check API quota info
/cancel          — Cancel current operation
```

---

## How Torrent Uploads Work

CloudTube uses `aria2c` (installed automatically on Colab) to download torrents:

```
Magnet link / .torrent URL
    ↓
aria2c --seed-time=0 (downloads, no seeding)
    ↓
Picks the largest video file (.mp4, .mkv, .avi, etc.)
    ↓
Uploads to YouTube via resumable upload
    ↓
Cleans up temp files
```

Supports: magnet links, direct `.torrent` file URLs. Downloads use 16 parallel connections for maximum speed on Colab's fast network.

---

## YouTube API Quota

| | |
|---|---|
| Daily quota | 10,000 units |
| Cost per upload | 1,600 units |
| **Uploads per day** | **~6 videos** |

To request higher quota: Google Cloud Console → APIs & Services → YouTube Data API v3 → Quotas → Apply for higher quota.

---

## FAQ

<details>
<summary><b>Do I need to log in every time I restart Colab?</b></summary>

No. Your YouTube auth token is saved to Google Drive (`MyDrive/CloudTubeBot/token.pickle`). After the first login, it auto-restores silently on every restart. You only re-authenticate if you revoke access.
</details>

<details>
<summary><b>How big can the files be?</b></summary>

Colab has ~100GB of `/tmp` space, so files up to ~80GB work comfortably. The upload uses resumable chunked transfer (10MB chunks) with automatic retry on server errors.
</details>

<details>
<summary><b>Does the Telegram bot stay alive forever?</b></summary>

No — it runs as long as the Colab session is active (~12 hours max). When it dies, just re-run the notebook. Auth is instant since the token is already on Drive.
</details>

<details>
<summary><b>"Quota exceeded" error</b></summary>

You've hit the daily upload limit (~6 videos). Wait 24 hours or request a quota increase from Google Cloud Console.
</details>

<details>
<summary><b>"File not found" for Drive files</b></summary>

Make sure the file is either in your own Drive, shared with you, or has "Anyone with the link" sharing enabled.
</details>

<details>
<summary><b>OAuth "unverified app" warning</b></summary>

This is normal. Click "Advanced" → "Go to CloudTube (unsafe)" to proceed. The app only accesses YouTube Upload and Drive Read permissions.
</details>

---

## Google Drive Permissions

The Drive file must be accessible to your Google account:

- Files in your own Drive — always works
- Files shared with you — works
- Files with "Anyone with the link" sharing — works

---

## Supported Video Formats

`.mp4` `.mkv` `.avi` `.mov` `.webm` `.flv` `.wmv` `.ts` `.m4v` `.mpg` `.mpeg` `.3gp`

For direct URL uploads, the URL must point directly to a video file, not a webpage.

---

## Use Your Own OAuth Credentials

The bundled credentials work out of the box. If you want your own API quota:

1. Go to [Google Cloud Console](https://console.cloud.google.com/) → create a project
2. Enable **YouTube Data API v3** and **Google Drive API**
3. Go to **APIs & Services → OAuth consent screen** → External → fill in app name
4. Go to **Credentials → Create Credentials → OAuth client ID → Desktop app**
5. Download the JSON, save as `client_secret.json` in the notebook directory

---

## Project Structure

```
CloudTube/
├── youtube_cloud_uploader.ipynb   # Colab notebook (form-based uploads)
├── telegram_bot.ipynb             # Colab notebook (Telegram bot)
├── client_secret.json             # OAuth credentials (auto-fetched)
└── README.md
```

---

<p align="center">
  <sub>Built for uploading videos from anywhere, on any device, for free.</sub>
</p>

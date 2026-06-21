# CloudTube - A YouTube Uploader

Upload videos to YouTube directly from **Google Drive** or a **direct URL** using Google Colab — no local download needed.

Video data streams: `Source → Colab VM (temp) → YouTube`. Nothing touches your local machine.

## Quick Start

1. Open `youtube_cloud_uploader.ipynb` in [Google Colab](https://colab.research.google.com/)
2. Run all cells — credentials are loaded automatically
3. Sign in with your Google account when prompted
4. Paste a Drive link or video URL and upload

## Features

| Feature | Description |
|---------|-------------|
| Google Drive → YouTube | Paste a Drive share link or file ID |
| URL → YouTube | Paste any direct video URL |
| Batch Upload | Upload all videos from a Drive folder |
| Resumable Uploads | Automatically retries on server errors |
| Progress Tracking | Shows download and upload progress |
| Privacy Control | Set private/unlisted/public per video |

## Important Notes

### YouTube API Quota

- Default daily quota: **10,000 units**
- Each video upload costs **1,600 units**
- That's roughly **6 uploads per day**

### Colab Session Limits

- Free Colab sessions may disconnect after ~90 minutes of inactivity
- For large files, keep the tab active
- Colab Pro gives longer sessions and more RAM/disk

### Supported URL Types

For direct URL uploads, the URL must point directly to a video file (`.mp4`, `.mkv`, `.avi`, etc.), not a webpage. Examples of what works:

- Direct download links from cloud storage
- CDN links to video files
- Any URL that returns a video file when accessed

### Google Drive Permissions

The Drive file must be accessible to your Google account:
- Files in your own Drive (always works)
- Files shared with you (works)
- Files with "Anyone with the link" sharing (works)

## Troubleshooting

**"Quota exceeded"**
→ You've hit the daily upload limit. Wait 24 hours or request a quota increase.

**"File not found" for Drive files**
→ Make sure the file is shared with your account or has link sharing enabled.

**OAuth consent screen shows "unverified app" warning**
→ This is normal. Click "Advanced" → "Go to CloudTube (unsafe)" to proceed.

## Advanced: Use Your Own Credentials

If you want your own API quota, you can provide your own OAuth credentials:

1. Go to [Google Cloud Console](https://console.cloud.google.com/) and create a project
2. Enable **YouTube Data API v3** and **Google Drive API**
3. Go to **APIs & Services → OAuth consent screen** → select **External** → fill in app name and emails
4. Go to **APIs & Services → Credentials** → **Create Credentials → OAuth client ID** → select **Desktop app**
5. Download the JSON and save it as `client_secret.json` in the notebook directory

The notebook will use your credentials instead of the bundled ones.

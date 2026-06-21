# CloudTube - A YouTube Uploader

Upload videos to YouTube directly from **Google Drive** or a **direct URL** using Google Colab — no local download needed.

Video data streams: `Source → Colab VM (temp) → YouTube`. Nothing touches your local machine.

## Quick Start

1. Set up Google Cloud credentials (see below)
2. Open `youtube_cloud_uploader.ipynb` in [Google Colab](https://colab.research.google.com/)
3. Upload your `client_secret.json` when prompted
4. Paste a Drive link or video URL and run

## Google Cloud Setup (One-Time, ~5 minutes)

### Step 1: Create a Google Cloud Project

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Click the project dropdown (top bar) → **New Project**
3. Name it something like `youtube-uploader` → **Create**
4. Select the new project from the dropdown

### Step 2: Enable APIs

1. Go to **APIs & Services → Library**
2. Search and enable these two APIs:
   - **YouTube Data API v3**
   - **Google Drive API**

### Step 3: Configure OAuth Consent Screen

1. Go to **APIs & Services → OAuth consent screen**
2. Select **External** → **Create**
3. Fill in:
   - App name: `YouTube Uploader` (anything you like)
   - User support email: your email
   - Developer contact email: your email
4. Click **Save and Continue** through the remaining steps
5. Under **Test users**, add your own Google email
6. **Publish the app** if you want to skip the "unverified app" warning (optional)

### Step 4: Create OAuth Credentials

1. Go to **APIs & Services → Credentials**
2. Click **+ Create Credentials → OAuth client ID**
3. Application type: **Desktop app**
4. Name: `YouTube Uploader` (anything)
5. Click **Create**
6. Click **Download JSON** — save it as `client_secret.json`

That's it! You now have the credentials file needed by the notebook.

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
- That's roughly **6 uploads per day** with a new project
- Request a quota increase: Cloud Console → APIs → YouTube Data API v3 → Quotas

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

**"Access blocked: This app's request is invalid"**
→ Make sure you selected "Desktop app" as the application type for your OAuth credentials.

**"The caller does not have permission"**
→ Make sure YouTube Data API v3 is enabled in your Cloud project.

**"Quota exceeded"**
→ You've hit the daily upload limit. Wait 24 hours or request a quota increase.

**"File not found" for Drive files**
→ Make sure the file is shared with your account or has link sharing enabled.

**OAuth consent screen shows "unverified app" warning**
→ This is normal for personal-use apps. Click "Advanced" → "Go to YouTube Uploader (unsafe)" to proceed. Or publish the app in the consent screen settings.

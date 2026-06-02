# Candfans Downloader

A Tampermonkey userscript to scan and download videos from [Candfans](https://candfans.com) creators you subscribe to.

Runs in the browser with your existing Candfans login session. It uses `mux.js` from jsDelivr to remux HLS segments into MP4 for in-browser downloads.

## Features

- Auto-detects creator and plan from the current page URL
- Scans all accessible posts and extracts video URLs
- Filter by quality, minimum duration, and plan
- **In-browser download** - downloads HLS streams directly and saves MP4 when remuxing succeeds
- **Export URL list** - TSV file with all video metadata for use with external tools
- Dark themed floating panel UI

## Install

1. Install [Tampermonkey](https://www.tampermonkey.net/) browser extension
2. Click to install from [Greasy Fork](https://greasyfork.org/scripts/571983-candfans-downloader)
3. Navigate to any creator's page on `candfans.com` or `candfans.jp`

## Usage

1. **Log in** to Candfans with your account
2. Navigate to a creator's page (for example, `candfans.com/username`)
3. Click the purple download button (bottom-right corner)
4. Adjust filters if needed, then click **Scan**
5. Choose:
   - **Download in browser** - downloads each video directly (no external tools)
   - **Export URL list** - saves a `.tsv` file with all video URLs

### Using the URL list with external tools

The exported `.tsv` file contains `post_id`, `duration`, `title`, and `url` columns.

The video URLs are public HLS streams (`.m3u8`) that can be downloaded with any tool:

```bash
# yt-dlp (recommended for bulk downloads)
yt-dlp "https://video.candfans.jp/user/.../xxx.m3u8" -o "video.mp4"

# ffmpeg
ffmpeg -i "https://video.candfans.jp/user/.../xxx.m3u8" -c copy video.mp4
```

## How it works

1. The script runs on `candfans.com` and `candfans.jp` pages, leveraging your existing login session
2. It calls the Candfans API to list posts from the creator's timeline
3. For accessible posts (`can_browsing === 1`), it extracts the HLS video URLs
4. Videos are hosted on a public CDN - once you have the URL, no authentication is needed

Only content from plans you have actively subscribed to is accessible. The script cannot bypass access controls.

## Limitations

- **In-browser download** buffers the entire video in memory before saving. For very large files (500MB+), this may be slow or cause tab crashes. Use the URL list export with yt-dlp for bulk downloads.
- Only downloads content you have access to through your subscriptions
- In-browser download buffers the whole video before saving. The script normally remuxes HLS segments into `.mp4`; if mux.js is unavailable or remuxing fails, it falls back to a `.ts` file that can be remuxed with ffmpeg: `ffmpeg -i video.ts -c copy video.mp4`

## Source

[GitHub Repository](https://github.com/HomoLab/candfans-downloader)

## License

MIT

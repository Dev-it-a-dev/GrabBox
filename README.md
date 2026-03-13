# GrabBox — Smart Media Downloader

A 100% client-side web tool for browsing and downloading files from album and gallery hosting sites. Runs entirely in your browser — no server, no backend, no data stored anywhere.

**[→ Live Demo on GitHub Pages](https://dev-it-a-dev.github.io/GrabBox/)**

---

## Features

- **Paste & Fetch** — Paste one or more album URLs, fetched via CORS proxy with `?advanced=1` for full file listing (no pagination misses)
- **3 View Modes** — Table (data-dense), Grid (visual cards), Gallery (full-screen lightbox with swipe/keyboard navigation)
- **Filter & Search** — Type chips (Image / Video / Other) + live text search by filename
- **Sort** — Click column headers to sort by name, type, or file size
- **Download** — Individual, multi-select, or bulk (all images / all videos / everything)
- **ZIP packaging** — Client-side ZIP via JSZip for bulk downloads
- **Export Links** — Resolve direct download URLs and copy/save as `.txt` for use with wget, aria2, JDownloader
- **Cloudflare Worker** — Optional tiny proxy (~60 lines) that sets the required Referer header for CDNs with anti-hotlinking
- **Multi-album** — Paste several URLs, browse all in one unified list
- **Pluggable providers** — Easy to add new hosting sites
- **Mobile-first** — Touch targets, swipe gallery, responsive layout
- **Keyboard shortcuts** — `Ctrl+A` select all, `Escape` deselect, `←/→` gallery navigation

## Download Strategies

GrabBox tries multiple download methods in order:

| Priority | Method | When it works |
|----------|--------|---------------|
| 1 | **Cloudflare Worker** | Always (sets correct Referer header) |
| 2 | **Direct fetch** | When CDN allows CORS |
| 3 | **CORS proxy** | When CDN doesn't check Referer |
| 4 | **Export links** | Always (manual download via external tool) |

### Setting up the Cloudflare Worker (recommended)

The Worker is needed because most CDNs require a `Referer` header that browsers can't set from JS.

1. Go to [Cloudflare Workers](https://workers.cloudflare.com/) — free account, no credit card
2. Create a new Worker
3. Paste the contents of `worker.js` from this repo
4. Deploy — you get a URL like `https://grabbox-dl.yourname.workers.dev`
5. Paste that URL into GrabBox's "Download Worker" field

Free tier: 100,000 requests/day. More than enough for personal use.

### Without the Worker

If you don't deploy a Worker, GrabBox will still:
- Try direct downloads (works for some CDNs)
- Try CORS proxy downloads (works when no Referer check)
- Let you **Export Links** as a text list to use with wget/aria2/JDownloader

## Files

```
index.html   ← The complete app (single file, deploy anywhere)
worker.js    ← Cloudflare Worker source (optional, for reliable downloads)
README.md    ← This file
```

## Deployment

### GitHub Pages
1. Push `index.html` to any repo
2. Settings → Pages → Source: main branch
3. Live at `https://yourusername.github.io/grabbox/`

### Local
Open `index.html` in any browser.

## Adding New Providers

```js
providers.push({
  name: 'sitename',
  match: (url) => { /* return { albumId, domain } or null */ },
  thumbBase: (uuid) => `https://.../${uuid}.webp`,
  apiEndpoint: 'https://...',
  refererBase: 'https://...',
});
```

## Tech Stack

- Vanilla HTML/CSS/JS — zero build step, single file
- [JSZip](https://stuk.github.io/jszip/) — client-side ZIP
- Google Fonts — JetBrains Mono, DM Sans, Space Grotesk

## License

MIT
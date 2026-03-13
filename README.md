# GrabBox — Smart Media Downloader

A 100% client-side web tool for browsing and downloading files from album and gallery hosting sites. Runs entirely in your browser — no server, no backend, no data stored anywhere.

**[→ Live Demo on GitHub Pages](https://dev-it-a-dev.github.io/grabbox/)**

---

## Features

- **Paste & Fetch** — Paste one or more album URLs from supported sites, the tool fetches and parses them via a CORS proxy
- **File Browser** — Table view (data-dense) or Grid view (visual thumbnails), toggle freely
- **Filter & Search** — Type filters (Image / Video / Other) + live text search by filename
- **Sort** — Click column headers to sort by name, type, or file size
- **Download individual** — One-click download for any single file
- **Download selected** — Check multiple files, then download individually or as a single ZIP
- **Bulk download** — Download all images, all videos, or all files at once (auto-ZIPs for 5+)
- **Multi-album support** — Paste several album URLs and browse them all in one unified list
- **Pluggable providers** — Extensible architecture for adding new hosting sites easily
- **Keyboard shortcuts** — `Ctrl+A` to select all, `Escape` to clear selection

## How It Works

1. You paste album/gallery URLs from supported hosting sites
2. The app fetches each page through a CORS proxy (you choose which proxy to use)
3. It parses the page HTML to extract file metadata (name, size, type, thumbnails)
4. Files are displayed in a filterable, sortable list
5. Downloads are fetched through the CORS proxy and saved via the browser's download API
6. ZIP packaging is done client-side with JSZip

## Adding New Providers

The app uses a pluggable provider system. Each provider is an object in the `providers` array with:

```js
{
  name: 'sitename',
  match: (url) => { /* return { albumId, domain } or null */ },
  thumbBase: (uuid) => `https://.../${uuid}.webp`,  // optional
  apiEndpoint: 'https://...',                        // optional
  refererBase: 'https://...',                        // optional
}
```

To add support for a new site, add a provider object to the array and implement the appropriate parsing logic in the `parseAlbumHtml` function (or create a site-specific parser).

## CORS Proxies

Since this runs on GitHub Pages (static hosting), it needs a CORS proxy to fetch pages. Built-in options:

| Proxy | URL |
|-------|-----|
| **codetabs** | `api.codetabs.com/v1/proxy` |
| **corsproxy.io** | `corsproxy.io` |
| **allorigins** | `api.allorigins.win` |
| **Custom** | Enter your own proxy URL |

> **Tip:** If one proxy is slow or rate-limited, switch to another. You can also deploy your own CORS proxy for reliability (e.g. [cors-anywhere](https://github.com/Rob--W/cors-anywhere) on a free Cloudflare Worker).

## Deployment

### GitHub Pages (recommended)

1. Fork this repo (or create a new one)
2. Drop `index.html` into the root
3. Go to **Settings → Pages → Source: main branch**
4. Your tool is live at `https://yourusername.github.io/grabbox/`

### Local

Just open `index.html` in any modern browser. That's it.

## Limitations

- **CORS proxy dependency** — Free public proxies may be rate-limited, slow, or go offline. For heavy use, self-host a proxy.
- **Sites change frequently** — Domains rotate, page structures evolve, API endpoints shift. The parsers handle current known structures but may need updates.
- **Large files** — Downloading very large videos through a CORS proxy can be slow or time out. Consider a dedicated downloader for files over ~500MB.
- **No authentication** — This tool doesn't handle accounts or password-protected albums.

## Tech Stack

- Vanilla HTML/CSS/JS — zero build step
- [JSZip](https://stuk.github.io/jszip/) — client-side ZIP creation
- Google Fonts — JetBrains Mono, DM Sans, Space Grotesk

## License

MIT — do whatever you want with it.
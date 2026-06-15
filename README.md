# Sailing Conditions

A clean, mobile-first dashboard for a Davis **WeatherLink** weather station,
built for sailing where **wind is the headline metric**.

🔗 Source station: [WeatherLink summary page](https://www.weatherlink.com/embeddablePage/show/369db7169c3743889aa6034a27ad4f5d/summary)

## What it does

- **Wind hero** — current speed, gust, average, and a live compass showing the
  direction the wind blows *from* (with cardinal + degrees).
- **Conditions grid** — temperature, feels-like, humidity, dew point,
  barometer (+ trend), rain rate, rain today, solar, UV.
- **More** section automatically lists any other sensors the station reports.
- **Live & resilient** — auto-refreshes every 60s, pauses when the tab is
  hidden, shows a connection status dot (live / stale / offline), and links to
  the original page if data can't load.
- **Installable** — add to your home screen for a full-screen, app-like view
  (PWA manifest + icon + iOS meta tags).

## Run it

It's a static site — no build step.

```bash
# any static server works; for example:
python3 -m http.server 8080
# then open http://localhost:8080
```

Or deploy the folder to **GitHub Pages**, Netlify, Cloudflare Pages, etc.

## How it gets data

The page reads the same JSON the official embeddable page uses:

```
https://www.weatherlink.com/embeddablePage/getData/<token>
```

Because that endpoint may not send permissive CORS headers, the page first
tries a **direct** request and then falls back to public CORS proxies
(configurable in `CONFIG.proxies` near the top of the script in `index.html`).
For best reliability, point it at your own proxy or host it on a domain you
control.

## Configuration

All settings live in the `CONFIG` object at the top of the `<script>` in
`index.html`:

| Key | Purpose |
| --- | --- |
| `token` | The WeatherLink share token (from the embeddable URL). |
| `refreshMs` | Auto-refresh interval. |
| `staleMinutes` | When to flag data as stale. |
| `proxies` | Ordered CORS-proxy fallbacks (set to `[]` for direct-only). |

The condition parser is defensive about field-name differences between
WeatherLink console/firmware versions, so it degrades gracefully and still
renders whatever sensors the station exposes.

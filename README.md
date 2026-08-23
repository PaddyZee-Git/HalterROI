# Halter Dairy ROI Calculator

A self-contained, mobile-friendly web app of the Halter NZ dairy ROI calculator.
Built with plain HTML/CSS/JS — no build step, no dependencies — so it is trivial
to host anywhere and to install on an iPad as a full-screen home-screen app.

The calculator itself (layout, copy, inputs, value drivers, and the underlying
maths) is reproduced exactly from the source Halter design.

## Files

| Path | Purpose |
| --- | --- |
| `index.html` | The calculator (the app entry point). |
| `tokens.css` | Halter design tokens — colours, type scale, spacing, `@font-face`. |
| `fonts/` | PP Neue Montreal typeface (all weights + italics). |
| `assets/hero-cattle-collar.jpg` | Hero photo (optimised for retina iPad). |
| `icons/` | App + home-screen icons and favicon. |
| `manifest.webmanifest` | PWA manifest for "Add to Home Screen". |
| `sw.js` | Service worker — caches the app so it works offline once loaded. |

## Run it locally

Because the app loads local fonts and an image, serve it over HTTP rather than
opening the file directly (some browsers block those requests on `file://`):

```bash
# from this folder
python3 -m http.server 8000
# then open http://localhost:8000
```

Opening `index.html` directly still works, but the offline service worker only
activates when served over HTTP(S).

## Host it

It is a static site — drop the whole folder onto any static host:

- **GitHub Pages** — Settings → Pages → deploy from this branch (root). The app
  is served at the repo's Pages URL.
- **Netlify / Vercel / Cloudflare Pages** — drag-and-drop the folder, or point
  at this repo. No build command; publish directory is the repo root.
- **Any web server** — copy the folder into the web root.

The service worker requires HTTPS (all the hosts above provide it), which every
option above satisfies.

## Put it on your iPad

1. Open the hosted URL in **Safari** on the iPad.
2. Tap the **Share** button → **Add to Home Screen**.
3. Launch it from the home-screen icon. It opens full-screen (no browser chrome)
   and, after the first load, keeps working without a connection.

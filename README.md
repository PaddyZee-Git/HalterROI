# Halter Dairy ROI Calculator

A self-contained, mobile-friendly web app of the Halter NZ dairy ROI calculator.
Built with plain HTML/CSS/JS — no build step, no dependencies — so it is trivial
to host anywhere and to install on an iPad as a full-screen home-screen app.

The calculator itself (layout, copy, inputs, value drivers, and the underlying
maths) is reproduced exactly from the source Halter design.

## Halter package pricing

In "Your farm", the Halter cost is driven by a **package** selector (Core / Pro /
Unlimited) and a **billing term** selector (Monthly / 12 months upfront / 24
months upfront), using Halter's Australian per-collar/month pricing (excl. GST):

| Package | Monthly | 12 mo upfront | 24 mo upfront |
| --- | --- | --- | --- |
| Core | $11.40 | $9.90 | $9.41 |
| Pro | $14.40 | $12.90 | $12.26 |
| Unlimited | $16.00 | $14.50 | $13.78 |

Selecting a package/term fills the per-collar price automatically. The price
field stays editable for a custom quote (flagged as "custom price" in the hint).

## Deployment cost

A "Deployment cost" section (after the value drivers) estimates the one-off cost
of deploying Halter, as three switchable components:

- **Towers** — number of towers × $5,700 per tower
- **Freight** — fixed $1,500 per deployment
- **Internet / connectivity** — number of properties × $1,440/year ($120/month)

These sum to a **Total deployment cost** shown in the results. Deployment is a
one-off setup cost, so it is reported separately and does **not** reduce the
annual net value (which stays a clean per-year figure).

## Saving meetings

Enter a **Meeting name** at the top of the inputs panel and tap **Save meeting**
to store the whole calculation — every input, driver toggle, and result — under
that title. Saved meetings appear in the **Saved meetings** list at the bottom,
where each can be **loaded** back (restoring all inputs) or **deleted**.

Meetings are stored on the device in the browser's `localStorage`, so they
persist across reloads and work offline, but they live only in the browser (or
installed app) they were saved in — they are not synced between devices.

## Printing / exporting a meeting

Tap **Print / PDF** (next to Save meeting) to produce a clean, one-page Halter
report of the current calculation — headline value, results, the value
breakdown, the farm inputs, and the assumptions behind each active driver. Each
saved meeting also has its own **Print** button.

This uses the browser's print dialog, so on an iPad you can **AirPrint** it or
choose **Save to Files** to export a PDF you can email or file. No calculation
data ever leaves the device.

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

# Halter Dairy ROI Calculator

A self-contained, mobile-friendly web app of the Halter NZ dairy ROI calculator.
Built with plain HTML/CSS/JS — no build step, no dependencies — so it is trivial
to host anywhere and to install on an iPad as a full-screen home-screen app.

The calculator itself (layout, copy, inputs, value drivers, and the underlying
maths) is reproduced exactly from the source Halter design.

## Milk solids per cow

"Your farm" includes an editable **Milk solids per cow** (kgMS/cow, default 450)
next to Milking cows. It feeds the Pasture utilisation → "Increased stocking
rate" option (extra cows carried × kgMS/cow × milk price); other options are
unaffected. That option also has an editable **Feed per extra cow** field
(annual kgDM demand per additional cow, default 5,000 kgDM ≈ SW Victoria), shown
only when the stocking-rate option is selected.

Below the field, a read-only **Herd production** caption shows total milk solids
(Milking cows × kgMS/cow) and approximate milk revenue (× milk price) — baseline
farm context that does not affect the ROI. It also prints in the report.

## Pasture lift (% ↔ kgDM/ha)

In the Pasture utilisation driver, "Additional pasture harvested" (kgDM/ha) is
linked to a **Pasture lift (%)** field. Enter a % lift and the kgDM/ha auto-fills
(% × DM harvested); edit the kgDM/ha and the % updates to match. The kgDM/ha
figure is what feeds the calculation, so changing "DM harvested" re-derives the
% from it.

## Calculation working ("How this is calculated")

Every value driver (pasture utilisation, pasture quality, reproduction,
lameness, labour, winter crop) shows a live "How this is calculated" line at the
bottom of its card that spells out the formula and numbers behind its figure, so
each result can be talked through transparently with the customer. The printed
report includes the working for every active driver under "How each value is
calculated".

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

## Vehicles & maintenance

A value driver estimating the saving from reduced reliance on farm vehicles.
Add vehicles as line items — **type** (Side-by-side, Quad / ATV, Motorbike,
Other), a **count**, the **hours of use per day**, and a **running cost
($/hr)**. Each vehicle's yearly cost is `hours/day × 365 × $/hr × count`, and
the running cost bundles depreciation, fuel and maintenance, pre-filled per
type (Side-by-side ≈ $16/hr, Quad ≈ $8/hr, Motorbike ≈ $4/hr — indicative
Australian benchmarks). Tap a vehicle's **Running cost** to open a popup that
breaks the $/hr into **depreciation**, **fuel** and **maintenance & repairs**;
edit any line and the total flows straight back into the calculation, or reset
it to the type default. The fleet's
total yearly cost × an editable "Halter reduces vehicle costs by %" gives the
annual saving, which feeds the profit. Each row shows its own working, and the
driver's working line shows the fleet cost per year and per month. Vehicles
persist in saved meetings and auto-resume.

## Other

A switchable value driver (under Vehicles & maintenance) for anything unique to
a farm that the standard drivers don't cover. Add **items** — each with a
**title**, a **calculation**, and whether it **adds value (+)** or is a
**cost (−)** — and tap **Add item** to keep adding more.

Each item has a live calculator field: write a sum such as `3000 - 1200 + 500`
and it evaluates to `= +$2,300` as you type, no need to press equals. On an
iPad you can write the sum straight into the field with the Apple Pencil —
iPadOS handwriting (Scribble) turns it into the digits and operators — or use
the large on-screen keypad (`7 8 9 ÷`, `4 5 6 ×`, …, with `⌫`, `C`, `( )` and
`=`). The evaluator supports `+ − × ÷`, parentheses, and decimals (a safe
expression parser — no `eval`).

The items net together and, when the driver is toggled on, feed the estimated
annual profit (which can go up or down). The driver value, each item's readout,
the working line, and the breakdown all show the signed figure, and the items
(titles and their sums) persist in saved meetings and auto-resume.

## Assumptions panel

A collapsible "Assumptions" section exposes the benchmark constants used by the
drivers so they can be adjusted per farm/region: 6-week in-calf value ($/cow per
1%), not-in-calf value ($/cow per 1%), MJME per kgMS, pasture direct profit
($/tDM), and winter-crop grow/buy rates ($/kgDM). Changes flow through the
calculations and the "how this is calculated" working lines, and persist in
saved meetings.

## Deployment cost

A "Deployment cost" section (after the value drivers) estimates the one-off cost
of deploying Halter, as three switchable components:

- **Towers** — number of towers × $5,700 per tower
- **Freight** — fixed $1,500 per deployment
- **Internet / connectivity** — number of properties × $1,440/year ($120/month)

These sum to a **Total deployment cost** shown in the results. Deployment is a
one-off setup cost, so it is reported separately and does **not** reduce the
annual net value (which stays a clean per-year figure).

## Meeting notes

A free-text **Notes** box under the meeting bar. Notes save with the meeting and
auto-resume, and print on the report (under a "Notes" heading) when present.

## New meeting

A "New meeting" button in the meeting bar resets every section back to defaults —
like a fresh document — after a confirm. It clears the current working state and
reloads; saved meetings are kept.

## Auto-resume

The app remembers your current working state on the device and restores it when
reopened, so you pick up exactly where you left off — every input, package/term,
driver toggle, deployment setting, and the meeting name. This is separate from
Saved meetings (named snapshots): it is a single, always-updating "last session"
stored per device in `localStorage`.

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

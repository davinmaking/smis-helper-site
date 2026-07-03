# SMIS Helper Site

## Project Overview
Public landing + tutorial site for the SMIS Helper Chrome Extension. Static HTML hosted on Cloudflare Pages at `https://smis.davinhub.com`. Auto-deploys on push to `main`. Audience: Malaysian teachers (BM-first, EN secondary).

Pages:
- `index.html` — hero / features / pricing / FAQ
- `panduan.html` — step-by-step tutorial (Bahagian 1-4 published, Bahagian 5 pending)
- `success.html` — post-payment landing (auto-polls license key, shows Telegram community)
- `privacy.html` — privacy policy

## Stack
- **Pure static HTML/CSS/JS** — no framework, no build step, no Node deps. Just files.
- **Hosted:** Cloudflare Pages project `smis-helper-site` under davinmaking account
- **Auto-deploy:** push to `main` → Pages picks up + deploys in ~1 minute
- **Custom domain:** `smis` CNAME → `smis-helper-site.pages.dev` in Cloudflare DNS for `davinhub.com` zone
- **Legacy fallback:** `davinmaking.github.io/smis-helper-site` still works as backup

## Local Preview
```bash
cd /Users/davins/Developer/smis-helper-site
python3 -m http.server 8765
open http://localhost:8765/index.html
open http://localhost:8765/panduan.html
```

(Or use `npx serve .` if you prefer Node-based.)

## Pricing (index.html) — static RM59, NO promo logic
Pro is a flat **RM59 sekali bayar**. Shown in 3 places: promo strip, Pro card (`#proPrice`), pay modal (`#modalSub`). **Payment rail = Billplz (since 2026-07-03; ToyyibPay retired):** the on-site BM modal (name+emel, NO phone) POSTs `/api/license-purchase` with `slug:"smis-helper-pro"` + `returnUrl:"https://smis.davinhub.com/success"` (server-side allowlist) → Billplz FPX → back to our `success.html`, which polls `/api/license-purchase/status?ref=` for the key. The BM funnel stays entirely on this site — davinhub's zh store sells the same product at `/store/buy/smis-helper-pro` for the Chinese audience; both share one rail.

⚠️ **Gotcha:** a previous time-bound promo (`PROMO_END` + `applyPromoState()`) silently flipped the LIVE price to RM99 when the date passed (2026-06-09 incident — site quoted RM99 while ToyyibPay billed RM59). Removed. Keep pricing as **static markup only** — no date/JS price logic. The planned RM99 "normal" price was abandoned; RM59 is permanent.

## Version pills — manual bump per release
Three hardcoded `SMIS Helper v2.1.X` pills in the hero mockups (`index.html:746, 811, 911`) + the JSON-LD `softwareVersion` do NOT auto-update. Bump all of them to match the CWS-live version each time a new build publishes (e.g. → `v2.1.3` once it's approved).

## Lighthouse Targets — all 100/100/100 (verified 2026-05-04)
| Page | Mobile | Desktop |
|---|---|---|
| `index.html` | a11y 100 / BP 100 / SEO 100 | same |
| `panduan.html` | 100 / 100 / 100 | same |
| `success.html` | 100 / 100 / 100 | same |
| `privacy.html` | 100 / 100 / 100 | same |

Re-run with Chrome DevTools → Lighthouse tab. Don't accept regressions: any page that drops below 100 needs fixing before push.

## Panduan Structure (panduan.html, 1100+ lines)

Long single-page tutorial. Each step is a self-contained `<article class="step">` block. Steps grouped into `<section class="section" id="aliran-asas">` etc. by Bahagian.

### Adding a new step
1. Inside the relevant `<section>`, add a new `<article class="step" id="step-X-Y">`
2. Pattern:
   ```html
   <article class="step" id="step-3-8">
     <div class="step-header">
       <span class="step-num">8</span>
       <div>
         <p class="step-tag">Muat naik gambar pukal</p>
         <h3 class="step-title">Upload gambar peserta dari folder</h3>
       </div>
     </div>
     <div class="step-body">
       <p>...</p>
     </div>
     <figure class="step-media">
       <img src="panduan-img/aliran-08-gambar-pukal.png" alt="...">
     </figure>
     <div class="step-callout">
       <div class="callout is-tip"><span class="callout-icon">💡</span>...</div>
     </div>
   </article>
   ```
3. Add a corresponding entry to TOC sidebar (`<ol class="toc-sections">` `<ul class="toc-substeps">`) at the top
4. Bottom-sheet TOC (mobile) clones the sidebar via JS — no separate update needed
5. Verify all anchors resolve: `document.querySelectorAll('.toc-list a[href^="#"]').forEach(a => { if (!document.getElementById(a.getAttribute('href').slice(1))) console.log('broken:', a) })`

### Placeholder pattern (for steps awaiting screenshots)
```html
<figure class="step-media">
  <div class="step-media-placeholder">
    <strong>Skrinsut akan ditambah</strong>
    Description of what should be shown
  </div>
</figure>
```

CSS class `.step-media-placeholder` already exists. Replace with `<img src="panduan-img/...">` when assets ready.

### Callout variants
| Class | Color | Use |
|---|---|---|
| `callout` (default) | Neutral gray | Info / context |
| `callout is-tip` | Amber bg | Pro tip |
| `callout is-warn` | Red bg | Warning / caution |
| `callout is-ok` | Green bg | Success / confirmation |

## panduan-img/ Asset Conventions

- **Screenshots:** PNG, ideally <500KB each. Naming pattern: `aliran-NN-description.png` for Bahagian 3, `sedia-NN-description.png` for Bahagian 4
- **Videos:** MP4 H.264. Always compress before commit:
  ```bash
  ffmpeg -i input.mp4 -vcodec libx264 -crf 28 -preset medium -an -movflags +faststart output.mp4
  ```
  (`-crf 28` quality, `-an` strip audio, `-movflags +faststart` allow streaming before download finishes.)
- **HTML embed:**
  ```html
  <video src="panduan-img/aliran-07-upload-daftar.mp4"
         controls muted playsinline preload="metadata"
         aria-label="Demo description"></video>
  ```
  `preload="metadata"` so browsers only fetch the first few KB until user clicks play.

## OG Image Regen (`og-image.png`, 1200×630)

The OG image isn't hand-drawn — it's rendered from an HTML/CSS template + Chrome screenshot + sips downscale. Re-render the same way when design/copy changes:

1. Build HTML version of the OG layout (typically saved as `og-image-template.html` during the design pass)
2. Open in Chrome at exactly 2400×1260 (2x for retina)
3. Take full-page screenshot via DevTools or CleanShot
4. Downscale: `sips -z 630 1200 og-image-screenshot.png --out og-image.png`

## Bottom-Sheet TOC FAB (mobile)

`panduan.html` has a floating ≡ FAB on mobile (<1024px) that opens a bottom-sheet TOC for jumping anywhere in the long doc. Sidebar TOC stays visible on desktop.

- FAB markup: `<button class="fab toc-fab" id="tocFab">`
- Sheet markup: `<div class="toc-sheet" id="tocSheet">`
- TOC content cloned from `.toc-list ol.toc-sections` → injected into `#tocSheetBody` via `cloneNode(true)` on first open. Scroll-spy syncs both DOMs.

## SEO Assets

- `robots.txt` — allows crawl, disallows `/admin` (which is on davinhub.com anyway, defensive)
- `sitemap.xml` — manually maintained list of pages
- `og-image.png` — see above
- `index.html` JSON-LD: `FAQPage` schema with 6 entries that **mirror** the 6 `.faq-item` blocks. Re-sync when FAQ copy changes, otherwise Google rich results drift from the page.

## Telegram Channel Reference

Public channel for SMIS Helper / Cikgu Davin brand: `https://t.me/cikgudavin` (BM/EN audience). Pro Group invite: `https://t.me/+r_eS74BFGwI2M2Vl`. Switched from `@mrdavinhub` 2026-05-05; zh-locale content on davinhub.com still uses `@mrdavinhub`.

## Cross-Repo Reference

- Extension repo: `/Users/davins/Developer/smis-helper/` (private — see its CLAUDE.md for SMIS API details, KAUM gotchas, etc.)
- Promo video repo: `/Users/davins/Developer/smis-helper-promo/`
- Backend (license + payment): davinhub repo — admin at `https://davinhub.com/admin`

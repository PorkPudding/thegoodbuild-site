# The Good Build — thegoodbuild.shop

## What this is

The public website for **The Good Build**, Roman's custom gaming PC business in
**Corona, California**. Static single-page site hosted on **GitHub Pages**
(CNAME: `thegoodbuild.shop`). Everything lives in one `index.html` — inline CSS
and JS, no build step, no framework. Keep it that way unless Roman asks otherwise.

## The business

- One-person operation: Roman sources parts, builds, cleans, stress-tests, and
  stands behind every machine personally. Building and selling since 2023,
  500+ builds completed.
- Builds use **refurbished components** — fully disassembled, cleaned, tested,
  stress-tested. Always disclosed honestly.
- Every build ships with clean Windows 11 Pro, no bloatware.
- 30-day return window, 90-day hardware warranty, free ongoing tech support.
- Trade-ins of old PCs accepted toward purchases.

## Current direction (July 2026) — IMPORTANT

The site **no longer sells through Shopify**. It is a **local advertising /
lead-generation site**: showcase the builds, build trust, get local buyers to
reach out. Concretely:

- **No checkout, no Buy Buttons, no Shopify Storefront API.** Do not reintroduce.
- **Local-only**: Corona, CA and surrounding area (Inland Empire / Orange
  County). No nationwide-shipping messaging.
- Buyers contact Roman via the **contact form**, email, or Instagram DM —
  then meet locally to pay and pick up.

## Data architecture

Build listings are data, not markup:

- `builds.json` — current source of truth for listings. Each build:
  `id, title, tagline, description, tier, price, status ("available"|"sold"),
  images[], specs[{label,value}], extras[]`. Photos live in `images/builds/`.
- `index.html` fetches `builds.json` and renders cards; `status:"available"`
  builds show under "Available now", `status:"sold"` under recent builds
  (social proof).
- **Planned**: Roman has a separate private repo, `PorkPudding/thegoodbuild-dashboard`
  (Vite PWA on Vercel at thegoodbuild.vercel.app, Google Sheet backend, has a
  publish workflow). The plan is a public read-only endpoint there
  (e.g. `/api/public-listings`) returning JSON in the same shape as
  `builds.json`; the site's `CONFIG.LISTINGS_URL` in `index.html` switches the
  data source to it. An `/api/inquiry` endpoint may later receive contact-form
  posts (`CONFIG.INQUIRY_ENDPOINT`); until then the form falls back to mailto.

## Brand voice

Plain-spoken, confident, a little wry. "One builder. No BS." Anti-corporate:
no call centers, no assembly lines, no bloatware. Never overpromise; honesty
about refurbished parts is a selling point. Avoid hypey marketing language.

## Contact channels

- Email: support@thegoodbuild.shop
- Instagram: @romans_goodbuild · TikTok: @thegoodbuild
- Location: Corona, CA (local pickup/meetup)

## Working notes

- Design system: navy/blue palette, DM Sans + Outfit fonts, CSS variables in
  `:root`. Match it for anything new.
- The site is plain static hosting — any "backend" behavior must live in the
  dashboard's Vercel functions, not here.
- Compress photos before committing (target ≲150KB each, max ~1400px).
- Old Shopify product data was archived to `builds.json` on 2026-07-08 before
  cancelling Shopify; the extracted raw copy is `shopify_builds_extracted.json`
  (safe to delete once builds.json is trusted).

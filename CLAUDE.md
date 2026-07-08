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

- **LIVE (since 2026-07-08): the dashboard is the source of truth.**
  `CONFIG.LISTINGS_URL` in `index.html` points at
  `https://thegoodbuild.vercel.app/api/public-listings` — a public read-only
  endpoint in Roman's private `PorkPudding/thegoodbuild-dashboard` repo (Vite
  PWA on Vercel, Google Sheet backend). It returns JSON in exactly the
  `builds.json` shape: `available` cards are builds Roman explicitly published
  to the "Website" target in his dashboard's Publish modal; `sold` cards come
  from his curated portfolio (newest-first, capped server-side at 12). Photo
  URLs are absolute (Vercel Blob). The feed is CDN-cached ~5 min
  (s-maxage=300, SWR 600), so dashboard changes appear within minutes.
- `builds.json` stays in this repo as the shape's documentation AND the
  automatic fallback: the renderer falls back to it if the endpoint is
  unreachable. Don't delete it. Each build:
  `id, title, tagline, description, tier, price, status ("available"|"sold"),
  images[], specs[{label,value}], extras[]`.
- `status:"available"` builds show under "Available now", `status:"sold"`
  under recent builds (social proof).
- The contact form POSTs to `CONFIG.INQUIRY_ENDPOINT`
  (`https://thegoodbuild.vercel.app/api/inquiry`) → lands in an "Inquiries"
  tab of Roman's dashboard Sheet. On ANY failure (non-2xx, network) the form
  falls back to the prefilled mailto — never remove that fallback. The form
  includes a hidden `company` honeypot field (position:absolute off-screen);
  the server silently swallows submissions where it's non-empty. Keep the
  field name `company` in sync with the dashboard's inquiry validator.

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
- Shopify history: the store's product data was archived into `builds.json` on
  2026-07-08, the subscription was cancelled the same day, and the raw extracted
  copy (`shopify_builds_extracted.json`) was deleted once the dashboard feed was
  live. Photos for those builds live on in `images/builds/`.

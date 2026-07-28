# Dapur Ayah — Gerai Pasar Tani

Single-page site for Dapur Ayah, a pasar tani food stall in Skudai & sekitar (Johor). RM7 flat-price meals, WhatsApp ordering, bulk/catering orders, and a weekly pasar tani schedule.

## Stack

Plain HTML/CSS/JS — no build step, no dependencies. Everything (styles, markup, and the small theme-toggle / today-banner script) lives in `index.html`.

## Running locally

Just open `index.html` in a browser, or serve it statically:

```bash
python3 -m http.server 8000
```

Then visit `http://localhost:8000`.

## Deploying

This repo is set up for **GitHub Pages**:

1. Push to `main`.
2. In repo Settings → Pages, set source to `main` branch, `/ (root)`.
3. Site will be live at `https://<username>.github.io/dapur-ayah/`.

No CI/build pipeline is needed since there's nothing to compile.

## Editing content

Most content lives in the `<script>` at the bottom of `index.html`:

- **Menu & prices** — the `MENU` array. The menu board and the order-form
  item pickers are both rendered from it, so edit in one place.
- **Weekly schedule** — the `SCHEDULE` array (`day`: 0=Ahad … 6=Sabtu). It
  drives the schedule table, the "today" highlight, the live open/closed
  banner, and the pickup-location dropdowns in the order form.
- **WhatsApp number** — the `WA` constant (and search `60197309787` for the
  plain `tel:`/`wa.me` links in the markup).

## Order form

Clicking any "Order" button opens a modal with three modes — **Biasa**
(retail), **Borong** (bulk, min. 10), and **Katering** (event enquiry). It
builds a formatted WhatsApp message and opens `wa.me`; nothing is stored or
sent server-side.

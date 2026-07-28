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

- Menu items: `.menu-grid` section in `index.html`.
- Weekly schedule: `#scheduleTable` rows (each `<tr>` has `data-day` (0=Ahad..6=Sabtu) and `data-map` attributes).
- WhatsApp number: search for `60197309787` and replace all occurrences.

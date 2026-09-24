# Dapur Ayah — Gerai Pasar Tani

Single-page site for Dapur Ayah, a pasar tani food stall in Skudai, Kulai & sekitar (Johor). RM7 flat-price meals, WhatsApp ordering, bulk/catering orders, and a weekly pasar tani schedule.

## Stack

Plain HTML/CSS/JS — no build step, no dependencies. Everything (styles, markup, and the small script) lives in `index.html`.

The site has one warm off-white theme. There is no dark mode or theme toggle.
Colours come from the CSS custom properties on `:root` at the top of the
`<style>` block, so change them there. Fonts come from Google Fonts: **Archivo** for
headings and prices, **Figtree** for everything else.

## How the page is ordered

Sections follow what visitors come to find out, in this order:

1. **Hero** — four things only: the owners' photo, "Semua RM7 je.", one
   line of what we cook, and a live **"are you open?" card** (`#nowCard`).
   The card shows when and which market, and tapping it opens Google Maps.
   Please don't add more here; everything else is one scroll away.
2. **Menu** (`#menu`) — filtered by market day. It defaults to today's market, or the next
   one if today's is over or it's a day off.
3. **Jadual** (`#jadual`) — the week's markets, each with an **Arah**
   (directions) link.
4. **Borong & Katering** (`#tempah`) — the two order types plus the 3 steps.
5. **Ulasan** (`#ulasan`), **Cerita** (`#cerita`), **Hubungi** (`#hubungi`).

On phones a sticky bar at the bottom holds WhatsApp and the order button.
That's why the hero's own buttons are hidden below 820px.

## Design rules

The top of the `<style>` block lists the small set of parts the whole page uses.
Reuse them rather than adding one-off styles:

- **Headings** — one style. h1, h2 and h3 differ only in size.
- **Text** — four sizes: `--t-lead`, `--t-body`, `--t-sm`, `--t-xs`.
- **Corners** — `--r-lg` for cards and photos, `--r` for buttons, fields and tiles, and pills for chips.
- **`.card`** — the only surface.
- **`.chip`** — the only badge (neutral, `.chip-green` or `.chip-red`).
- **`.tile`** — the only icon box.
- **Buttons** — red opens the order form, green opens WhatsApp, and outline is used for everything else.
- **Selected state** — a dark ink fill, the same for the day filter and the order-form tabs.

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
  item picker are both built from it, so edit in one place. Prices other than
  RM7 get a red chip automatically. Items can carry an optional `days` array
  (0=Ahad … 6=Sabtu) to restrict them to specific market days. The restriction
  is shown as a tag and enforced (disabled + reset) in the Borong picker.
- **Weekly schedule** — the `SCHEDULE` array (`day`: 0=Ahad … 6=Sabtu). It
  drives the schedule list, the "hari ini" highlight, the hero's live
  open/closed card, the "Isnin & Sabtu kami cuti" line, and the pickup
  dropdown in the order form. Keep `time` in the form `3:00–7:30 PM`: the
  open/closed logic parses it, and the start takes the end's AM/PM unless it
  has its own. The `<li>` rows in the `#sched` markup are only a no-JS fallback.
  JS replaces them, but it's still worth keeping them roughly in sync.
- **WhatsApp number** — the `WA` constant. Also search for `60197309787`
  to find the plain `tel:`/`wa.me` links in the markup.

## Reviews (`#ulasan`)

Customer reviews are plain markup: three `<figure class="review">` blocks,
each with a tag, a `<blockquote>`, and a name/role with a coloured initial
avatar. To add or edit one, copy an existing block. The avatar colour cycles
automatically off `:nth-child`. Under 900px the grid becomes a swipeable
snap rail with dots, so keep the quotes to roughly similar lengths. Don't add
reviews that weren't actually given by a customer.

The three fact tiles (`.facts`) in the story section give the year, the number
of markets per week (filled from `SCHEDULE`), and the borong minimum. Keep
them in sync if the business changes.

## Order form

Every "Tempah" button opens a sheet that first asks **Borong** (bulk, min. 10)
or **Katering** (event enquiry). Once one is picked, a two-tab switch appears.
The form builds a formatted WhatsApp message and opens `wa.me`. Nothing is
stored or sent server-side. Validation errors show inline above the send
button, and the Borong running total sits in the sheet footer. The date picker
starts from tomorrow ("tempah sehari awal").

**Single retail orders are paused.** The opening state (`mode==="biasa"`) is
a chooser that tells walk-in customers to visit the stall instead. To
re-enable single orders, add a tab for it and remove the early-return guard in
`buildMessage()`.

## Photos

`images/` holds three real photos:

- `pasartani2.jpg` — the hero (owners at the counter).
- `pasartani1.jpg` and `pasartani4.jpg` — both 3:4 portraits, shown as a
  pair in the story section.

To swap a photo, replace the file (keep the same name and aspect) or update
the `src`/`alt`. Images are pre-resized/compressed with `sips` (max
~900–1000px wide, ~76–78% JPEG quality) to keep the page light. Do the same
for any replacement before committing.

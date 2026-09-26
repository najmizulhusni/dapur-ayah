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
4. **Borong & Katering** (`#tempah`) — the page's one red block, because
   bulk and catering orders are what the site is for. Two white option cards,
   each with a short tick list and one button.
5. **Ulasan** (`#ulasan`) and **Tentang Kami** (`#tentang`).
6. **Hubungi** (`#hubungi`) — the dark footer: phone, WhatsApp, areas.

On phones a sticky bar at the bottom holds WhatsApp and the order button.
That's why the hero's own buttons are hidden below 820px.

## Design rules

The page should read like the stall's own notice board, not a website
template. The top of the `<style>` block lists the small set of parts the page
uses. Reuse them rather than adding one-off styles:

- **Section names** — plain labels ("Menu", "Jadual pasar tani"), not
  questions or slogans. "Semua RM7 je." is the one slogan on the page.
- **Headings** — one style. h1, h2 and h3 differ only in size.
- **Text** — four sizes: `--t-lead`, `--t-body`, `--t-sm`, `--t-xs`.
- **Layout** — plain sections share one background with a hairline between
  them. Only two sections get colour: the red order block and the beige
  reviews band. Don't add more, or nothing stands out.
- **Cards** — for things that belong together: the menu board, the status
  card, the two order options, each review. Everything else sits on the page.
- **`.chip`** — the only badge (neutral or `.chip-green`). It says who or
  what a card is for, e.g. "Ambil sendiri di gerai" or "Tempahan surau".
- **Buttons** — red opens the order form, green opens WhatsApp, and outline is used for everything else.
- **Selected state** — a dark ink fill, the same for the day filter and the order-form tabs.
- **Copy** — short and plain. No em dashes in visible text; use a comma or a
  full stop.

Avoid the things that make a small-business page look generated: icons in
rounded squares, stat tiles, numbered "how it works" steps, and question
headings.

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
3. Site will be live at `https://najmizulhusni.github.io/dapur-ayah/`.

If the address changes (for example, a custom domain), update the absolute
URLs in `<head>`: `canonical`, `og:url`, `og:image` and the JSON-LD block.
WhatsApp and Facebook link previews only show the photo when `og:image` is a
full URL.

No CI/build pipeline is needed since there's nothing to compile.

## Editing content

Most content lives in the `<script>` at the bottom of `index.html`:

- **Menu & prices** — the `MENU` array. The menu board and the order-form
  item picker are both built from it, so edit in one place. Prices other than
  RM7 are set in red automatically. Items can carry an optional `days` array
  (0=Ahad … 6=Sabtu) to restrict them to specific market days. The restriction
  is shown as a tag and enforced (disabled + reset) in the Borong picker.
- **Weekly schedule** — the `SCHEDULE` array (`day`: 0=Ahad … 6=Sabtu). It
  drives the schedule list, the "hari ini" highlight, the hero's live
  open/closed card, the "Isnin & Sabtu kami cuti" line, and the pickup
  dropdown in the order form. Keep `time` in the form `3:00–7:30 PM`: the
  open/closed logic parses it, and the start takes the end's AM/PM unless it
  has its own. The `<li>` rows in the `#sched` markup are only a no-JS fallback.
  JS replaces them, but it's still worth keeping them roughly in sync. The
  `openingHoursSpecification` in the JSON-LD block in `<head>` repeats the
  hours for Google, so update it too. The copy says "lima pasar" in two
  places (the Jadual intro and Tentang Kami); change those if the number of
  markets changes.
- **WhatsApp number** — the `WA` constant. Also search for `60197309787`
  to find the plain `tel:`/`wa.me` links in the markup.

## Reviews (`#ulasan`)

Customer reviews are plain markup: three `<figure class="review">` cards.
Each has a chip saying who the customer is (surau, kenduri, regular), the
`<blockquote>`, and the name with an initial. Wrap the one sentence that best
answers "can they handle my order?" in `<strong>`, so the card can be read at a
glance. To add one, copy an existing card.

They sit in three columns on desktop. On a phone they become one swipeable row
with dots, so keep the quotes to roughly similar lengths. Keep the customer's
own wording. Don't add reviews that weren't actually given by a customer.

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
  pair in Tentang Kami. They're cropped to 4:5 so the camera's date stamp on
  `pasartani1.jpg` stays hidden.
- `icon-180.png` — the home-screen icon (`apple-touch-icon`).

To swap a photo, replace the file (keep the same name and aspect) or update
the `src`/`alt`. Images are pre-resized/compressed with `sips` (max
~900–1000px wide, ~76–78% JPEG quality) to keep the page light. Do the same
for any replacement before committing.

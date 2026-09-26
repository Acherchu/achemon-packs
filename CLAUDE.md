# Ache-mon Packs

**Live at https://acherchu.github.io/achemon-packs/** — GitHub Pages off `main` in
[Acherchu/achemon-packs](https://github.com/Acherchu/achemon-packs). Pushing to `main` redeploys it;
a build takes about a minute.

A shop site for Archer's handmade Pokémon booster packs (real cards inside). Six **type packs**,
$9 and 7 cards each, and each one only holds cards of its type:
Flames (001, Fire), Aqua (002, Water), Nature (003, Grass), Shadow (004, Dark), Neutral (005, Normal),
Gear (006, Steel). The old Umbreon and Charizard packs were removed on request.
Plus the **Micaiah Pack** (007, any type, rainbow `micaiahArt`), which is **free** (`price: 0`): Stripe
can't charge under $0.50, so it has no payLink. Free packs show "Free", never get a Pay button, and
a cart with a free pack doesn't auto-jump to Stripe (the done screen explains the free pack). Free
orders only reach Archer once an orders inbox (`SHOP.orders`) is set up. A 1¢ "micaiah pack"
product exists in the Stripe sandbox but is unused.

Each pack's foil/trim colors come from its `theme` in `PRODUCTS` (CSS variables `--acc-rgb`,
`--f1`…`--f5`, glows, sparkles; defaults in `:root` are Umbreon's). Art is a hand-built SVG function
per pack. They're all **emblems, not Pokémon** (`flamesArt`, `aquaArt`, `natureArt`, `shadowArt`,
`neutralArt` share the `elementalArt(u, opts)` builder — background, halo, dust, emblem; `gearArt`
draws generated metal gears).

The back of every pack is **plain foil** on purpose: Archer asked for the rarity rows, the card
pictures, the pull rates and the card close-up to be removed (they existed earlier — don't add them
back unless asked).

## Files

- `index.html` — the entire site. No build step, no dependencies. Double-click it and it runs
  (card pictures load from images.pokemontcg.io, so those need internet).

## How it's organized (all in `index.html`)

- **`SHOP`** and **`PRODUCTS`** at the top of the script are the things to edit: shop name, order
  email (`orderEmail`, empty = buyers copy their order instead), and each pack's price, card count,
  contents, `pulls` (rarity groups with `odds` and card pictures) and `art` (an SVG function).
  `hidden: true` keeps a pack off the shop.
- **`umbreonArt(u)`** — hand-built SVG sticker art. `u` prefixes every id so several packs can share
  a page. Umbreon is one connected silhouette (`cSil` clip + `ink` filter for outline/rim light);
  keep limb tops overlapping the body or seams show.
- **`boxHTML(p)`** — the 3D crimp-sealed foil pack (front/back bodies, sides, edges, crimps).
  The back shows the `pulls` rows.
- Hash routes: `#/` shop grid, `#/pack/<id>` pack page.
- Pack page: drag to spin; click the pack to zoom (fixed-position `.stage.zoomed`); while zoomed,
  wheel / pinch / + − zoom toward the cursor and clicking a card on the back opens the card close-up
  (`#zoom` dialog). There is no "open pack" feature (removed on purpose).
- Cart is `{productId: qty}` in `localStorage`.
- **Checkout is deliberately minimal** (Archer: "as quick as possible"): one screen with total +
  6 required fields (name, email, address, city, state, ZIP) → Place order → pay/confirmation.
  No optional fields, no review step — don't add phone/apartment/country/note back unless asked.
  Per-field validation, details remembered in `localStorage.buyer`, flat `SHOP.shipping`, order
  number `AM-YYYYMMDD-XXXX`.
  - `SHOP.orders.googleForm` (action + `entry.*` field ids) or `SHOP.orders.formspree` decides where
    orders land — **both still empty**, so orders currently only show as copyable text.
    Google Form setup: make a form with long-answer questions (order, name, email, phone, address,
    note) → Send → link → the `/viewform` URL becomes `/formResponse`; entry ids come from the
    page source (`entry.123456789`). Posted with `mode:'no-cors'`, so failures are silent.
  - **Payments: each pack has its own Stripe Payment Link** (`PRODUCTS[].payLink`). They are
    **TEST-mode links** (`buy.stripe.com/test_…`) from Archer's Stripe *sandbox* — no real money moves
    until a parent activates the live account and live links replace these. Checkout shows one
    green Pay button per kind of pack (a one-kind cart skips that screen and goes **straight to
    Stripe** — "Continue to payment"), adds `prefilled_email` + `client_reference_id` (order number)
    to the Stripe URL, and tells the buyer to set the quantity on Stripe's page (Payment Links can't
    preset quantity). Links were made with adjustable quantity + billing & shipping address (US only),
    no shipping rates (free shipping). `SHOP.payLink` is only a fallback.
  - **The links use manual capture** (`payment_intent_data[capture_method]=manual`): a purchase only
    *holds* the card; it shows as **Uncaptured** in Stripe → Payments, and Archer clicks Capture
    (charge + ship) or Cancel. Holds expire after ~7 days. The dashboard UI can't set this, so the
    links (…EI06–EI0b) were made in Workbench → Shell with `stripe payment_links create … -d
    "payment_intent_data[capture_method]=manual"` (price IDs: `stripe prices list`). The first set
    (…EI00–EI05, auto-capture) is no longer used by the site.
  - Never add real card fields to this site; it's a static page with no server.

## Running it

Preview server `pack-shop` in `C:\Users\arche\.claude\launch.json` (port 8084).

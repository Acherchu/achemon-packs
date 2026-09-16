# Ache-mon Packs

**Live at https://acherchu.github.io/achemon-packs/** — GitHub Pages off `main` in
[Acherchu/achemon-packs](https://github.com/Acherchu/achemon-packs). Pushing to `main` redeploys it;
a build takes about a minute.

A shop site for Archer's handmade fan-made Pokémon booster packs. Right now it sells one pack, the
**Umbreon Pack** ($9, 7 cards), but it's built for several.

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
- Cart is `{productId: qty}` in `localStorage`; checkout is an order request (no payments).

## Running it

Preview server `pack-shop` in `C:\Users\arche\.claude\launch.json` (port 8084).

# Ache-mon Packs

**Live at https://acherchu.github.io/achemon-packs/** — GitHub Pages off `main` in
[Acherchu/achemon-packs](https://github.com/Acherchu/achemon-packs). Pushing to `main` redeploys it;
a build takes about a minute.

A shop site for Archer's handmade Pokémon booster packs (real cards inside). It sells the
**Umbreon Pack** (No. 001) and the **Charizard Pack** (No. 002), $9 and 7 cards each.

Each pack's foil/trim colors come from its `theme` in `PRODUCTS` (CSS variables `--acc-rgb`,
`--f1`…`--f5`, glows, sparkles; defaults in `:root` are Umbreon's). Art is a hand-built SVG function
per pack (`umbreonArt`, `charizardArt`). Charizard's pulls are the Charmander line (Charmander
commons, Charmeleon uncommons, Charizard rares/legendaries) with TCGdex image URLs (`img` + `big`).

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

# 面板資料庫｜戀與深空戰鬥討論群

This project is a battle stats database website for 「戀與深空戰鬥討論群」. It collects Orbit and Endless Challenge battle stats, allowing players to search, filter, save, and review battle records more efficiently.

Website: https://ladsbattle.github.io/Love-and-Deepspace-Battle-Stats-tw/

The battle stats are collected from 「戀與深空戰鬥討論群_臉書面板分享專區」 and maintained through Google Sheets. The website reads published CSV data directly from the spreadsheet.

## Features

- Orbit search with quick level ranges, exact levels and manual level input
- Endless Challenge search through clickable character buttons and companion portraits
- Advanced condition filters with independent reset controls
- Video-only filtering
- Search-condition sharing and restoration
- Report form for incorrect or missing data
- Personal folder with favorites and browsing history
- Version History and database Last Updated timestamp
- Contributor acknowledgment list maintained through Google Sheets

## Data Entry Rules

The parser reads columns by position rather than by header name. Include one header row in each worksheet and keep the following column order unchanged.

### Orbit

```text
Orbit → Level → Upper Card → Upper Companion → Upper Stella → Lower Card → Lower Companion → Lower Stella → Has Video → Link → 星數
```

- **Level configuration:** In Fluctuation Orbit (開放波動), levels that are multiples of 5 have both an upper and a lower configuration. In other Orbits, this applies to levels that are multiples of 10. All remaining levels have a single configuration.
- **Partial records:** If only one configuration is submitted for a level that has both, enter it in the corresponding Upper or Lower columns. Leave the missing configuration's fields blank or mark its Card as `N/A`.
- **Clear stars (column K, `星數`):** For new Fluctuation Orbit records, enter the confirmed clear rating as a number from `0` to `3`. Use `0` only for a confirmed zero-star clear. Leave unknown ratings and all other Orbits blank. Keep this column immediately after `Link`.

### Endless Challenge

```text
Companion → Card → Stella Match Count → Score → Has Video → Link
```

When adding or editing records:

- Keep every record on a single CSV line. Do not insert line breaks inside cells.
- Use the companion's short name without the LI's name. Keep its spelling, punctuation and spacing identical across all records.
- Keep card names and Stella labels consistent with existing records. Do not create alternate spellings for the same option.
- Enter `TRUE` in `Has Video` only when the record includes a video. Every other value is treated as false.
- Enter Stella Match Count and Score as numeric values without descriptive text.
- Make sure each Link points to the corresponding battle record.
- New rows may be appended anywhere in the worksheet. Website display order is controlled by `js/catalog.js`, not by the worksheet row position.

## Catalog and Display Order

`js/catalog.js` is the single source of truth for active Orbits, maximum levels, character order, all companion ownership, rarity and order, portrait paths and result sorting. Each character and their matching directional Orbit share one entry in `CHARACTER_RELEASES`; changing that single flag controls both modes. Endless Challenge companion options are derived automatically from companions whose `rarity` is `5`.

- Companions within each character are sorted by rarity first (five-star before four-star). Five-star companions follow their explicit catalog `order`, which records release order; four-star companions sort automatically with English names first, followed by Traditional Chinese stroke order.
- Orbit results: Orbit order → Level ascending → T1 Card → T2 Card → video availability as the final tie-breaker. T1/T2 Companions and Stella labels do not affect display order.
- Endless Challenge results: Character → Companion → Stella Match Count ascending → Card → Score ascending → video availability as the final tie-breaker.
- Card order: No Set → Rank 0 → Rank 1 → Rank 2 → Rank 3.
- Favorites retain the player's save order. Browsing History remains newest first.

When adding a character or companion, update `js/catalog.js` and add the matching portrait. Do not duplicate companion names in `app.js` or `styles.css`.

## Deployment

This project is deployed with GitHub Pages.

Upload the contents of this deployment folder (`outputs/` in the local workspace) to the repository root while preserving the `css/`, `js/` and `assets/` directory structure. Do not upload an extra enclosing `outputs/` or `split/` folder.

- Include `index.html`, `css/`, `js/` (including `catalog.js`), `assets/`, `README.md`, `VALKO-LAUNCH.md`, `robots.txt` and `sitemap.xml`.
- When CSS or JavaScript changes, update the corresponding `?v=` value in `index.html`, then upload the changed asset and `index.html` together.
- Keep the canonical URL, `og:url`, README website link, sitemap URL and robots sitemap declaration pointed at the production website.
- Update the sitemap `lastmod` for meaningful website releases. Website Version History is maintained separately in Google Sheets.
- After GitHub Pages finishes deploying, verify both modes on desktop and mobile, character and companion selection, manual level input, advanced filters, search-condition sharing, Contributors, and the detail views.
- Favorites and browsing history are stored in the player's browser under `ladsbattle_local_folder_v1`. Ordinary deployments must not rename this key or require players to clear site data.

## Notes

Valko is not enabled yet, but his place in this project is reserved. The integration and release checklist is maintained in [VALKO-LAUNCH.md](VALKO-LAUNCH.md).

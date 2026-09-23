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
Orbit → Level → Upper Card → Upper Companion → Upper Stella → Lower Card → Lower Companion → Lower Stella → Has Video → Link
```

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
- Within the same Orbit and Level, maintainers may adjust row order based on readability and reference value.

## Deployment

This project is deployed with GitHub Pages.

Upload the contents of this deployment folder (`outputs/` in the local workspace) to the repository root while preserving the `css/`, `js/` and `assets/` directory structure. Do not upload an extra enclosing `outputs/` or `split/` folder.

- Include `index.html`, `css/`, `js/`, `assets/`, `README.md`, `valko-launch.md`, `robots.txt` and `sitemap.xml`.
- When CSS or JavaScript changes, update the corresponding `?v=` value in `index.html`, then upload the changed asset and `index.html` together.
- Keep the canonical URL, `og:url`, README website link, sitemap URL and robots sitemap declaration pointed at the production website.
- Update the sitemap `lastmod` for meaningful website releases. Website Version History is maintained separately in Google Sheets.
- After GitHub Pages finishes deploying, verify both modes on desktop and mobile, character and companion selection, manual level input, advanced filters, search-condition sharing, Contributors, and the detail views.
- Favorites and browsing history are stored in the player's browser under `ladsbattle_local_folder_v1`. Ordinary deployments must not rename this key or require players to clear site data.

## Notes

Valko is not enabled yet, but his place in this project is reserved. The integration and release checklist is maintained in [valko-launch.md](valko-launch.md).

# 面板資料庫｜戀與深空戰鬥討論群

This project is a battle stats database website for 「戀與深空戰鬥討論群」. It collects Orbit and Endless Challenge battle stats, allowing players to search, filter, save, and review battle records more efficiently.

## Website

https://ladsbattle.github.io/Love-and-Deepspace-Battle-Stats-tw/

## Data Source

The battle stats are collected from 「戀與深空戰鬥討論群_臉書面板分享專區」 and maintained through Google Sheets. The website reads published CSV data directly from the spreadsheet.

## Features

- Orbit search with quick level ranges, exact levels and manual level input
- Endless Challenge search through clickable character buttons and companion portraits (no dropdown menu)
- Advanced condition filters next to the result count, with their own reset control
- Video-only filter
- Share searches by copying the browser URL; filters update the URL automatically and are restored after data loads.
- Report form for incorrect or missing data
- Personal folder with favorites and browsing history
- Version History
- Last Updated timestamp for the database
- Chinese-only contributor list, maintained through Google Sheets

On mobile, both modes share the same initial filter-block minimum height (`23rem`). The initial companion hint is centered horizontally and vertically; selecting a character lets the block expand naturally to fit its portraits.

## Search Links

Search URLs use full names (no separate ID catalog). For example, `?mode=orbit&orbit=光&level=90` or `?mode=endless&character=祁煜&partner=赤霄武神`.
They preserve the current deployment path and share committed search conditions, not a frozen result list, favorites, or history. Manual layer input updates the URL only after submission. Changing filters replaces the current history entry rather than adding one entry per click.
Advanced parameters are `T1_Card`, `T1_Partner`, `T1_Stella`, `T2_Card`, `T2_Partner`, `T2_Stella` for Orbit, `card` for Endless, and `video=1` for either. Orbit ranges use `range=61-120` instead of `level`. Legacy `layer`, `upper…`, and `lower…` parameters are still accepted and rewritten to the current names when the URL syncs. If both names are present, the current name takes precedence.
Keep names consistent: renaming a character or companion may invalidate old links; renamed or removed advanced options can produce zero matches. Shared advanced conditions are retained even if the latest base results drop to zero or one, with a reset option still available.

## Data Maintenance

### Contributor List

- The acknowledgment dialog initially focuses its static title without an outline. The close button retains normal keyboard-focus feedback and no longer receives automatic opening focus.

The footer's `Contributors` link opens a Chinese-only acknowledgment dialog. Its published CSV is configured by `CONTRIBUTORS_CSV_URL` in `js/app.js` (sheet gid `1484906078`). No separate creator credit is hardcoded in the footer.

- Columns: `貢獻別` and `名稱` (column order does not matter; `分類` is also accepted).
- CSV categories and section headings, in display order: `網站維護`, `面板分享`. Old category labels are no longer supported; update the Sheet tags directly.
- Incomplete final rows are centered as a group without changing name order or column width: one remaining name centers alone; two remaining desktop names center together with the usual gap.
- The single-page dialog shares Information's fixed height, header and inner scrolling, with no tabs. Title and introduction stay visible; both name sections and the contact note scroll. Opening resets list scrolling. Section headings are centered and share the gradient underline used beneath Information's version dates. Names use regular weight: English retains Noto Serif TC; Chinese and Bopomofo use Noto Sans TC like the hint text, including within mixed-language names. Names are centered in three desktop columns or two mobile columns. Long names shrink to fit without truncation, recalculated when fonts or viewport width change. Introduction, loading text and contact note share the hint size. The introduction clarifies that alphabetical/stroke order does not rank contributions or importance.
- The Chinese contact note explains that only public posters are listed (not anonymous posters), and invites corrections, additions and name-removal requests. Only publish names eligible for this notice: the CSV has no anonymity field, so the site cannot automatically detect anonymous posters. The underlined `Ting Ting Wang` name opens the specified Facebook group profile in a new tab, with hover and keyboard-focus feedback; the link is maintained in `index.html`, not the Sheet.
- Use `、` between multiple categories in one cell. The same person can appear in multiple sections; repeated identical names within a section are deduplicated.
- Names are plain text, not profile links. English names sort first, A–Z (case-insensitive); Chinese names follow in stroke order. Leading punctuation does not determine the language group; names without Latin/Chinese letters sort last.
- The published names are shown as-is, including placeholders. Replace placeholders in the Sheet before announcing the list.
- The list loads only when opened, with a timeout, one automatic retry and a manual retry option. It does not block database startup. A successfully loaded list is reused until the page is refreshed; subsequent visits read the published Sheet again (Google publication/cache delays may apply).

### Battle Stats

The database is maintained in Google Sheets. Please keep all fields consistent when adding or editing battle stats.

Required Orbit column order (the parser reads by position, not header name):

```text
Orbit → Layer → Upper Card → Upper Companion → Upper Stella → Lower Card → Lower Companion → Lower Stella → Has Video → Link
```

Required Endless Challenge column order:

```text
Companion → Card → Stella Match Count → Score → Has Video → Link
```

Include a header row in each worksheet. `Has Video` must publish as `TRUE` for records with video; other values are treated as false. Keep each record on a single CSV line (no line breaks inside cells). Use companion short names consistently.

Within the same orbit and layer, maintainers may adjust the display order based on readability and reference value.

## Image Assets

When adding or updating image assets, keep filenames consistent with the names used in Google Sheets and the front-end display.

- Companion icons should be placed in `assets/companions/`.
- Rank icons should be placed in `assets/ranks/`.
- Use clear PNG files with transparent backgrounds when possible.

## New Companion Launch Checklist

When a new companion is released, update the following items together. The companion name must use exactly the same spelling in Google Sheets, the JavaScript catalog, CSS selectors, and the image filename. In this section, `LI` means love interest.

1. **Google Sheets data**
   - Add the new Endless Challenge records to the Endless Challenge worksheet.
   - Use only the companion's short name in the companion column, represented below as `COMPANION_NAME`, without the LI's name.
   - Confirm that the published CSV contains the new records and that there are no extra spaces or alternate punctuation in the companion name.

2. **Character and companion catalog**
   - In `js/app.js`, add `COMPANION_NAME` to the appropriate LI's `partners` array in `ENDLESS_CHARACTER_CATALOG`.
   - The catalog determines character order, companion order, character visibility and portrait-button theme. Companion labels use short names only; there is no `#partnerSelect` dropdown to edit in HTML.
   - For a new LI, add a catalog entry with `name`, `theme`, `visible` and `partners`. Keep unreleased entries at `visible: false` until launch.

3. **Theme color mapping**
   - Add the new `data-partner` value to all three matching selector groups in `css/styles.css`:
     - the Endless Challenge card top bar (`.endless-card::before`);
     - the companion badge (`.partner-badge`);
     - the Endless Challenge score (`.endless-score`).
   - Use the orbit color assigned to that companion.
   - Portrait buttons inherit the LI's catalog `theme`. If introducing a new theme, also provide its color tokens and matching character/companion button selectors.

4. **Companion icon**
   - Add a transparent PNG to `assets/companions/`.
   - The filename must exactly match the short companion name, represented as `COMPANION_NAME.png`.
   - The image path is generated from the companion name; no separate image-path mapping is required. The catalog update in step 2 is still required. Verify the PNG exists: inline avatars hide missing images, but portrait buttons do not provide the same fallback.

5. **Release verification**
   - Confirm the companion appears under the correct LI, in the intended portrait-button position.
   - Select the companion and confirm its Google Sheets records are returned.
   - Check the card top bar, badge, and score all use the intended theme color.
   - Check the companion icon in result cards, detail view, favorites, and browsing history.
   - Test the video-only and card filters together with the new companion.
   - Run a JavaScript syntax check before deployment, then verify the interaction on both desktop and mobile layouts.

6. **Maintenance history**
   - Add the new companion launch to the maintenance-history worksheet when the change should appear in the website's Version History.

## Deployment

This project is deployed with GitHub Pages.

Upload the contents of this deployment folder (`outputs/` in the local workspace) to the repository root while preserving the `css/`, `js/`, and `assets/` directory structure, then commit the changes to GitHub. Do not upload an extra enclosing `outputs/` or `split/` folder.

- Include `index.html`, `css/`, `js/`, `assets/`, `README.md`, `robots.txt` and `sitemap.xml`. Keep `.gitignore` in the repository; exclude `.DS_Store` and `._*` files. Git ignores do not filter files uploaded manually through a browser.
- When publishing CSS or JavaScript changes, update the corresponding `?v=` value on the stylesheet or script link in `index.html` and upload the HTML and changed asset together. Actual filenames remain `css/styles.css` and `js/app.js`.
- Keep the canonical URL, `og:url`, README website link, sitemap URL and robots sitemap declaration pointed at the production path shown above. Search links automatically preserve whichever deployment path the player visits.
- Update sitemap `lastmod` when releasing meaningful page changes. It describes the page update, not the latest Google Sheets record. Website Version History is maintained separately in its worksheet.
- After GitHub Pages finishes deploying, check both modes on desktop and mobile, portrait selection, manual level input, advanced filters, a shared search URL and Contributors. If the appearance is stale, confirm the deployed HTML and versioned assets match before clearing browser data.
- Favorites and browsing history stay in the player's browser under `ladsbattle_local_folder_v1`. Ordinary file updates do not clear them; do not rename that storage key or advise clearing site data as a routine deployment step.

### Loading and recovery

Main CSV requests time out after 10 seconds and retry once; a remaining failure offers `重新載入資料`. Image preloads have an 8-second per-image limit and do not prevent startup when they fail. Version History and Contributors are secondary data and do not block the main search interface.

## Notes

This website is a fan-maintained database for battle stats reference. All data is provided for search and reference purposes only.

- If Valko（敖尹） returns in a future update, remember to restore the Metal orbit visibility, upload the related companion image assets, and confirm that the Google Sheets data matches the front-end filter options before deployment.

# 面板資料庫｜戀與深空戰鬥討論群

This project is a battle stats database website for 「戀與深空戰鬥討論群」. It collects Orbit and Endless Challenge battle stats, allowing players to search, filter, save, and review battle records more efficiently.

## Website

https://ladsbattle.github.io/Love-and-Deepspace-Battle-Stats-tw/

## Data Source

The battle stats are collected from 「戀與深空戰鬥討論群_臉書面板分享專區」 and maintained through Google Sheets. The website reads published CSV data directly from the spreadsheet.

## Features

- Orbit battle stats search
- Endless Challenge battle stats search
- Advanced condition filters
- Video-only filter
- Report form for incorrect or missing data
- Personal folder with favorites and browsing history
- Version History
- Last Updated timestamp for the database

## Data Maintenance

The database is maintained in Google Sheets. Please keep all fields consistent when adding or editing battle stats.

Required column order:

```text
Orbit → Layer → Upper Card → Upper Companion → Upper Stella → Lower Card → Lower Companion → Lower Stella → Has Video → Link
```

Within the same orbit and layer, maintainers may adjust the display order based on readability and reference value.

## Image Assets

When adding or updating image assets, keep filenames consistent with the names used in Google Sheets and the front-end display.

- Companion icons should be placed in `assets/companions/`.
- Rank icons should be placed in `assets/ranks/`.
- Use clear PNG files with transparent backgrounds when possible.

## New Companion Launch Checklist

When a new companion is released, update the following items together. The companion name must use exactly the same spelling in Google Sheets, HTML, CSS selectors, and the image filename. In this section, `LI` means love interest.

1. **Google Sheets data**
   - Add the new Endless Challenge records to the Endless Challenge worksheet.
   - Use only the companion's short name in the companion column, represented below as `COMPANION_NAME`, without the LI's name.
   - Confirm that the published CSV contains the new records and that there are no extra spaces or alternate punctuation in the companion name.

2. **Endless Challenge companion menu**
   - Add the companion to `index.html` under `#partnerSelect`.
   - Keep `value` as the short companion name. The visible label may include the LI, for example:

     ```html
     <option value="COMPANION_NAME">LI_NAME．COMPANION_NAME</option>
     ```

   - Place the option after the other companions belonging to the same LI and in the intended display order.

3. **Theme color mapping**
   - Add the new `data-partner` value to all three matching selector groups in `css/styles.css`:
     - the Endless Challenge card top bar (`.endless-card::before`);
     - the companion badge (`.partner-badge`);
     - the Endless Challenge score (`.endless-score`).
   - Use the orbit color assigned to that companion.

4. **Companion icon**
   - Add a transparent PNG to `assets/companions/`.
   - The filename must exactly match the short companion name, represented as `COMPANION_NAME.png`.
   - No JavaScript update is normally required: the page automatically builds the icon path from the companion name and hides the image if the file is missing.

5. **Release verification**
   - Confirm the companion appears in the correct menu position.
   - Select the companion and confirm its Google Sheets records are returned.
   - Check the card top bar, badge, and score all use the intended theme color.
   - Check the companion icon in result cards, detail view, favorites, and browsing history.
   - Test the video-only and card filters together with the new companion.
   - Run a JavaScript syntax check before deployment, then verify the interaction on both desktop and mobile layouts.

6. **Maintenance history**
   - Add the new companion launch to the maintenance-history worksheet when the change should appear in the website's Version History.

## Deployment

This project is deployed with GitHub Pages.

To update the website, select and upload everything in this deployment folder (`outputs/` in the local workspace) to the repository root while preserving the `css/`, `js/`, and `assets/` directory structure, then commit the changes to GitHub.

## Notes

This website is a fan-maintained database for battle stats reference. All data is provided for search and reference purposes only.

- If Valko（敖尹） returns in a future update, remember to restore the Metal orbit visibility, upload the related companion image assets, and confirm that the Google Sheets data matches the front-end filter options before deployment.

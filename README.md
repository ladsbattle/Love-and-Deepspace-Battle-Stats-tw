# 面板資料庫｜戀與深空戰鬥討論群

This project is a battle stats database website for 「戀與深空戰鬥討論群」. It collects orbit and Endless Challenge battle stats, allowing players to search, filter, save, and review battle records more efficiently.

## Website

https://ladsbattle.github.io/Love-and-Deepspace-Battle-Stats-tw/

## Data Source

The battle stats are collected from 「戀與深空戰鬥討論群_臉書面板分享專區」 and maintained through Google Sheets. The website reads published CSV data directly from the spreadsheet.

## Features

- Orbit battle stats search
- Endless Challenge battle stats search
- Video-only filter
- Advanced condition filters
- Report form for incorrect or missing data
- Personal folder with favorites and browsing history
- Version History
- Last Updated timestamp for the database

## Data Maintenance

The database is maintained in Google Sheets. Please keep all fields consistent when adding or editing battle stats.

Recommended row order:

```text
Orbit → Layer → Upper Card → Lower Card
```

Within the same orbit and layer, maintainers may adjust the display order based on readability and reference value.

## Deployment

This project is deployed with GitHub Pages.

To update the website, replace or edit `index.html`, then commit the change to GitHub.

## Notes

This website is a fan-maintained database for battle stats reference. All data is provided for search and reference purposes only.

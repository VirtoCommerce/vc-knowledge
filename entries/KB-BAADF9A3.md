---
id: KB-BAADF9A3
subject: Admin Sign-in log grid prints the browser's timezone while the Sign-in record card and the user's Last login date use the operator profile timezone
plane: experiential
question: "Admin Security > Sign-in log: why does the same record show different clock times in the grid and on the Sign-in record blade?"
status: active
appliesTo:
  - axis: surface
    value: admin-ui
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/platform/security/sign-in-log/search
  - coordinate: GET /api/platform/profiles/currentuser
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T08:43:16.894Z
    by: session:624d35a1
    who: kutasinaelena
    build: b6daa5d65748d7e7
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_regression
    build: 76963fc77f8439ee
    conditions: platform=3.1076.6; setting:VirtoCommerce.Platform.UI.TimeZone=America/Los_Angeles; browser_tz=Europe/Moscow
    at: 2026-10-19T20:14:42.264Z
    by: session:6d670c2e
    bodyHash: 8df76e85474c
    defect: VCST-6310
    who: kutasinaelena
    note: "2026-10-19: createdDate 2026-10-19T20:03:59Z -> grid '10/19/26 11:03 PM' (browser UTC+3), Sign-in record card 'Oct 19, 2026 1:03:59 PM' (profile LA). Chart 24h last bucket 20:00Z labelled '11:00 PM'. Custom period From=To=2026-10-13 sends startDate 2026-10-12T21:00:00.000Z / endDate 2026-10-13T20:59:59.999Z (browser day). On a build with vc-platform PR #3140 (3.1084.0-pr-3140) grid, chart hours and the custom filter follow the profile zone instead."
---
Platform 3.1076.1, vcptcore_regression, 2026-10-13. Operator profile VirtoCommerce.Platform.UI.TimeZone = America/Los_Angeles (GET /api/platform/profiles/currentuser); browser timezone UTC+3. Record from POST /api/platform/security/sign-in-log/search had createdDate 2026-10-13T08:32:19.2266981Z. Grid Date column: '10/13/26 11:32 AM' (browser zone, UTC+3). Sign-in record blade Date: 'Oct 13, 2026 1:32:19 AM' (profile zone, UTC-7). Security > Users > admin > Last login date: 'Oct 13, 2026 1:32:19 AM' (profile zone). The dashboard 'Attempts over time' chart buckets also end at the browser's current hour. So the grid and the chart ignore the profile timezone that the record card, the user blade and (per other entries) the orders grid honour; neither prints an offset. The grid is not UTC.

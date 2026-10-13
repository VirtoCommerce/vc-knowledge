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
---
Platform 3.1076.1, vcptcore_regression, 2026-10-13. Operator profile VirtoCommerce.Platform.UI.TimeZone = America/Los_Angeles (GET /api/platform/profiles/currentuser); browser timezone UTC+3. Record from POST /api/platform/security/sign-in-log/search had createdDate 2026-10-13T08:32:19.2266981Z. Grid Date column: '10/13/26 11:32 AM' (browser zone, UTC+3). Sign-in record blade Date: 'Oct 13, 2026 1:32:19 AM' (profile zone, UTC-7). Security > Users > admin > Last login date: 'Oct 13, 2026 1:32:19 AM' (profile zone). The dashboard 'Attempts over time' chart buckets also end at the browser's current hour. So the grid and the chart ignore the profile timezone that the record card, the user blade and (per other entries) the orders grid honour; neither prints an offset. The grid is not UTC.

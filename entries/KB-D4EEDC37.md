---
id: KB-D4EEDC37
subject: whiteLabelingSettings.favicons lists thumbnail hrefs for the favicons task's sizes even when those thumbnails were never generated, and the storefront requests only those hrefs (no faviconUrl fallback), so the tab gets no custom favicon
plane: experiential
question: Does the storefront fall back to faviconUrl when the whiteLabelingSettings.favicons thumbnail files do not exist?
questions:
  - text: Why does a newly uploaded white-labeling favicon not show in the browser tab?
  - text: Where do the sizes in whiteLabelingSettings.favicons come from?
  - text: Why do favicon_<name>_16x16.png requests return 404 on the storefront?
concepts:
  - id: white-labeling
status: active
appliesTo:
  - axis: feature
    value: white-labeling
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.whiteLabelingSettings
  - coordinate: POST /api/assets
evidence:
  - method: observation
    deployment: vcst_qa
    build: 56f07d69d90abb81
    conditions: platform=3.1101.0; theme=2.63.0-alpha.2681; store=B2B-store; setting:ImageTools.Thumbnails.EnableImageProcessJob=false
    at: 2026-11-26T16:54:43.780Z
    by: session:d2ee4088
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 56f07d69d90abb81
    conditions: platform=3.1101.0; store=B2B-store; setting:ImageTools.Thumbnails.EnableImageProcessJob=false
    at: 2026-11-26T17:19:57.241Z
    by: session:d2ee4088
    bodyHash: a14d8927a7e3
    who: Lenajava1
    note: "Organization favicon PNG uploaded via the org White labeling blade (faviconUrl file 200): storefront home of an org buyer requested only the five _16x16.._196x196 thumbnail hrefs, all 404, never faviconUrl."
---
On vcst_qa 2026-11-26 (ImageTools.Thumbnails.EnableImageProcessJob=false, EventBasedThumbnailGeneration=false; thumbnail task "favicons" on /customization/favicons with options 16x16, 32x32, 96x96, 128x128, 196x196), a PNG favicon uploaded through the store White labeling blade was saved as faviconUrl (file 200). Query.whiteLabelingSettings returned faviconUrl plus favicons[] with five hrefs <file>_16x16.png ... _196x196.png. The storefront home page requested exactly those five hrefs, all 404, and never requested faviconUrl, so no custom favicon was shown. The same held for an organization favicon (.webp). Until the thumbnail task runs for the new file, the advertised favicon sizes do not exist.

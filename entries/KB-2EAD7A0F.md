---
id: KB-2EAD7A0F
subject: After the 2026-11-26 reseed the seeded organization favicon and its five favicons[] thumbnails return 200 although the ImageTools thumbnail job is disabled
plane: experiential
question: Are favicons[] hrefs of the seeded WL_ELECTRONICS organization reachable when the thumbnail job is off?
status: active
appliesTo:
  - axis: feature
    value: white-labeling
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.whiteLabelingSettings
  - coordinate: GET /cms-content/assets/customization/favicons/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: 56f07d69d90abb81
    conditions: platform=3.1101.0; module:VirtoCommerce.WhiteLabeling=3.1008.0; setting:ImageTools.Thumbnails.EnableImageProcessJob=false
    at: 2026-11-27T11:09:49.604Z
    by: session:d2ee4088
    who: Lenajava1
---
On vcst_qa 2026-11-27 (ImageTools.Thumbnails.EnableImageProcessJob=false) whiteLabelingSettings for the seeded organization AGENT-TEST-Electronics Store returned faviconUrl (.webp) plus favicons[] with 16x16, 32x32, 96x96, 128x128, 196x196 hrefs (.webp), and a plain GET of the faviconUrl, the 16x16 and the 196x196 hrefs returned 200, as did the logo. So for assets the seeder re-uploaded on 2026-11-26 the advertised thumbnail files exist; the 404 case in KB-D4EEDC37 is a favicon uploaded through the blade while the job is off, not every favicon. A case that needs a favicon tab to render must use a file whose thumbnails exist (a seeded asset) or run the thumbnail task for its own upload first.

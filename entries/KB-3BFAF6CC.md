---
id: KB-3BFAF6CC
subject: whiteLabelingSettings.favicons sizes are a hard-coded list of five in the xAPI handler, independent of any ImageTools favicons task
plane: experiential
question: Where do the favicon sizes in whiteLabelingSettings.favicons come from, and what are they?
status: active
appliesTo:
  - axis: module
    value: whitelabeling
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.whiteLabelingSettings
evidence:
  - method: source
    deployment: VirtoCommerce.WhiteLabeling@3.1008.0
    at: 2026-11-27T11:28:17.268Z
    by: session:d2ee4088
    who: Lenajava1
  - method: source
    deployment: VirtoCommerce.WhiteLabeling@3.1008.0
    at: 2026-11-27T11:43:04.345Z
    by: session:d2ee4088
    bodyHash: 371a4c3c954d
    who: Lenajava1
    note: "Re-read GetWhiteLabelingSettingsQueryHandler at tag 3.1008.0 on 2026-11-27: static _faviconsSizes = 16x16, 32x32, 96x96, 128x128, 196x196; AddFaviconsAsync runs whenever the merged FaviconUrl is non-empty. Same sizes, no thumbnail task registered by the module."
---
Read in vc-module-white-labeling 3.1008.0: GetWhiteLabelingSettingsQueryHandler declares a static array of five sizes (16x16, 32x32, 96x96, 128x128, 196x196) and, when the merged setting has a faviconUrl, adds one favicons[] item per size with href name_size+ext and type image/ext. The module registers no thumbnail task or sizes; the files exist only if an ImageTools task on the favicons folder generated those suffixes. A code search of the VirtoCommerce org for 196x196 finds only this handler; 180x180, 192x192 and 512x512 appear nowhere in the module. Not observed live by this session (KB-D4EEDC37 holds the live observation).

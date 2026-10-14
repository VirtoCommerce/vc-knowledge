---
id: KB-A81FBFCB
subject: A catalog publishing channel created or saved in the Admin keeps a stored CompletenessPercent (0) that evaluation never refreshes, so its Complete % stays 0% after evaluating products
plane: experiential
question: Why does a channel show Complete 0% after Evaluate finished for all its products?
status: active
appliesTo:
  - axis: module
    value: catalogpublishing@3.1007.1
  - axis: surface
    value: admin-ui
  - axis: surface
    value: rest-api
anchors:
  - coordinate: GET /api/completeness/channels/{id}
  - coordinate: POST /api/completeness/channels/{id}/evaluate
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-14T07:03:21.646Z
    by: session:624d35a1
    who: kutasinaelena
---
On CatalogPublishing 3.1007.1, I created a channel in Admin (Catalog publishing > Add) on a 108-product catalog, renamed it through Manage > Save, then ran Evaluate: 'Evaluation finished', 108/108, 0 errors. POST /api/completeness/channels/{id}/products/evaluate then returned per-product entries of 62/25/25. Even so, GET /api/completeness/channels/{id} and the Channels grid kept showing completenessPercent 0. Pre-2025 channels whose stored value is null show the average of their entries (e.g. 68.2%, 78.1%). In the source, CompletenessServiceImpl.GetChannelsByIdsAsync computes entries.Average() only when the stored channel.CompletenessPercent == null. Nothing in the evaluation path writes the channel value, and the channel update/patch persists whatever completenessPercent the blade sends. The same code exists at 3.1005.1.

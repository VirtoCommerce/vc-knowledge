---
id: KB-771EFA5D
subject: An organization's theme preset is read per org via GET /api/white-labeling/organization/{organizationId}; the module exposes no search endpoint, and the store-level preset is separate
plane: experiential
question: How do I find which organizations use a given theme preset (themePresetName) in white labeling?
questions:
  - text: Is there a search endpoint for white-labeling settings?
  - text: Where is an organization's themePresetName stored and how do I read it via REST?
  - text: Which REST routes does the White Labeling module expose?
status: active
appliesTo:
  - axis: surface
    value: platform-api
anchors:
  - coordinate: GET /api/white-labeling/organization/{id}
  - coordinate: POST /api/members/search
  - coordinate: Query.whiteLabelingSettings
evidence:
  - method: observation
    deployment: vcst_qa
    build: 01d892bf697c7bac
    at: 2026-11-10T13:01:13.086Z
    by: session:5730f825
    who: Aleksandra-Mitricheva
---
The White Labeling module's swagger (/docs/VirtoCommerce.WhiteLabeling/swagger.json) lists only GET /api/white-labeling/{id}, GET /api/white-labeling/organization/{organizationId}, GET /api/white-labeling/store/{storeId} and POST/PUT /api/white-labeling. POST /api/white-labeling/search returns 405 and /api/whitelabeling/search returns 404, so there is no search. To find the orgs that use a preset, list the organizations (POST /api/members/search with memberType=Organization) and call GET /api/white-labeling/organization/{id} for each one. An org with no setting returns an empty body. Each setting carries themePresetName and isEnabled. Anonymous Query.whiteLabelingSettings(storeId:"B2B-store") returned the store-level preset (Mercury) on this stand, while several orgs held their own presets (Watermelon, Coffee, Purple Pink, black-gold, …).

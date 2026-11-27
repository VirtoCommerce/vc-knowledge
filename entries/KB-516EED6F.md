---
id: KB-516EED6F
subject: POST /api/white-labeling accepts an organization record with isEnabled true and every branding field null, and GET /organization/{id} returns it with all fields null
plane: experiential
question: Can an organization have an enabled white-labeling record with no logo, favicon, preset or link lists, and what does GET return for it?
questions:
  - text: Is an empty enabled white-labeling record accepted by the REST API?
  - text: Does the white-labeling validator require a logo or preset on an organization record?
status: active
appliesTo:
  - axis: feature
    value: white-labeling
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/white-labeling
  - coordinate: GET /api/white-labeling/organization/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: 56f07d69d90abb81
    at: 2026-11-27T14:40:16.800Z
    by: session:d2ee4088
    who: Lenajava1
---
On vcst_qa 2026-11-27 POST /api/white-labeling with organizationId set, isEnabled true and logoUrl, secondaryLogoUrl, faviconUrl, themePresetName, mainMenuLinkListName, footerLinkListName all null was accepted; GET /api/white-labeling/organization/{organizationId} then returned the record with isEnabled true, storeId null and every one of those six fields null. No field beyond the organizationId/storeId key is required, so an organization can carry an enabled record that contributes nothing to whiteLabelingSettings.

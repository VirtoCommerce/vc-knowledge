---
id: KB-4FC31FE9
subject: whiteLabelingSettings for an organization whose record sets no main-menu name returns the store record's main-menu links, not an empty array
plane: experiential
question: What does xAPI whiteLabelingSettings(organizationId, storeId).mainMenuLinks return for an organization whose record has no main menu link list name while the store record has one?
status: active
appliesTo:
  - axis: feature
    value: white-labeling
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.whiteLabelingSettings
evidence:
  - method: observation
    deployment: vcst_qa
    build: 56f07d69d90abb81
    conditions: platform=3.1101.0; module:VirtoCommerce.WhiteLabeling=3.1008.0; store=B2B-store; setting:WhiteLabeling.WhiteLabelingEnabled=true
    at: 2026-11-27T12:22:00.118Z
    by: session:d2ee4088
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 56f07d69d90abb81
    conditions: platform=3.1101.0; theme=vc-theme-b2b-vue-2.63.0; store=B2B-store; browser=chromium
    at: 2026-11-27T12:49:53.262Z
    by: session:d2ee4088
    bodyHash: dbb01db07a22
    who: Lenajava1
    note: "Anonymous whiteLabelingSettings(organizationId: footer-only organization whose record has a footer list and no main-menu name, storeId: B2B-store) returned the store's 15 mainMenuLinks plus the organization's own footer; a member of that organization signed in on the storefront sees those same 15 store entries in the header menubar."
---
On vcst_qa 2026-11-27, with an admin token, Query.whiteLabelingSettings(organizationId: <theme-only organization GUID>, storeId: B2B-store, cultureName: en-US) for an organization whose enabled record has no main-menu and no footer name returned mainMenuLinks with 15 entries, the same count as the store-keyed call (store record main menu list catalog-menu). So the per-field merge hands an empty organization field to the store record; the answer is not the empty array that a missing/empty list name yields on the organization record alone. Read-only, repeated twice with the same count.

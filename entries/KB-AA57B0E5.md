---
id: KB-AA57B0E5
subject: the promotion combine policy setting is platform-wide, not per store
plane: experiential
question: Is the setting that decides whether promotion rewards stack configured per store or for the whole platform?
questions:
  - text: If I change whether discounts stack for one shop, does it affect my other shops too?
  - text: Where do I find the promotion stacking policy - in store settings or platform settings?
  - text: Why is the combine policy missing from the store's settings collection?
  - text: What scope does the platform settings API report for the promotion combine policy?
concepts:
  - id: promotion-combination
  - id: platform-settings
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: GET /api/platform/settings/{name}
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-16T17:27:27.519Z
    by: session:26f59771
    splitFrom: KB-13B32D5F
---
Marketing.Promotion.CombinePolicy, the setting that decides whether rewards stack, is PLATFORM-WIDE on this deployment - GET /api/platform/settings/Marketing.Promotion.CombinePolicy returns objectId null, objectType null, groupName Marketing|General, and it is absent from the store's own settings collection - so changing it changes every store at once.

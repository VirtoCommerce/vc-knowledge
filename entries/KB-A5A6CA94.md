---
id: KB-A5A6CA94
subject: The eight SalesRep.Statistics settings are edited only in the platform Settings blade (Sales Rep > Statistics), shown by their raw names; the store Settings blade has Sales Rep > General only
plane: experiential
question: Where in the Admin can an operator change the SalesRep.Statistics cache TTL and InvalidateOnChange settings, and are they available per store?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: GET /api/platform/settings/v2/global/schema
  - coordinate: GET /api/platform/settings/v2/tenant/Store/schema
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T22:01:00.079Z
    by: session:391bbdbb
    who: yuskithedeveloper
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T22:15:29.601Z
    by: session:391bbdbb
    who: yuskithedeveloper
    note: "Re-observed 2026-10-08 on Platform 3.1076.0 + SalesRep 3.1013.0-pr-18, admin language English: all 8 SalesRep.Statistics.* rows labelled by raw setting name; GET /api/platform/localization?lang=en has settings.SalesRep = {Enabled} only; v2/global/schema gives displayName null for all 9 SalesRep.* descriptors. Opening the row help shows the raw key settings.SalesRep.Statistics.<name>.description, not an empty text."
---
SalesRep 3.1013.0-pr-18. Admin > Settings (platform, global) has a 'Sales Rep' node whose only subgroup is 'Statistics', holding 8 settings: four SalesRep.Statistics.{Cart,CustomerCounts,Order,TopSeller}CacheExpirationMinutes integer fields and four ...InvalidateOnChange toggles, each labelled with its raw setting name (no localized title; the module's en localization defines only SalesRep.Enabled), values matching GET /api/platform/settings/{name}. A modified value carries the blue changed dot; integers have a 'Reset to default value' icon, toggles do not. Stores > B2B-store > Settings shows 'Sales Rep > General' with only 'Sales Rep Enabled' - no Statistics group, so the cache settings are module-global, not per store. The blades load GET /api/platform/settings/v2/global/schema + /v2/global/values and GET /api/platform/settings/v2/tenant/Store/schema.

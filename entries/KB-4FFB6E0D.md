---
id: KB-4FFB6E0D
subject: whiteLabelingSettings(domain) resolves the store only (organizationId null), and a call with neither storeId nor domain returns null
plane: experiential
question: What does Query.whiteLabelingSettings return when called with only the storefront domain, or with no storeId and no domain?
status: active
appliesTo:
  - axis: argument
    value: domain
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
    conditions: platform=3.1101.0; module:VirtoCommerce.WhiteLabeling=3.1008.0; store=B2B-store
    at: 2026-11-27T12:22:05.539Z
    by: session:d2ee4088
    who: Lenajava1
    note: "distinct: KB-F98BB191 is about a wrong organizationId on a storeId-keyed call; this is about the domain argument (store only) and about a call with no storeId and no domain (null)."
---
On vcst_qa 2026-11-27 (admin token, cultureName en-US): whiteLabelingSettings(domain: <storefront domain>) returned the store record's branding with storeId B2B-store, organizationId null and the same logoUrl as the storeId-keyed call; whiteLabelingSettings(cultureName: "en-US") with neither storeId nor domain returned HTTP 200, errors[] empty and data.whiteLabelingSettings null. So a domain resolves a store, never an organization, and an unresolvable call is null, not an error.

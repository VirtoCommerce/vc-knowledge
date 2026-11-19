---
id: KB-DBE2B1D9
subject: "Query.themeSnapshot: storeId wins over domain; an unknown domain falls back to the default store, while no store argument or an unknown storeId returns null"
plane: experiential
question: How does Query.themeSnapshot resolve the store from storeId, domain and cultureName?
questions:
  - text: Which wins in themeSnapshot when both storeId and domain are passed?
  - text: What does themeSnapshot return for an unknown domain or unknown store?
  - text: Which language does themeSnapshot serve for an unsupported cultureName?
status: active
appliesTo:
  - axis: module
    value: sitebuilder
  - axis: surface
    value: graphql
anchors:
  - coordinate: Query.themeSnapshot
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: platform=3.1099.0; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0
    at: 2026-11-19T13:11:36.637Z
    by: session:51ac2fe3
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: platform=3.1099.0; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0
    at: 2026-11-19T13:55:19.123Z
    by: session:51ac2fe3
    bodyHash: 83bd235fe6ed
    who: Lenajava1
    note: "Anonymous themeSnapshot(storeId, cultureName en-US): B2B-store and Electronics returned a snapshot (no store switch consulted); storeId AGENT-TEST-6571-A-NOSTORE returned themeSnapshot null."
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    at: 2026-11-19T14:20:50.866Z
    by: session:51ac2fe3
    bodyHash: 83bd235fe6ed
    who: Lenajava1
    note: "B2B-store (15 languages, default en-US): cultureName xx-XX -> cultureName en-US; de-DE -> de-DE; domain of the storefront host with zz-ZZ -> B2B-store, en-US; no cultureName -> en-US."
---
Anonymous calls, HTTP 200 each: storeId=Electronics plus the main storefront's domain -> Electronics (storeId wins). domain only (bare host or with https://) -> the store bound to that host. domain=no-such-host.example -> the default store's snapshot, not null. No arguments, or storeId "" -> themeSnapshot null. storeId=NO-SUCH-STORE -> null. storeId in another letter case (b2b-store) resolves to the store. cultureName the store supports (de-DE, fr-FR) -> served with that cultureName; cultureName the store lacks (xx-XX, or de-DE on a single-language store) -> the store default language, and the response field cultureName states the language actually served.

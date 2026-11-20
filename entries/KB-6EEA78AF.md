---
id: KB-6EEA78AF
subject: Flag-on storefront sends exactly one ThemeSnapshot per cold load; a /de/ URL sends cultureName "de" and is answered with de-DE; an aborted inline request triggers one refetch without permalink
plane: experiential
question: How many ThemeSnapshot requests does the Site Builder (flag-on) storefront send on a cold page load, and what language is served for a /de/ prefix?
questions:
  - text: Does the storefront refetch the theme snapshot when the inline request fails?
  - text: What variables does the inline ThemeSnapshot request carry?
  - text: Is ThemeSnapshot sent before InitializeApplication?
status: active
appliesTo:
  - axis: buyer
    value: guest
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.themeSnapshot
  - coordinate: /de/demo-landing
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: theme=vc-theme-b2b-vue-2.63.0-pr-2676; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; platform=3.1099.0; setting:site_builder_enabled=true
    at: 2026-11-19T16:05:21.726Z
    by: session:51ac2fe3
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: theme=2.63.0-pr-2676; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; store=B2B-store; browser=chromium; publication=16
    at: 2026-11-19T18:13:53.205Z
    by: session:51ac2fe3
    bodyHash: 87cec41142a2
    who: Lenajava1
    note: "Chromium lane, guest, cache disabled: /, /demo-landing, /de/demo-landing, /qa-return-policy, /de/qa-return-policy each sent exactly one ThemeSnapshot, before the mf-entry-bootstrap module request; /de/ sent cultureName \"de\", answered de-DE. Aborted inline request -> one refetch {storeId, cultureName en-US}; delayed 2.5 s -> no refetch; no snapshot on later in-site navigation."
  - method: source
    deployment: vc-frontend@a18a3bb
    conditions: theme=2.63.0-pr-2676; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; setting:site_builder_enabled=true
    at: 2026-11-20T07:26:03.128Z
    by: session:51ac2fe3
    bodyHash: 87cec41142a2
    who: Lenajava1
    contradicts: true
    note: "The \"aborted inline request triggers one refetch without permalink\" half is wrong. In the Chromium HAR the {storeId, cultureName} request came on the first in-site click (Referer /contacts, same operation id as GetSlugInfo \"contacts\"), not at boot: it is the navigation retry (boot.ts:107-108 -> :86). An aborted inline request is an error outcome and triggers no boot refetch (boot.ts:116-118). The boot refetch (store/language mismatch) does carry the permalink (boot.ts:50). \"No snapshot on later in-site navigation\" is also wrong: every navigation retries while the snapshot is failed. The one-request-per-cold-load, /de/ -> de-DE and delayed-request halves stand. The Firefox half was not re-checked (no Firefox HAR kept)."
---
Anonymous guest, Firefox, fresh browser contexts (empty cache), B2B-store. 20 cold loads of /, /demo-landing and two PDPs: exactly one POST /graphql ThemeSnapshot per load, the first GraphQL request (≈175-565 ms after navigation start, always before InitializeApplication), variables {domain, permalink} with permalink "/" for home and the bare path without leading slash otherwise; answer publication 16, storeId B2B-store, cultureName en-US. /de/demo-landing: one request with variables {domain, cultureName:"de", permalink:"demo-landing"}, answered cultureName "de-DE", html lang de-DE, German H1, no refetch. Inline request aborted: the app sent exactly one refetch with {storeId:"B2B-store", cultureName:"en-US"} (no domain, no permalink), answered publication 16 with slug null; page rendered normally. Inline request delayed 3 s: no refetch was sent. In-site navigation after either degraded start sent no further ThemeSnapshot. The flag-on build drops one GetMenu call that dev makes after GetPageContext (menus come with the snapshot), so the GraphQL request count per page equals dev's.

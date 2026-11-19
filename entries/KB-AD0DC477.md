---
id: KB-AD0DC477
subject: "Flag-on storefront: a signed-in contact whose default language differs from the store's gets one ThemeSnapshot refetch in that language on an unprefixed URL, and the first render already shows that language"
plane: experiential
question: What does the flag-on storefront do on an unprefixed URL when the signed-in contact's default language is not the store default?
questions:
  - text: Contact defaultLanguage and ThemeSnapshot refetch
  - text: How long after the snapshot does a contact in another language see the first render?
status: active
appliesTo:
  - axis: buyer
    value: signed-in-contact
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.themeSnapshot
  - coordinate: /account/profile
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: theme=2.63.0-pr-2676; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; setting:site_builder_enabled=true; browser=chromium
    at: 2026-11-19T18:14:27.637Z
    by: session:51ac2fe3
    who: Lenajava1
---
Contact defaultLanguage de-DE (store default en-US), no pinnedLocale, Chromium, cache disabled, 6 cold loads of unprefixed URLs (/agent-test page, /demo-landing): inline ThemeSnapshot {domain, permalink} answered en-US at ~300-350 ms; after GetPageContext the app sent exactly one refetch {storeId:"B2B-store", cultureName:"de-DE", permalink} answered de-DE at ~665-790 ms; first main heading painted at 925-1409 ms after navigation start, already in German, html lang de-DE, URL moved to /de/<path>; header texts German. With defaultLanguage null the same loads stayed en-US with no refetch. Switching the header language selector while signed in does not write the contact's defaultLanguage (it stays null); the profile page's Default language does.

---
id: KB-DF8D085C
subject: a page missing the requested culture falls back to the default-language content, not a 404
plane: experiential
question: What does the storefront serve at /{culture}/{permalink} when the page exists but has no version in that language?
questions:
  - text: If I switch the store to another language and a page is not translated, do I get an error page?
  - text: Is serving the default-language body for an untranslated content page expected behaviour or a defect?
  - text: What does a localized content page URL return when the page has no version in that culture?
  - text: "In which order does the storefront resolve the locale: URL, pinned locale, contact culture, store default?"
  - text: Do draft or archived content pages render the not-found view or fall back to another language?
concepts:
  - id: localization
  - id: content-page
  - id: page-not-found
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /{culture}/{permalink}
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:15:43.215Z
    by: session:memimpor
    who: Lenajava1
    build: e7c4faf10f640e7c
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T09:03:58.392Z
    by: session:b1874592
    who: Lenajava1
    note: "2026-10-08: /fr/qa-return-policy (fr-FR version exists only as Draft) returned the en-US body under FR chrome (breadcrumb 'Accueil'); en-US at /qa-return-policy, de-DE at /de/qa-return-policy. Also observed: /en/ prefix is not a valid locale prefix on this store (/en/ home → 404 page)."
    build: c4000d0bea152759
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    build: 706d29d69470a72c
    conditions: theme=2.61.0-pr-2576-211e3e15; module:VirtoCommerce.Pages=3.1011.0-pr-23-4bcd; module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-175-a3bc; setting:VirtoPages.MissingTranslationBehavior=unset(DefaultLanguage)
    at: 2026-10-23T12:31:49.387Z
    by: session:0204f22f
    bodyHash: c7ca310357eb
    who: Aleksandra-Mitricheva
    note: "2026-10-23 on a Multilanguage Page (pagebuilder PR 175): /es/<permalink> with no es-ES version rendered the store-default (en-US) version, html lang es-ES, HTTP 200. Holds only while a store-default-language version is published: with only de-DE+fr-FR published the same URL rendered the 404 view (see the separate capture on the root default language)."
  - method: observation
    deployment: vcptcore_qa
    build: 706d29d69470a72c
    conditions: theme=2.61.0-pr-2576-211e3e15; module:VirtoCommerce.Pages=3.1011.0-pr-23-4bcd
    at: 2026-10-23T13:01:45.528Z
    by: session:0204f22f
    bodyHash: c7ca310357eb
    who: Aleksandra-Mitricheva
    note: "Under the default (setting unset): /es/<en permalink> and /es/<fr permalink> both rendered the en-US version (the second URL corrected to /es/<en permalink>), html lang es-ES. With VirtoPages.MissingTranslationBehavior=NotFound on the store the same /es/ URL rendered the 404 view at HTTP 200 (slugInfo es-ES empty) while en-US and fr-FR versions stayed served; restoring the setting brought the fallback back at once. A version restricted to a user group the visitor lacks does NOT fall back: /fr/<fr permalink> rendered 404."
---
When a content page exists but has no version in the requested supported culture, the storefront returns 200 and renders the store default language's page body under site chrome localized to the requested culture. Locale resolution runs URL locale, then pinned locale, then contact culture, then the store default culture. A permalink that does not exist at all (including draft or archived pages) renders the 404 Page not found view. So an untranslated existing page is BY DESIGN a fallback; serving real content in that language needs a page version for that culture.

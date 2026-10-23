---
id: KB-AA9BFC82
subject: Multilanguage Page fallback (MissingTranslationBehavior=DefaultLanguage) targets the STORE default language version, not the page's own Default language; without a store-default version a missing language resolves to nothing
plane: experiential
question: Which version does slugInfo return for a missing language when a Multilanguage Page's Default language differs from the store default?
questions:
  - text: Does the DefaultLanguage missing-translation fallback use the multilanguage page default culture or the store default culture?
  - text: Why does /es/<permalink> show 404 for a multilanguage page with only de-DE and fr-FR published?
  - text: When is x-default emitted for a Page Builder multilanguage page?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pages
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.slugInfo
  - coordinate: Query.pageDocument
  - coordinate: /{id}/{id}
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 706d29d69470a72c
    conditions: module:VirtoCommerce.Pages=3.1011.0-pr-23-4bcd; module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-175-a3bc; module:VirtoCommerce.XCMS=3.1009.0-pr-27-bb4c; theme=2.61.0-pr-2576-211e3e15; setting:VirtoPages.MissingTranslationBehavior=unset
    at: 2026-10-23T12:32:11.450Z
    by: session:0204f22f
    who: Aleksandra-Mitricheva
---
Store default en-US, setting VirtoPages.MissingTranslationBehavior unset (DefaultLanguage). A Multilanguage Page with DefaultCultureName de-DE and only de-DE + fr-FR versions published: slugInfo(permalink, storeId, cultureName=es-ES) and cultureName=en-US both returned entityInfo null; the storefront rendered the 404 view at HTTP 200 for /es/<permalink> and /<permalink>; head had hreflang de-DE and fr-FR only, no x-default. After an en-US version was added and published, es-ES resolved to the en-US version and x-default appeared pointing at the en-US URL. Archiving the en-US version brought the 404 back within 2 s. So the fallback and x-default follow the store default culture; the page's own Default language (the source language for Outdated) does not act as a fallback.

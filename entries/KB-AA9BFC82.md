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
  - method: observation
    deployment: vcptcore_qa
    build: 706d29d69470a72c
    conditions: theme=2.61.0-pr-2576-211e3e15; module:VirtoCommerce.Pages=3.1011.0-pr-23-4bcd; setting:VirtoPages.MissingTranslationBehavior=unset
    at: 2026-10-23T13:00:56.774Z
    by: session:0204f22f
    bodyHash: ba69327870e5
    who: Aleksandra-Mitricheva
    note: "Reproduced on a new de-DE-default page: /es/ and / render the 404 view, hreflang de-DE+fr-FR only, no x-default; after an en-US version is published /es/ serves en-US content and x-default appears."
  - method: observation
    deployment: vcptcore_qa
    build: f1db890a6a93d133
    conditions: module:VirtoCommerce.Pages=3.1011.0-pr-23-e50f; module:VirtoCommerce.PageBuilderModule=3.1039.0-pr-175-3d2f; module:VirtoCommerce.XCMS=3.1009.0-pr-27-a8d6; theme=2.61.0-pr-2576-be2c-be2c7cc0; setting:VirtoPages.MissingTranslationBehavior=unset
    at: 2026-10-27T18:59:36.441Z
    by: session:b715861d
    bodyHash: ba69327870e5
    who: Aleksandra-Mitricheva
    contradicts: true
    contested: module:VirtoCommerce.Pages 3.1011.0-pr-23-e50f vs 3.1011.0-pr-23-4bcd; module:VirtoCommerce.PageBuilderModule 3.1039.0-pr-175-3d2f vs 3.1036.0-pr-175-a3bc; module:VirtoCommerce.XCMS 3.1009.0-pr-27-a8d6 vs 3.1009.0-pr-27-bb4c; theme 2.61.0-pr-2576-be2c-be2c7cc0 vs 2.61.0-pr-2576-211e3e15; …
    note: "On the newer build a de-DE-default Multilanguage Page with only de-DE + fr-FR published (store default en-US) no longer resolves to nothing: slugInfo(de permalink, es-ES) and (de permalink, en-US) return the de-DE version with languageCode null; slugInfo(fr permalink, en-US/es-ES) returns the fr-FR version — i.e. the version the visited permalink belongs to. Storefront /es/<de permalink> and /<de permalink> render the de-DE content (no 404), hreflang de-DE + fr-FR, no x-default. After an en-US version is published, /es/<de permalink> moves to /es/<en permalink> with en-US content and x-default appears pointing at the en-US URL. So the store-default part of the entry still holds when a store-default version exists; the 'resolves to nothing' part no longer holds on this build."
---
Store default en-US, setting VirtoPages.MissingTranslationBehavior unset (DefaultLanguage). A Multilanguage Page with DefaultCultureName de-DE and only de-DE + fr-FR versions published: slugInfo(permalink, storeId, cultureName=es-ES) and cultureName=en-US both returned entityInfo null; the storefront rendered the 404 view at HTTP 200 for /es/<permalink> and /<permalink>; head had hreflang de-DE and fr-FR only, no x-default. After an en-US version was added and published, es-ES resolved to the en-US version and x-default appeared pointing at the en-US URL. Archiving the en-US version brought the 404 back within 2 s. So the fallback and x-default follow the store default culture; the page's own Default language (the source language for Outdated) does not act as a fallback.

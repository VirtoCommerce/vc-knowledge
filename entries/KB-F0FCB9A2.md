---
id: KB-F0FCB9A2
subject: Removing a language from a store drops that language's multilanguage page version from pageDocument.alternates at once, but slugInfo with that culture still resolves the version and pageDocument still serves it
plane: experiential
question: What happens to a published multilanguage page version when its language is removed from the store?
questions:
  - text: Does pageDocument.alternates still list a version whose culture is no longer a store language?
  - text: Does slugInfo resolve a permalink for a culture removed from the store?
status: active
appliesTo:
  - axis: entity
    value: multilanguage-page
  - axis: module
    value: virtocommerce.pages
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.slugInfo
  - coordinate: Query.pageDocument
  - coordinate: PUT /api/stores
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 37f84b650ab7f9a6
    conditions: module:VirtoCommerce.Pages=3.1011.0-pr-23-4bcd; module:VirtoCommerce.XCMS=3.1009.0-pr-27-bb4c; module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-175-a3bc; setting:VirtoPages.MissingTranslationBehavior=unset; setting:Stores.AllowAnonymousUsers=true
    at: 2026-10-26T09:20:32.003Z
    by: session:0204f22f
    who: Aleksandra-Mitricheva
    note: "distinct: KB-77A6C523 covers resolution between versions whose cultures ARE store languages; this is about a version whose culture was REMOVED from the store after publishing: alternates drop it, but slugInfo/pageDocument with that culture still serve it."
---
Single-language store (en-US) temporarily given fr-FR via PUT /api/stores; multilanguage page with en-US and fr-FR (custom permalink) versions, both published: alternates listed both. After PUT /api/stores removed fr-FR again (read back languages=["en-US"]), within ~10 s pageDocument.alternates for both versions listed only en-US (fr-FR gone, also from the fr-FR document's own alternates). slugInfo(fr permalink, en-US) returned the en-US version (correct). slugInfo(fr permalink or en permalink, fr-FR) still returned the fr-FR version id, and pageDocument(that id) still returned the document — xAPI does not check that the requested culture is a store language. slugInfo(en permalink, de-DE — never a store language) returned the en-US version (fallback). Storefront half not observed (that store's storefront host did not resolve).

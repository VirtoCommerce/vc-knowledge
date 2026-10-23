---
id: KB-BE24A13A
subject: Page Builder multilanguage hreflang uses full culture codes, absolute hrefs on the store URL, x-default = store-default version, and no rel=canonical
plane: experiential
question: What hreflang tags does the storefront emit on a Page Builder multilanguage page?
questions:
  - text: Are hreflang values short (de) or full cultures (de-DE) on Page Builder pages?
  - text: Which locale prefix does the storefront use for nl-NL and nl-BL when both are store languages?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.xcms
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /{id}/{id}
  - coordinate: Query.pageDocument
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 706d29d69470a72c
    conditions: theme=2.61.0-pr-2576-211e3e15; module:VirtoCommerce.XCMS=3.1009.0-pr-27-bb4c; module:VirtoCommerce.Pages=3.1011.0-pr-23-4bcd
    at: 2026-10-23T12:32:11.649Z
    by: session:0204f22f
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcptcore_qa
    build: 706d29d69470a72c
    conditions: theme=2.61.0-pr-2576-211e3e15; module:VirtoCommerce.Pages=3.1011.0-pr-23-4bcd; module:VirtoCommerce.XCMS=3.1009.0-pr-27-bb4c
    at: 2026-10-23T13:00:56.579Z
    by: session:0204f22f
    bodyHash: 9cb2788ea177
    who: Aleksandra-Mitricheva
    note: "Re-observed on a fresh 5-version page (en-US default, fr-FR custom permalink, de-DE, nl-NL, nl-BL): one hreflang per published version with full culture, absolute on the store URL, x-default = en-US URL, no canonical; a single-version (unlinked) page has no hreflang at all. A version restricted to a user group and an unlinked version both drop out of alternates and hreflang."
---
On a signed-in storefront page of a published Multilanguage Page the head held one link rel=alternate per published store-language version with hreflang set to the full culture (de-DE, en-US, fr-FR, nl-NL, nl-BL), href absolute on the store URL, the store default culture without a prefix and others with /<xx>/ or, for nl-NL and nl-BL coexisting, /nl-NL/ and /nl-BL/; plus hreflang x-default pointing at the store-default version, omitted when no store-default version is published. No rel=canonical was present. /nl/<permalink> renders the 404 view for any page (also a plain Landing page) when two nl-* cultures exist. alternates/hreflang reflect a language version's permalink as soon as it is changed via PUT grouped, before the version is republished.

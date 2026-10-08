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
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T09:03:58.392Z
    by: session:b1874592
    who: Lenajava1
    note: "2026-10-08: /fr/qa-return-policy (fr-FR version exists only as Draft) returned the en-US body under FR chrome (breadcrumb 'Accueil'); en-US at /qa-return-policy, de-DE at /de/qa-return-policy. Also observed: /en/ prefix is not a valid locale prefix on this store (/en/ home → 404 page)."
---
When a content page exists but has no version in the requested supported culture, the storefront returns 200 and renders the store default language's page body under site chrome localized to the requested culture. Locale resolution runs URL locale, then pinned locale, then contact culture, then the store default culture. A permalink that does not exist at all (including draft or archived pages) renders the 404 Page not found view. So an untranslated existing page is BY DESIGN a fallback; serving real content in that language needs a page version for that culture.

---
id: KB-DF8D085C
subject: a page missing the requested culture falls back to the default-language content, not a 404
plane: experiential
question: What does the storefront serve at /{culture}/{permalink} when the page exists but has no version in that language?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /{culture}/{permalink}
evidence:
  - method: observation
    deployment: vcst-qa
    at: 2026-09-25T11:15:43.215Z
    by: session:memimpor
    who: Lenajava1
---
When a content page exists but has no version in the requested supported culture, the storefront returns 200 and renders the store default language's page body under site chrome localized to the requested culture. Locale resolution runs URL locale, then pinned locale, then contact culture, then the store default culture. A permalink that does not exist at all (including draft or archived pages) renders the 404 Page not found view. So an untranslated existing page is BY DESIGN a fallback; serving real content in that language needs a page version for that culture.

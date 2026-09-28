---
id: KB-B77CC09C
subject: "Storefront search results h1 accessible name has no space before or after the result count: '...returned the following3results'"
plane: experiential
question: How is the storefront search results heading with the product count exposed to assistive technology?
status: active
appliesTo:
  - axis: element
    value: search-results-heading
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /search
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-28T16:31:20.791Z
    by: session:p30132
    who: Lenajava1
---
On /search?q=<term> with 3 hits the level-1 heading's accessible name was "Your search for <term> returned the following3results": the superscript count node reads "3results". The visual gaps come only from CSS margins (me-1 on the count). The same heading markup serves /search?barcode= lists. Observed on a vc-frontend 2.59 build; the template is unchanged from dev.

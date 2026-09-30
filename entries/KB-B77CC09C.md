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
  - method: observation
    deployment: vcst
    at: 2026-09-28T18:22:23.282Z
    by: session:p25764
    who: Lenajava1
    note: "Re-observed on /search?barcode=<shared GTIN> and /search?q=<GTIN>: h1 accessible names 'Your search for barcode <v> returned the following3results' and '...following2results'"
  - method: observation
    deployment: vcst
    at: 2026-09-30T15:35:19.831Z
    by: session:134990b5
    who: Lenajava1
    note: "Re-observed on theme 2.59.0-pr-2501-3a82: barcode heading accessible name 'Your search for barcode <v> returned the following3results'."
---
On /search?q=<term> with 3 hits the level-1 heading's accessible name was "Your search for <term> returned the following3results": the superscript count node reads "3results". The visual gaps come only from CSS margins (me-1 on the count). The same heading markup serves /search?barcode= lists. Observed on a vc-frontend 2.59 build; the template is unchanged from dev.

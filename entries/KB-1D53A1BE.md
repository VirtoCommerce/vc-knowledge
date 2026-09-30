---
id: KB-1D53A1BE
subject: A barcode lookup pushes view_search_results with the barcode as search_term, and a single-hit redirect pushes only view_item
plane: experiential
question: Which analytics events does the storefront push for a /search?barcode= lookup compared with a /search?q= search?
questions:
  - text: When a shopper scans a code that matches one item, is that counted as a search in our analytics?
  - text: What search_term does the search results event carry for a barcode lookup that also has a keyword?
  - text: Does a zero-result barcode lookup still report a search results event with a count of zero?
  - text: Is the search event pushed when a barcode results page is opened from a link rather than a scan?
concepts:
  - id: analytics
  - id: barcode-search
status: active
appliesTo:
  - axis: element
    value: google-analytics
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /search
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-28T18:22:50.157Z
    by: session:p56028
    who: Lenajava1
    splitFrom: KB-6F0094B3
---
Observed in the GA4 dataLayer on a vc-frontend 2.59 build carrying the barcode scanner search setup (theme PR 2501), store in exact mode. Opening /search?barcode=<v> by link: 3 hits pushed view_item_list and view_search_results with search_term <v>, results_count 3, results_page 1 and the three SKUs in visible_items; 0 hits pushed both with results_count 0 and an empty visible_items; exactly one hit replaced the URL with the product page and pushed only view_item, no view_search_results and no view_item_list. With &q=<other term> added, view_search_results still carried the barcode as search_term. /search?q=<v> pushed view_search_results with the phrase as search_term. No search event was pushed for a link-opened lookup (in source it is sent only after a scan).

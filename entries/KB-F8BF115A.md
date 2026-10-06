---
id: KB-F8BF115A
subject: salesRepCustomerOrders returns an empty term_facets array when the filter matches zero orders, so the storefront must keep its own status labels for filter chips
plane: experiential
question: What does the SalesRepCustomerOrders status facet return when a search or date filter yields zero orders, and what does the status filter chip show then?
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers/{orgId}/orders
  - coordinate: Query.salesRepCustomerOrders
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-06T12:06:27.142Z
    by: session:7e4f80ae
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-06T15:07:13.273Z
    by: session:7982e016
    who: Aleksandra-Mitricheva
    note: "Confirmed on theme 2.59.0-pr-2519: zero-match (keyword or future createddate range) returns totalCount 0 and term_facets []. On THIS older build (not pr-2541) the status chip falls back to the raw term: 'Wird bearbeitet' becomes 'Processing' at zero matches and returns to 'Wird bearbeitet' when rows come back."
---
On /company/my-customers/<orgId>/orders, culture de-DE, theme 2.59.0-pr-2541 (vc-frontend PR #2541), with Order.Status translations present: with results, SalesRepCustomerOrders (facet:"status") returns term_facets status terms with localized labels (Cancelled→Abgesagt, Processing→In Bearbeitung, Payment required→Bezahlung erforderlich, New→Neu) and counts that ignore the status filter itself. Adding a non-matching keyword (filter "ZZNOMATCH7Q2X status:\"Cancelled\"") or a future createddate range returns totalCount 0 and term_facets [] — no label for the applied term. With that build the status chip keeps the localized label (it remembers labels from earlier responses); the filter drawer hides the Status group at zero matches. Without Order.Status translations for the culture the facet labels come back as the raw English term, so a chip-localization check is not discriminating on such a stand. Switching storefront language reloads the page and clears filters and search.

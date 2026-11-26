---
id: KB-55BB53BA
subject: "Org-member Select address popup on /cart: facet options show names (country, region, city) with counts, and picking one re-queries GetCurrentOrganizationAddresses with countryCodes / regionIds"
plane: experiential
question: What do the Country / State / City facets of the /cart Select address popup show, and what request does picking one send?
questions:
  - text: Does the city facet in the checkout address popup show city names or postal codes?
  - text: Which GraphQL argument carries the selected state in the cart address popup?
  - text: Is the country facet label the same text as the Country column in the address table?
status: active
appliesTo:
  - axis: role
    value: organization-member
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: Query.currentOrganizationAddresses
  - coordinate: Mutation.addOrUpdateCartShipment
evidence:
  - method: observation
    deployment: vcst_dev
    build: 7fd3fb4cc4be53eb
    conditions: theme=2.63.0-pr-2674-fc38-fc383c4e; role=Organization maintainer
    at: 2026-11-18T20:28:26.529Z
    by: session:feee22e7
---
Organization member (corporate) on /cart, Delivery = Shipping, clicked 'select a shipping address'. The popup (data-test-id select-address-modal) has three facet buttons (filter-country, filter-region, filter-city), a keyword input (search-keyword-input) and a 4-column table Address / Description / Country / Active. GetCurrentOrganizationAddresses returns term_facets CountryCode (term USA, label 'United States of America'), RegionId (term CA, label 'California'), City (term and label 'San Francisco'), each with count. The dropdown options render the label plus count ('San Francisco 1', 'California 1', 'United States of America 1') — names, not codes or postal codes. Checking a Country option re-sends GetCurrentOrganizationAddresses with countryCodes:["USA"]; checking a State option sends regionIds:["CA"]; a chip and 'Reset filters' appear. Note the facet label 'United States of America' differs from the table's Country cell 'United States' (address countryName). Selecting the row and OK sends AddOrUpdateCartShipment and Shipping address shows the full address.

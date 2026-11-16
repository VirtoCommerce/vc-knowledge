---
id: KB-F303BBA8
subject: "The /compare product list is kept across a storefront organization switch: the same entries are listed under the newly selected organization"
plane: experiential
question: Does the compare list on /compare persist after switching organization?
questions:
  - text: Is the product compare selection per organization or per browser?
  - text: What happens to compared products when a multi-org user switches organization?
status: active
appliesTo:
  - axis: feature
    value: product-compare
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /compare
  - coordinate: Query.products
evidence:
  - method: observation
    deployment: vcst_qa
    build: fcea7ce160c179b6
    conditions: theme=3.3.0-pr-23-7be3-7be3dfdd; browser=firefox
    at: 2026-11-16T13:20:03.773Z
    by: session:4cdfee7c
    who: Lenajava1
---
Signed in as a contact belonging to two organizations (Firefox 1280px, theme 3.3.0-pr-23). Added two products to compare under organization A, opened /compare (2 columns), switched to organization B in the header organization switcher, reopened /compare: the same 2 entries were listed with their prices re-fetched (the products query bearer token carried the new organization_id claim; no organizationId variable is sent). Switching back to A showed the same list. The list is per browser profile, not per organization. The two products had equal prices in both organizations, so whether the price cells re-price per organization was not decidable from this data.

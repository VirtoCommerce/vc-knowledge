---
id: KB-F0FDB3F2
subject: The State / Province facet in the checkout address popup is removed while a Country filter selects a country without regions, and returns when the filter is cleared
plane: experiential
question: Does the State / Province facet of the checkout shipping address popup react to an applied Country filter?
status: active
appliesTo:
  - axis: flow
    value: checkout-shipping-address-popup
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: Query.currentOrganizationAddresses
evidence:
  - method: observation
    deployment: vcst_qa
    build: e00c89f4da0e9c98
    conditions: platform=3.1083.0; theme=2.60.0-alpha.2579; role=Org Admin
    at: 2026-10-15T11:27:32.628Z
    by: session:10db9d7d
    who: Lenajava1
---
On /cart with Delivery = Shipping, the shipping address-selection popup of an organisation member whose address book holds US, CA and GB addresses shows three facet buttons: Country (CA 5, GB 5, US 12), State / Province (12 US and CA states with counts) and City. After choosing Country = United Kingdom the popup lists the 5 GB addresses, a Country chip and Reset filters appear, and the State / Province button is no longer in the popup (absent from the accessibility tree, not merely empty); the Country dropdown keeps listing all three countries with their unfiltered counts. Closing the chip restores the full list and the State / Province button.

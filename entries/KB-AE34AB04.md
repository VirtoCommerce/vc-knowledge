---
id: KB-AE34AB04
subject: The rep coupons page for an unserved organization shows the profile's not-found text, the generic 'Promotion coupons' heading and no coupon cards, although salesRepCoupons still returns store coupons
plane: experiential
question: What does /company/my-customers/{organizationId}/coupons show a sales rep for an organization the rep does not serve?
questions:
  - text: Does the sales rep coupons page leak store coupons for a customer the rep has no access to?
  - text: What heading and breadcrumb does the customer coupons page show when salesRepCustomer is null?
  - text: Is the not-found text on the customer coupons page the same as on the customer profile page?
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers/{id}/coupons
  - coordinate: Query.salesRepCoupons
  - coordinate: Query.salesRepCustomer
evidence:
  - method: observation
    deployment: vcst_qa
    build: e967c68919b4671e
    at: 2026-11-03T18:07:13.764Z
    by: session:05e86aa3
    who: Lenajava1
    note: "distinct: KB-B1DBAD5D describes what the coupons list contains for a served customer; this is the access-denied page state for an UNSERVED organization (not-found text, generic heading, no cards, no customer breadcrumb) while the store-wide query still answers."
---
Signed in as a sales rep, opened /company/my-customers/{id} and then /company/my-customers/{id}/coupons for an organization that exists but is not served by the rep. Profile page: 'Customer not found or not in your customers.' with 'Back to my customers'. Coupons page: H1 'Promotion coupons' (no customer name), breadcrumb Home / Account / Sales Rep hub / My customers / Promotion coupons (no customer segment), the same text 'Customer not found or not in your customers.' (also in a status live region) with a 'Back to customer' link, zero coupon cards, organization name absent from the DOM. Network: SalesRepCustomer(organizationId) -> data.salesRepCustomer null, no errors[]; SalesRepCoupons(storeId B2B-store, first 10) still answered totalCount 24 with 10 items. So the page, not the API, withholds the coupons; console had no errors or warnings.

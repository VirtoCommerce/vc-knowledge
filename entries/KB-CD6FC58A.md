---
id: KB-CD6FC58A
subject: Sales Rep Hub customer pickers request salesRepCustomers with first:1000, and a granted customer outside the served set stays listed and untickable in List Share
plane: experiential
question: How do the Sales Rep Hub Customers pickers (Send a message popup, List Share > Specific customers, task customer select) list customers — what page size does Query.salesRepCustomers get, and how is a customer that is not in the fetched options shown?
status: active
appliesTo:
  - axis: module
    value: sales-rep
  - axis: role
    value: sales-rep
  - axis: surface
    value: graphql-xapi
anchors:
  - coordinate: Query.salesRepCustomers
  - coordinate: /company/my-customers
evidence:
  - method: observation
    deployment: vcst_qa
    build: d4814f891c8d3f52
    conditions: module:VirtoCommerce.SalesRep=3.1028.0-pr-40-87bf; role=sales-rep; state:served-organizations=120
    at: 2026-11-24T17:18:32.918Z
    by: session:fb7343ca
    who: Lenajava1
    note: "distinct: KB-87F2142F is the server accepting first:1000 for a 5-customer rep; this is the storefront pickers actually requesting first:1000 with 120 customers, and how each picker shows a customer outside the fetched options (Share keeps it untickable, task form shows No customer)."
---
On vcst with SalesRep 3.1028.0-pr-40-87bf, as a rep serving 120 created organizations: the storefront's customer-options request sends Query.salesRepCustomers with first:1000 and received all 120, so the 115th customer was inside the options (not capped at 100). From the /company/my-customers row envelope on that customer, the Send a message popup opened with it as a named chip; search found it ticked; untick and re-tick worked. In List Share > Specific customers it was ticked and named; Share sent addSharedWithIds/removeSharedWithIds and notified only the added org. The task edit form showed the customer by name and saving unchanged kept customerId. After ADMIN removed that org from the rep's served set (119 served), it was absent from the fetched options, yet the Share dialog still listed it by name, ticked and untickable, and Share sent removeSharedWithIds for it with no notification; the task form showed 'No customer' because the server returns customerId and customerName null for an unserved customer, and saving unchanged left the stored link intact. No raw organization id appeared in any picker.

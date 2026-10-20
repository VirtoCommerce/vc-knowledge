---
id: KB-3F3D3C6B
subject: salesRepCustomerOrders on /graphql/sales-rep answers a non-rep buyer token with HTTP 200, totalCount 0 and no errors[] — an empty result, not a refusal
plane: experiential
question: What does Query.salesRepCustomerOrders on /graphql/sales-rep return to an authenticated buyer who holds no sales-rep:access?
questions:
  - text: Does the scoped sales-rep GraphQL schema refuse a non-rep token on salesRepCustomerOrders?
  - text: Is salesRepCustomerOrders authorization a Forbidden error or data filtering for users without sales-rep:access?
status: active
appliesTo:
  - axis: actor
    value: buyer-without-sales-rep-access
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.salesRepCustomerOrders
  - coordinate: POST /graphql/sales-rep
evidence:
  - method: observation
    deployment: vcst_qa
    build: f6080d062139e9f0
    conditions: module:VirtoCommerce.SalesRep=3.1014.0; theme=3.1.0-pr-11-d67b; store=B2B-store; role=Organization maintainer
    at: 2026-10-20T13:58:35.247Z
    by: session:d992339e
    who: Lenajava1
---
Minted a password-grant token (storeId=B2B-store) for an organization buyer with the Org Admin role and no sales-rep:access, and POSTed salesRepCustomerOrders(storeId:"B2B-store", first:2){totalCount items{id number}} to /graphql/sales-rep. Answer: HTTP 200, data.salesRepCustomerOrders {totalCount:0, items:[]}, no errors[]. The same query with a plain Sales Representative token returned totalCount 172. So the scoped schema does not refuse a non-rep: access is enforced by membership filtering (empty result), consistent with the module enforcing authentication only on this family. No data leaks to the buyer; a test expecting a Forbidden error here will fail by design.

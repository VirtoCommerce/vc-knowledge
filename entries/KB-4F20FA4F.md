---
id: KB-4F20FA4F
subject: A sendCustomerCommunication to 1000 served organizations is accepted and takes about 112 seconds
plane: experiential
question: Does a 1000-organization sendCustomerCommunication complete, and how long does it take?
status: active
appliesTo:
  - axis: module
    value: sales-rep
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.sendCustomerCommunication
  - coordinate: /graphql/sales-rep
  - coordinate: Query.salesRepCustomers
evidence:
  - method: observation
    deployment: vcst_qa
    build: d4814f891c8d3f52
    conditions: module:VirtoCommerce.SalesRep=3.1028.0-pr-40-87bf; background-jobs=RabbitMQ
    at: 2026-11-24T17:32:46.680Z
    by: session:fb7343ca
    who: Lenajava1
---
As a rep serving 1001 organizations (1000 created in the case with a handful of contacts, one shared by two orgs), Mutation.sendCustomerCommunication(organizationIds: 1000 ids, sendPush) on a fresh connection returned HTTP 200 succeeded true after 111.6 s, created one PushMessage with the distinct contacts once and without the sender. The same call with 1001 ids returned in 0.5 s with the cap error. salesRepCustomers(first:1000) returns 1000 of the 1001, and first:1001 and above return all 1001.

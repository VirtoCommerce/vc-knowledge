---
id: KB-3FEA7B03
subject: A sales rep's sendCustomerCommunication push reaches the recipient's Query.pushMessages inbox only after several minutes (about 7–9 min on vcst), while the admin push record exists at once
plane: experiential
question: How long after Mutation.sendCustomerCommunication does the push appear in the recipient's Query.pushMessages inbox?
status: active
appliesTo:
  - axis: module
    value: sales-rep
  - axis: surface
    value: graphql-xapi
anchors:
  - coordinate: Mutation.sendCustomerCommunication
  - coordinate: Query.pushMessages
  - coordinate: POST /api/push-message/search
evidence:
  - method: observation
    deployment: vcst_qa
    build: d4814f891c8d3f52
    conditions: module:VirtoCommerce.SalesRep=3.1028.0-pr-40-87bf; role=sales-rep
    at: 2026-11-24T17:53:08.318Z
    by: session:fb7343ca
    who: Lenajava1
    note: "distinct: KB-6AF43C39 is a push created directly by an admin POST on an older build (seconds); this is a push created by the sales rep's sendCustomerCommunication on SalesRep 3.1028, which goes through a background job and takes minutes."
---
On vcst with SalesRep 3.1028.0-pr-40-87bf, in several independent runs on 2026-11-24: right after a successful Mutation.sendCustomerCommunication (succeeded true, pushSent true) the admin push record (POST /api/push-message/search) already lists the recipients in memberIds with status Sent, but the recipient's own Query.pushMessages (and the storefront notifications page) stay empty for minutes. Observed: about 7 min (first poll at 150 s empty), about 9 min for another probe, and one batch whose recipient rows still did not exist 15+ minutes later before eventually appearing; inbox items carry a createdDate of when the recipient row was materialised, not the send time. Readers who verify delivery should check the admin record's memberIds for the audience and poll the inbox patiently (10–20 min) before calling a push lost.

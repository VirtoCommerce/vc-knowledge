---
id: KB-BAFA4213
subject: sales-rep-documents:write without :read is denied on salesRepDocuments reads
plane: experiential
question: Can a rep holding sales-rep-documents:write but not sales-rep-documents:read list documents via Query.salesRepDocuments?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.salesRepDocuments
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 5c581fd90c2dabb7
    conditions: module:VirtoCommerce.SalesRep=3.1015.0-pr-15-e5bc; role=Sales Representative + Sales Rep Documents Manager (write-only variant)
    at: 2026-10-19T23:44:44.240Z
    by: session:68b36f2b
    who: Aleksandra-Mitricheva
---
A rep whose token carries only sales-rep:access + sales-rep-documents:write gets data.salesRepDocuments null with errors[] 'Access denied.' code Forbidden on /graphql/sales-rep. SalesRepDocumentsQueryBuilder authorizes against sales-rep-documents:read only; the read-or-write disjunction (SalesRepDocumentPermissions) existed only in a pre-merge PR commit and is absent from every release since 3.1006.0 (unchanged blob through 3.1014.0 and pr-15). Note: on stands seeded by a release, the 'Sales Rep Documents Manager' role carries read+write, so composing it does NOT produce a write-only rep; on a stand seeded by the pre-merge build the same role name may carry write only.

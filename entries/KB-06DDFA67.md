---
id: KB-06DDFA67
subject: The storefront returns list sends a status code from ?status= in whatever letter case the link has; the chip shows the raw code and the Filters checkbox stays unticked, while the list itself filters (on a SqlServer stand)
plane: experiential
question: What does /account/returns do with a status code in a non-canonical letter case in the link (e.g. ?status=requested)?
status: active
appliesTo:
  - axis: db-provider
    value: sqlserver
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/returns
  - coordinate: /account/returns?scope=organization
  - coordinate: Query.organizationReturns
  - coordinate: Query.returns
  - coordinate: Query.returnStatuses
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-09T21:44:06.729Z
    by: session:61a7b05a
    who: yuskithedeveloper
---
On theme 2.60.0-pr-2523-466f with Return 3.1005.0-pr-28-fb4f, an organization maintainer opened /account/returns?scope=organization&status=requested. GetOrganizationReturns was sent with statuses:["requested"] exactly as typed and returned the 19 Requested returns (the same rows as status=Requested). The chip read 'requested' (the raw code, because the page looks the label up by exact code in Query.returnStatuses, whose keys are Approved, Canceled, Cancelled, Completed, Draft, New, PartiallyApproved, Processing, Rejected, Requested), and in Filters the 'Requested' checkbox was unticked while the list was filtered by it (Apply disabled). On My returns, ?status=draft likewise sent statuses:["draft"] to Query.returns and listed the buyer's Draft return. The server code does not fold case (ReturnSearchService: statuses.Contains(x.Status)); the match comes from the database comparison, and this stand reports DatabaseProvider SqlServer, so a case-sensitive database may return nothing for the same link. The UI itself only ever writes the canonical keys, so such a link comes from a hand-edited or externally authored URL. The chip's close button and Reset filters clear it.

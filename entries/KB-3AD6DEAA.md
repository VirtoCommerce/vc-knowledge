---
id: KB-3AD6DEAA
subject: A locked organization membership takes that organization out of a sales rep's served set
plane: experiential
question: "locked organization membership (OrganizationMembership isLocked) for a sales representative: does the locked organization still count as served for task customer links and My customers?"
questions:
  - text: can a sales rep link a call to a customer whose membership is locked?
  - text: does salesRepCustomers return organizations whose sales rep membership is locked?
  - text: what does a call linked to a customer show after the rep's membership in that customer is locked?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.createSalesRepTask
  - coordinate: Mutation.updateSalesRepTask
  - coordinate: Query.salesRepCustomers
  - coordinate: VirtoCommerce.SalesRep.Data.Services.SalesRepOrganizationAccessService
evidence:
  - method: source
    deployment: vc-module-sales-rep@7007756
    at: 2026-10-20T16:35:01.413Z
    by: session:70c763de
    who: yuskithedeveloper
    note: "distinct: KB-EA467E9B says a per-organization membership is what makes an organization a rep's customer; it says nothing about a LOCKED membership, which this entry establishes is excluded from every served-set check (OnlyUnlocked = true)."
  - method: observation
    deployment: virtostart
    build: f4e35ed21176a918
    conditions: module:VirtoCommerce.Customer=3.1029.0; role=Advanced Sales Representative
    at: 2026-10-22T13:51:08.472Z
    by: session:da35f399
    bodyHash: f1adbd72a040
    who: Lenajava1
    note: "Observed live: an Advanced Sales Representative served 10 organizations (GET /api/sales-rep/{id} listed all 10, each membership carrying the sales-rep role); 2 of those memberships had isLocked=true. On /graphql/sales-rep, acting as the rep, salesRepCustomers returned the 8 unlocked organizations only, and salesRepOrders returned 19 of the 23 orders placed on the rep's login: the 4 missing were exactly the orders of the 2 locked organizations."
---
No. Every served-set check in the Sales Rep module goes through SalesRepOrganizationAccessService.GetGrantingMembershipsAsync, which searches organization memberships with OnlyUnlocked = true, restricted to the roles that grant sales rep access. A locked membership is therefore treated like an unassigned organization: createSalesRepTask and updateSalesRepTask refuse it as a customerId (the task command handler's ServesOrganizationAsync check), salesRepCustomers and salesRepCustomer leave it out, a task's customer field is hidden for it (SalesRepTaskType reads GetServedOrganizationIdsAsync), and the order, statistics, top-seller, rule and customer-communication reads skip it. Read in source; not observed live with a locked membership.

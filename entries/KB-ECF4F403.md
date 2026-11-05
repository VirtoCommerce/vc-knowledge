---
id: KB-ECF4F403
subject: PR #33 removes the salesRepTaskCounts query and the status badge counts from /company/tasks; the status line is the list's own N tasks
plane: experiential
question: Does /company/tasks still request salesRepTaskCounts and show badge counts on the status chips after vc-module-sales-rep PR #33, and what does the dashboard overdue notice ask for?
status: active
appliesTo:
  - axis: module
    value: sales-rep
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/tasks
  - coordinate: Query.salesRepTasks
  - coordinate: Query.salesRepTaskCounts
evidence:
  - method: source
    deployment: vc-module-sales-rep@3.1018.0-pr-33-8d4a
    conditions: module:VirtoCommerce.SalesRep=3.1018.0-pr-33-8d4a; pr=vc-module-sales-rep#33 (open)
    at: 2026-11-05T14:20:45.620Z
    by: session:ffa9e791
    who: Lenajava1
  - method: source
    deployment: vc-module-sales-rep@7b59be8d
    conditions: pr=vc-module-sales-rep#33 (open); head=7b59be8d
    at: 2026-11-05T14:38:06.133Z
    by: session:ffa9e791
    bodyHash: 4beaa88a94e2
    who: Lenajava1
    note: "Independent source re-read at head 7b59be8d: useSalesRepCustomerOptions OPTIONS_LIMIT = 1000 (was 100) with keyword '' (browser-side search), shared by the Customers filter, the task popup's Customer select and wishlist-customer-sharing; the list renders customerName ?? '—' in its own column and taskSubline(..., {withCustomer:false}) for the desktop table, while the mobile card keeps 'Customer not assigned' (calls only). Not live: UI unreachable on the stale remoteEntry. Server side live: salesRepCustomers first 1000/1001/5000 accepted (KB-87F2142F)."
  - method: observation
    deployment: vcst_qa
    build: 33215a89701bbcde
    conditions: platform=3.1094.0; storefront-plugin-bundle=last-modified 12:23:20; browser=edge
    at: 2026-11-05T15:26:21.313Z
    by: session:ffa9e791
    bodyHash: 4beaa88a94e2
    who: Lenajava1
    note: "Observed live (partial): no badge counts on the page, the only count is the 'N tasks' status line; the desktop Customer column shows an em dash for an unlinked task and the Task cell carries no customer segment. salesRepTaskCounts absence and the 1000 picker limit were not inspected."
---
Read in the source of vc-module-sales-rep PR #33 at head 8d4a30bd; not observed live. The PR deletes salesRepTaskCountsQuery.graphql, useSalesRepTaskCounts, the scope chips component and the status tabs, so the Today, All, Upcoming, Overdue and Completed badge values no longer exist and no salesRepTaskCounts request is sent; the only count on the page is the list status line 'N tasks' (the totalCount of the filtered salesRepTasks request). The dashboard overdue notice uses its own SalesRepOverdueTaskCount query, salesRepTasks(first: 0, filter: overdue, today) asking for totalCount only, no longer sharing a cache entry with a counts query. In the customer column of the table an unlinked task shows an em dash; the text 'Customer not assigned' stays only in the mobile card line. The Customers picker (filter, task popup, list sharing) loads the first 1000 served customers, searched in the browser (was 100).

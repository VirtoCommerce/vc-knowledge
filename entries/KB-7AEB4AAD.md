---
id: KB-7AEB4AAD
subject: Returns cancelled before the submitted-date migration are hidden from organization holders even if they were submitted; their owners still list and open them
plane: experiential
question: Do returns that were cancelled before Return 3.1005.0-pr-28-fb4f (SubmittedDate migration) appear in Query.organizationReturns and open through Query.return for an organization holder?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.organizationReturns
  - coordinate: Query.return
  - coordinate: /account/returns?scope=organization
  - coordinate: /account/returns/{id}
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-09T20:40:48.435Z
    by: session:61a7b05a
    who: yuskithedeveloper
---
On Return 3.1005.0-pr-28-fb4f with theme 2.60.0-pr-2523-466f, the admin return search reports submittedDate empty for every return that was Cancelled before this build (including ones that had been submitted first) and set for the pre-build Requested, Approved and New ones. A holder of xapi:my_organization:return:view who is a member of two organizations saw, on each organization's tab, every pre-build Requested, Approved and New return of that organization, and none of the pre-build Cancelled ones (status Cancelled filter showed only returns cancelled after the upgrade); deep links to those pre-build Cancelled returns showed 'This return was not found'. The buyer who owns them still sees them as Cancelled in My returns and opens their details.

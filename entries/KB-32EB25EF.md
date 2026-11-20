---
id: KB-32EB25EF
subject: A Site Builder page group with a reversed schedule (startDate after endDate) can never match the Active lifecycle filter or ActiveOn; it lists as Pending before its start and Archived once its end passes
plane: experiential
question: Where does a published Site Builder page with startDate after endDate appear in the lifecycle lists?
questions:
  - text: Is a Site Builder page with End date before Start date ever active?
  - text: How do Pending, Active and Archived filters treat a reversed schedule?
status: active
appliesTo:
  - axis: module
    value: site-builder
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: POST /api/site-builder-pages/list
  - coordinate: PUT /api/site-builder-pages/grouped
evidence:
  - method: source
    deployment: vc-module-site-builder@28d00e0
    at: 2026-11-20T07:27:54.405Z
    by: session:07a3812a
    who: Lenajava1
    note: "distinct: This is what a stored reversed range does to the lifecycle lists (Pending/Active/Archived, ActiveOn), read in source, not whether the save is accepted, which KB-0F25C49D covers."
---
GroupedPageSearchService (vc-module-site-builder @28d00e0, lines 110-120 and 145-155): Active = a published page and (StartDate null or <= now) and (EndDate null or >= now); Pending = published and StartDate > now; Archived = all pages archived or EndDate < now; ActiveOn uses the same start/end predicate as Active. With StartDate > EndDate no instant satisfies Active/ActiveOn, so the page is Pending until its start (if published) and Archived as soon as its end passes, even before its start. Whether the storefront delivery path uses this predicate was not traced.

---
id: KB-9A9DA56B
subject: In Site Builder a page in Pending is a published page whose start date is still in the future, not a page waiting for review
plane: experiential
question: What does Pending mean in the Site Builder page list filter (Draft, Pending, Active, Archived)?
questions:
  - text: why is my published page Pending
  - text: Site Builder page statuses and scheduling
  - text: difference between Page Builder Pending and Site Builder Pending
status: active
appliesTo:
  - axis: module
    value: sitebuilder
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/admin-shell#/content/site/{storeId}
  - coordinate: POST /api/site-builder-pages/search
evidence:
  - method: source
    deployment: VirtoCommerce.SiteBuilder@3.1003.0
    at: 2026-11-26T23:30:02.131Z
    by: session:c8a6829d
    who: ivan-kalachikov
---
Read in source: the designer's pageLifecycle maps a page to Archived when its status is Archived or its end date has passed, to Draft when its status is not Published, to Pending when it is Published and its start date is later than now, else Active; the server filter PageLifecycleFilters.Pending and the MCP get_site_overview 'Scheduled' count use the same rule. The Page Builder user guide describes Pending as the review step between Draft and Active (Publish moves Draft to Pending, Publish again to Active). The Admin Shell page list offers the filter options All Pages, Draft, Pending, Active, Archived on vcst_dev.

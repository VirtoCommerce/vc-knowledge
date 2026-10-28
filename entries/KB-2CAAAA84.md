---
id: KB-2CAAAA84
subject: "Page Builder authorization is not permission-scoped: builder:* permissions have no availableScopes, and the resource check only compares the page's StoreId with the user's own StoreId (Administrators bypass)"
plane: experiential
question: Can a Page Builder role be scoped to a store, and what makes POST /api/page-builder-pages/grouped/publishing/{groupId}?unpublishConflicting=true return 403 for the conflicting pages?
questions:
  - text: Are builder:publish / builder:update store-scoped permissions?
  - text: How does PageBuilderAuthorizationHandler decide access to a page?
  - text: Can an admin user publish a page but lack rights on a conflicting page of the same permalink?
  - text: Why does a non-admin back-office user get 403 on every page builder route?
status: active
appliesTo:
  - axis: module
    value: pagebuilder
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/page-builder-pages/grouped/publishing/{id}
  - coordinate: GET /api/platform/security/permissions
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: a5a20e99d84d8f1b
    conditions: module:VirtoCommerce.PageBuilderModule=3.1039.0-pr-179-5c9d
    at: 2026-10-28T14:19:58.627Z
    by: session:44c579c3
    who: Aleksandra-Mitricheva
---
GET /api/platform/security/permissions on PageBuilderModule 3.1039.0-pr-179 lists builder:access/read/create/update/delete/publish/promote/templates/theme/shared-components:* all with availableScopes=[] (store/catalog/order/shipping/marketing permissions do carry scopes on the same stand). In source (fix/VCST-6442 branch) PageBuilderAuthorizationHandler succeeds for the Administrator role; otherwise it loads the ApplicationUser and succeeds only when every resource's StoreId equals user.StoreId (case-insensitive). PublishGroup authorizes the page itself first, then searches conflicts with criteria.StoreId = page.StoreId and requires same StoreId, then authorizes the conflicting groups with the same requirement. Since a non-admin who passed the first check has user.StoreId == page.StoreId == every conflicting group's StoreId, the second 403 ("unpublishConflicting on a page you may not touch") is not reachable through any role or user configuration; it is a defensive branch only.

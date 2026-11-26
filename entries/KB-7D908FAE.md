---
id: KB-7D908FAE
subject: Publishing a Site Builder page does not publish the store's pending header/footer theme drafts; they go live only with Publish theme, which themeSnapshot reflects on the very next query
plane: experiential
question: Does publishing a Site Builder page also publish pending header and footer theme drafts?
questions:
  - text: Does POST grouped/publishing change the themeSnapshot publication number?
  - text: When does a header or footer edit made in the Site Builder designer reach the storefront?
  - text: Is the themeSnapshot cache reset right after POST theme/publish?
status: active
appliesTo:
  - axis: module
    value: sitebuilder
  - axis: surface
    value: graphql
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/site-builder-pages/grouped/publishing/{id}
  - coordinate: POST /api/site-builder/theme/publish
  - coordinate: GET /api/site-builder/theme/status
  - coordinate: Query.themeSnapshot
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; store=B2B-store; setting:SiteBuilder.Store.GitContentEnabled=null
    at: 2026-11-19T14:21:14.908Z
    by: session:51ac2fe3
    who: Lenajava1
    note: "distinct: KB-5748288B is about the first read of a store with no publication; this is about what a page publish does to pending header/footer theme drafts and when theme/publish shows in the snapshot."
  - method: observation
    deployment: vcst_dev
    build: 858d3d28d721a302
    conditions: platform=3.1100.0; module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-01b3; theme=2.63.0-pr-2698-faeb-faeba500; store=B2B-store
    at: 2026-11-26T17:55:49.824Z
    by: session:c8a6829d
    bodyHash: 4f320e31a65a
    who: ivan-kalachikov
    note: Designer Publish on a page with pending header+footer drafts sent one grouped/publishing and no theme/publish; themeSnapshot publication and drafts unchanged. theme/publish -> snapshot answered the new number 166 ms after the 200.
---
B2B-store, Vue designer on a new landing page: added an announcement bar to the Header group, a menu block to the Footer group and a Text section to the page, Save -> exactly one POST designer/save carrying the page and both theme files; theme/status then listed drafts [sections/header-group.json, sections/footer-group.json] at publication 12. Designer Publish -> one POST /api/site-builder-pages/grouped/publishing/{groupId}?publish=true, no theme/publish. Afterwards theme/status still publication 12 with both drafts pending, Query.themeSnapshot still publication 12 with the old header/footer, and the storefront permalink showed the page body without the announcement or footer heading. Publish theme (2) -> POST theme/publish 200 {publication:13}; an anonymous themeSnapshot read 8 s later, and on a later publish 168 ms after the 200, already answered the new number with the drafts in header/footer. theme/status and designer/theme-file reads lagged a designer/save or publish by up to ~20 s (old draft list / old content) before catching up.

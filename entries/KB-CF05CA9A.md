---
id: KB-CF05CA9A
subject: PUT /api/page-builder-multilanguage-pages accepts any organizationId string and copies it to every version, and silently ignores a changed storeId (200, store unchanged)
plane: experiential
question: Does updating a Multilanguage Page validate organizationId and storeId?
questions:
  - text: Can a Multilanguage Page be moved to another store by PUT?
  - text: Is the Multilanguage Page organization validated?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pagebuildermodule
  - axis: surface
    value: admin-api
anchors:
  - coordinate: PUT /api/page-builder-multilanguage-pages
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 706d29d69470a72c
    conditions: module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-175-a3bc; platform=3.1085.0
    at: 2026-10-23T13:00:32.719Z
    by: session:0204f22f
    who: Aleksandra-Mitricheva
    note: "distinct: KB-988AA72D is about DefaultCultureName validation on POST vs PUT; this is about organizationId (not validated, propagated) and storeId (ignored) on PUT"
---
PUT of an existing root with organizationId 'AGENT-TEST-…-not-an-org' returned 200 and every language version's organizationId became that string. PUT of the same root with storeId 'Electronics' (an existing store) returned 200 with storeId still 'B2B-store' in the response and on GET: the change is ignored without an error. PUT with an unknown id returns 404.

---
id: KB-EC76B5AB
subject: POST grouped/{id}/content-json accepts an invalid settings.permalink (204) into a new Draft; after publish the content's settings.permalink diverges from the group permalink, and routing keeps using the group permalink
plane: experiential
question: Can the page-builder content-json endpoint store a permalink the group API rejects, and which permalink does the storefront route by?
questions:
  - text: Does content-json validate settings.permalink?
  - text: Which permalink do pageDocument and slugInfo use when page content settings carry a different permalink?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pagebuildermodule
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/page-builder-pages/grouped/{id}/content-json
  - coordinate: Query.pageDocument
  - coordinate: Query.slugInfo
evidence:
  - method: observation
    deployment: local
    build: 3a492aac51b809f5
    conditions: module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-178-e4ff; setting:VirtoPages.Enable=true; store=B2B-store
    at: 2026-10-22T15:05:39.494Z
    by: session:2c92b249
    who: Aleksandra-Mitricheva
---
On a published, valid page (/agent-test-6443-4a-journey), GET grouped/{id}/content returned an ETag; POST grouped/{id}/content-json with If-Match and content settings.permalink '/agent-test-6443 4a via content?z' returned 204. GET grouped/{id} kept permalink '/agent-test-6443-4a-journey', status Published, hasChanges true, and a new Draft page appeared beside the Published one; the published xAPI pageDocument was unchanged. After POST grouped/publishing/{id}?publish=true the new page id became the Published one: xAPI pageDocument.permalink and pageDocuments list stay '/agent-test-6443-4a-journey' while pageDocument.content settings.permalink is the invalid value; slugInfo resolves only the group permalink (the invalid value returns null) and the storefront still opens the page at /agent-test-6443-4a-journey. The server-rendered HTML carries no canonical link.

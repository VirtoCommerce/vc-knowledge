---
id: KB-B5DF20A1
subject: A signed Sanity webhook indexes the page with the title fetched from Sanity; a title in the webhook body is ignored
plane: experiential
question: Does POST /api/pages/sanity index the page title from the webhook body or from the document fetched from Sanity?
questions:
  - text: Is the Sanity webhook payload treated as content or as a notification?
  - text: If the Sanity webhook body carries a title, which title ends up in the Pages index?
status: active
appliesTo:
  - axis: surface
    value: rest-api
  - axis: surface
    value: xapi
anchors:
  - coordinate: POST /api/pages/sanity
  - coordinate: POST /api/pages/search
  - coordinate: Query.pageDocument
  - coordinate: Query.slugInfo
evidence:
  - method: observation
    deployment: vcptcore_qa
    conditions: module:VirtoCommerce.Sanity=3.1004.0-pr-4-f7a8; module:VirtoCommerce.Pages=3.1011.0-pr-24-8c93; setting:Sanity.Enabled=true
    at: 2026-10-28T22:51:44.840Z
    by: session:9818a58d
    who: kutasinaelena
---
Page A was absent from the Pages index (POST /api/pages/search read fully with take 500 + skip paging, 631 docs, 0 hits). A signed POST /api/pages/sanity update notification for it whose body also carried a decoy title "... (webhook-body decoy)" returned 200; within ~1 s pages/search held the page exactly once, Status Published, with title equal to the title stored in Sanity, not the decoy. xAPI pageDocument(id) returned the same id, the Sanity title and content.hero.asset.url (Sanity CDN url). A signed delete removed it from pages/search within ~1 s and slugInfo(permalink) then returned entityInfo null. The webhook body is a notification only; content is fetched from Sanity.

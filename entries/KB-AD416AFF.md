---
id: KB-AD416AFF
subject: A Multilanguage Page translation becomes Outdated when the source language is published (single publish or publish-all), not on a source draft save; translation publish and mark-up-to-date clear it; two concurrent publish-all calls can lose the source revision update
plane: experiential
question: When are Multilanguage Page translations marked Outdated and what clears it?
questions:
  - text: Does saving a source draft mark translations Outdated?
  - text: Does publish-all mark other languages Outdated?
  - text: What happens with two concurrent publish-all requests?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pagebuildermodule
  - axis: surface
    value: admin-api
anchors:
  - coordinate: POST /api/page-builder-multilanguage-pages/{id}/publish-all
  - coordinate: POST /api/page-builder-multilanguage-pages/mark-up-to-date/{id}
  - coordinate: GET /api/page-builder-multilanguage-pages/{id}/languages
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 706d29d69470a72c
    conditions: module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-175-a3bc; platform=3.1085.0
    at: 2026-10-23T13:00:01.011Z
    by: session:0204f22f
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcptcore_qa
    build: f1db890a6a93d133
    conditions: module:VirtoCommerce.PageBuilderModule=3.1039.0-pr-175-3d2f; platform=3.1085.0
    at: 2026-10-27T19:05:31.277Z
    by: session:b715861d
    bodyHash: 7bb9c67dc144
    who: Aleksandra-Mitricheva
    contradicts: true
    contested: module:VirtoCommerce.PageBuilderModule 3.1039.0-pr-175-3d2f vs 3.1036.0-pr-175-a3bc; virtocommerce.pagebuildermodule 3.1039.0-pr-175-3d2f vs 3.1036.0-pr-175-a3bc; virtocommerce.pages 3.1011.0-pr-23-e50f vs 3.1011.0-pr-23-4bcd
    note: "On PageBuilderModule 3.1039.0-pr-175-3d2f only the race clause no longer holds; the Outdated rules held again (source publish flags translations incl. never-published drafts created as copies, Mark up to date and translation publish clear it). 10 rounds of two parallel POST /{id}/publish-all calls on 10 roots right after a source draft save (5 with only an en-US draft, 5 with en-US + fr-FR drafts): in every round exactly one call processed en-US (and fr-FR) and the other reported every language skipped 'NoDraft'; the source sourceRevisionDate advanced every time and the translations without a draft became Outdated, the republished fr-FR did not. A parallel archive-all pair: one call archived all three, the other reported them 'AlreadyArchived'."
  - method: observation
    deployment: vcptcore_qa
    build: f1db890a6a93d133
    conditions: module:VirtoCommerce.PageBuilderModule=3.1039.0-pr-175-3d2f
    at: 2026-10-27T19:05:54.666Z
    by: session:b715861d
    bodyHash: 7bb9c67dc144
    who: Aleksandra-Mitricheva
    note: Outdated rules only (source publish flags translations; UI Mark up to date and translation publish clear it) re-observed on 3.1039.0-pr-175-3d2f; the concurrency clause is disputed separately.
---
Root with en-US (source), fr-FR, de-DE published and a pt-PT draft. Saving new en-US draft content (POST /api/page-builder-pages/grouped/{id}/content-json with eTag) left every isOutdated false. Publishing en-US (version blade Publish, or POST /{id}/publish-all with only en-US having a draft) set isOutdated true on fr-FR, de-DE and the pt-PT draft; statuses stayed Published/Draft. Publishing de-DE cleared it; POST /mark-up-to-date/{groupId} (UI 'Mark up to date', shown only on Outdated rows, no toast) cleared fr-FR. Archived versions are never flagged. sourceRevisionDate of the source equals its last publish time and translations compare against it. In 1 of 5 rounds of two parallel publish-all calls right after a source draft save, both calls reported en-US processed and the source's sourceRevisionDate did NOT advance, so the translations were not flagged Outdated although the source content changed; in the other 4 rounds one call processed and the other skipped, end state correct. The admin UI sends one request on a double-click.

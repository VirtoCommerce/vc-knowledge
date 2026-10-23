---
id: KB-D0ED8CDD
subject: PageBuilder page REST endpoints reject an invalid permalink with 400 text/plain; PUT validates only a changed permalink, case-only change counts as changed
plane: experiential
question: What do the PageBuilder page REST endpoints return for a permalink with spaces, ?, #, &, \, // or ./.. segments, and when does PUT check it?
questions:
  - text: Does PUT /api/page-builder-pages/grouped re-validate an unchanged legacy permalink?
  - text: Is a 400 on PUT grouped a partial save of the other fields?
  - text: Which permalink characters does create-group-page accept?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pagebuildermodule
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/page-builder-pages/create-group-page
  - coordinate: POST /api/page-builder-pages/grouped
  - coordinate: PUT /api/page-builder-pages/grouped
evidence:
  - method: observation
    deployment: local
    build: 3a492aac51b809f5
    conditions: platform=3.1089.0; module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-178-e4ff
    at: 2026-10-22T15:15:06.959Z
    by: session:2c92b249
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcst_qa
    build: 4fd52d6a347c726e
    conditions: platform=3.1090.0-pr-3133-8b4a; module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-178-add2; store=B2B-store
    at: 2026-10-23T10:47:52.050Z
    by: session:562b04df
    bodyHash: f5a1b92bd2d7
    who: Aleksandra-Mitricheva
    contradicts: true
    contested: platform 3.1090.0-pr-3133-8b4a vs 3.1089.0; module:VirtoCommerce.PageBuilderModule 3.1036.0-pr-178-add2 vs 3.1036.0-pr-178-e4ff; virtocommerce.pagebuildermodule 3.1036.0-pr-178-add2 vs 3.1036.0-pr-178-e4ff
    note: "On PageBuilderModule 3.1036.0-pr-178-add2 the rule was widened (author decision): create-group-page, POST grouped and PUT grouped now also return 400 text/plain for %, %20, %2F, \" < > { } | ^ backtick, BMP Cf (U+200B, U+00AD, U+200E, U+2060, U+200D) and length > 2048, with body \"Permalink must be at most 2048 characters and cannot contain whitespace, control or invisible formatting characters, the characters ? # & % \\ \" < > { } | ^ `, empty segments ('//') or '.' and '..' segments.\" Still accepted verbatim: Cyrillic, Japanese, full-width ？＃＆＼, no leading slash, lone emoji, U+FE0F. Unchanged legacy permalink still exempt on PUT (incl. a stored % and < value); case-only change still 400; rejected PUT leaves the entity byte-identical."
---
On PageBuilderModule 3.1036.0-pr-178: POST /api/page-builder-pages/create-group-page, POST /api/page-builder-pages/grouped and PUT /api/page-builder-pages/grouped return 400, content-type text/plain; charset=utf-8, body "Permalink cannot contain whitespace, control characters, the characters '?', '#', '&' or '\', empty segments ('//') or '.' and '..' segments." for leading/trailing/inner space, tab, LF, NBSP, U+3000, U+0085, U+FEFF, U+0001, ?, #, &, \, //a, /a//b, ".", /./a, /a/.. — nothing is stored (search finds no page). Accepted and stored verbatim: Cyrillic, Japanese, %20, a bare %, trailing slash, "/", and also empty string, no leading slash, U+200B, "<>{}|^ and full-width ？＃＆＼. PUT with an invalid changed permalink plus a changed name returns 400 and the stored entity is byte-identical (no partial save). PUT of a page whose stored permalink is already invalid succeeds when the permalink is sent unchanged (name change persisted), but a case-only change (S7 -> s7) or another invalid value returns 400. A user without the Create permission gets 403 (empty body) before validation on create-group-page and POST grouped. POST grouped/publishing/{id} needs ?publish=true; without it a never-published page answers 400 "Published page not found."

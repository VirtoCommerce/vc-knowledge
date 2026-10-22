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
---
On PageBuilderModule 3.1036.0-pr-178: POST /api/page-builder-pages/create-group-page, POST /api/page-builder-pages/grouped and PUT /api/page-builder-pages/grouped return 400, content-type text/plain; charset=utf-8, body "Permalink cannot contain whitespace, control characters, the characters '?', '#', '&' or '\', empty segments ('//') or '.' and '..' segments." for leading/trailing/inner space, tab, LF, NBSP, U+3000, U+0085, U+FEFF, U+0001, ?, #, &, \, //a, /a//b, ".", /./a, /a/.. — nothing is stored (search finds no page). Accepted and stored verbatim: Cyrillic, Japanese, %20, a bare %, trailing slash, "/", and also empty string, no leading slash, U+200B, "<>{}|^ and full-width ？＃＆＼. PUT with an invalid changed permalink plus a changed name returns 400 and the stored entity is byte-identical (no partial save). PUT of a page whose stored permalink is already invalid succeeds when the permalink is sent unchanged (name change persisted), but a case-only change (S7 -> s7) or another invalid value returns 400. A user without the Create permission gets 403 (empty body) before validation on create-group-page and POST grouped. POST grouped/publishing/{id} needs ?publish=true; without it a never-published page answers 400 "Published page not found."

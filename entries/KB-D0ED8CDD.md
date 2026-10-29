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
    resolved: version-scoped
    resolvedAt: 2026-10-29T10:00:55.037Z
    resolvedBy: session:4d39d1e8
    resolvedIn: judge/batch-20261029-1343
    resolution: The accepted list (%, %20, U+200B, "<>{}|^) is the PR 178 e4ff build's; add254f0 widened the rule and released PageBuilderModule 3.1038.0 refuses all of them with 400, as observed on vcst_qa on 2026-10-29; the PUT changed-only and case-only clauses were not re-checked and keep their wording.
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1090.0; module:VirtoCommerce.PageBuilderModule=3.1038.0; store=B2B-store; role=administrator; db=SqlServer
    at: 2026-10-29T10:00:52.426Z
    by: session:4d39d1e8
    note: "2026-10-29: create-group-page and POST grouped returned 400 text/plain with the widened InvalidMessage for space, ?, #, &, \\, %, %20, \", <>, U+200B, U+2060, U+E0041, U+1D173 and a 2049-char permalink; 2048 chars and Cyrillic returned 200. PUT grouped changing a valid page's permalink to the 2049-char value, U+E0041, '?x' or a space returned 400 and GET showed permalink and modifiedDate unchanged; a valid change returned 200. Test pages deleted."
    investigation: true
    bodyHash: 5bccfd9bb59e
---
On PageBuilderModule 3.1036.0-pr-178: POST /api/page-builder-pages/create-group-page, POST /api/page-builder-pages/grouped and PUT /api/page-builder-pages/grouped return 400, content-type text/plain; charset=utf-8, body "Permalink cannot contain whitespace, control characters, the characters '?', '#', '&' or '\', empty segments ('//') or '.' and '..' segments." for leading/trailing/inner space, tab, LF, NBSP, U+3000, U+0085, U+FEFF, U+0001, ?, #, &, \, //a, /a//b, ".", /./a, /a/.. — nothing is stored (search finds no page). Accepted and stored verbatim: Cyrillic, Japanese, %20, a bare %, trailing slash, "/", and also empty string, no leading slash, U+200B, "<>{}|^ and full-width ？＃＆＼. PUT with an invalid changed permalink plus a changed name returns 400 and the stored entity is byte-identical (no partial save). PUT of a page whose stored permalink is already invalid succeeds when the permalink is sent unchanged (name change persisted), but a case-only change (S7 -> s7) or another invalid value returns 400. A user without the Create permission gets 403 (empty body) before validation on create-group-page and POST grouped. POST grouped/publishing/{id} needs ?publish=true; without it a never-published page answers 400 "Published page not found."

Build range (judged 2026-10-29): the accepted-values list above describes the pre-release PR 178 build 3.1036.0-pr-178-e4ff only. Commit add254f0 widened the rule, and the released VirtoCommerce.PageBuilderModule 3.1038.0 also rejects %, %20, " < > { } | ^ `, BMP format characters (U+200B, U+2060) and, per code point (commit 61e8ba05), astral format characters such as U+E0041 and U+1D173, plus any permalink longer than 2048 UTF-16 characters. The 400 body on 3.1038.0 reads: "Permalink must be at most 2048 characters and cannot contain whitespace, control or invisible formatting characters, the characters ? # & % \ " < > { } | ^ `, empty segments ('//') or '.' and '..' segments." Cyrillic is still accepted and stored verbatim. The PUT clauses (only a changed permalink is validated, a case-only change counts as changed, a stored invalid permalink can be re-saved unchanged) were not re-checked; a rejected PUT still leaves the stored group unchanged.

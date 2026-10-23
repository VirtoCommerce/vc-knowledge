---
id: KB-7BDB128B
subject: On PageBuilderModule 3.1036.0-pr-178-add2 the Page Builder shell Permalink field rejects %, spaces and & ? # with an inline error and disabled Save, while Cyrillic saves verbatim
plane: experiential
question: What does the Page Builder shell page blade do with a permalink containing %, a space or & ? # on PageBuilderModule 3.1036.0-pr-178-add2?
questions:
  - text: Is percent allowed in a Page Builder permalink in the shell after the add2 rule change?
  - text: What is the inline permalink error text in the Page Builder shell on pr-178-add2?
  - text: Does the Page Builder shell save a Cyrillic permalink unchanged?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pagebuildermodule
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/page-builder-shell#/page-builder-draft/details
  - coordinate: POST /api/page-builder-pages/create-group-page
  - coordinate: POST /api/page-builder-pages/grouped
evidence:
  - method: observation
    deployment: vcst_qa
    build: 4fd52d6a347c726e
    conditions: module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-178-add2; store=B2B-store; role=Administrator
    at: 2026-10-23T11:12:01.562Z
    by: session:562b04df
    who: Aleksandra-Mitricheva
---
As administrator in /apps/page-builder-shell (store B2B-store), New page blade with Name and Language en-US set: typing '/my page test', '/page&test?q=1#anchor' or (on a reopened saved page) '/страница-тест-50%' marks the Permalink field invalid, shows the alert "Permalink must be at most 2048 characters and cannot contain spaces or invisible characters, the characters ? # & % \ \" < > { } | ^ `, double slashes, or . and .. segments", keeps the typed value unaltered and leaves Save disabled. The REST twin POST /api/page-builder-pages/create-group-page returns 400 text/plain with a differently worded message ("...cannot contain whitespace, control or invisible formatting characters, the characters ? # & % \ ... empty segments ('//') or '.' and '..' segments.") and stores nothing. '/страница-тест' enables Save, POST /api/page-builder-pages/grouped returns 200, and the reopened blade and GET grouped/{id} hold exactly '/страница-тест'. So on add2 '%' is no longer allowed (it was on 3.1036.0-pr-178-e4ff), and the shell and REST messages differ in wording.

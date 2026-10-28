---
id: KB-BEC9CFB5
subject: Page Builder shell page form warns under Permalink with plain-text names of other same-permalink pages and gates Publish with Certain/Possible/refused dialogs
plane: experiential
question: "page-builder-shell admin: what happens when a page permalink is used by another page — warning under permalink, publish dialogs"
questions:
  - text: What does the page builder admin show when two pages share a permalink?
  - text: What dialog appears when publishing a page whose permalink is already published?
  - text: Does the page builder UI block saving a page with a duplicate permalink?
  - text: What happens when publishing over a page that has unpublished changes in the page builder UI?
status: active
appliesTo:
  - axis: app
    value: page-builder-shell
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/page-builder-shell
  - coordinate: POST /api/page-builder-pages/grouped/permalink-usages
  - coordinate: POST /api/page-builder-pages/grouped/publishing/{id}
evidence:
  - method: observation
    deployment: vcptcore_qa
    conditions: module:VirtoCommerce.PageBuilderModule=3.1039.0-pr-179-5c9d; module:VirtoCommerce.Pages=3.1011.0-pr-24-8c93; store=B2B-store; role=Administrator
    at: 2026-10-28T17:00:31.260Z
    by: session:44c579c3
    who: Aleksandra-Mitricheva
---
Page Builder shell (PageBuilderModule 3.1039.0-pr-179). Opening a page form calls POST /api/page-builder-pages/grouped/permalink-usages (debounced on permalink typing: one request for the final value) and shows a warning alert under Permalink: 'Other pages use this permalink: "<name>", "<name>". This is fine for other visitors or dates; publishing checks for conflicts.' Names are plain quoted text, not links. Same-language and any-language pages are listed; other-language and Archived pages are not (the API response itself still contains the other-language page; the UI filters it). Save/create with a used permalink is allowed. Publish re-checks usages client-side: Certain -> 'Confirmation' dialog 'Already published at this permalink for the same visitors: "<A>". Unpublish them and publish this page?' (Confirm sends grouped/publishing/{id}?publish=true&unpublishConflicting=true, A becomes Draft; Cancel/Escape sends nothing). Possible -> 'Also published at this permalink for some of the same visitors or dates: "<A>". Where both pages apply, the one published last is shown. Publish anyway?' (Confirm publishes without the flag, both stay Published). Both kinds -> two dialogs in sequence; cancelling the second sends nothing. Certain over a page with unpublished changes -> 'Error' dialog 'Already published at this permalink for the same visitors: "<A>". They have unpublished changes, so publish or discard those changes first.' with OK, nothing changes. General page next to an org-specific page publishes with no prompt. Double-click on Publish or Confirm sends one publishing request. Clone gives '<name> (copy)' and '<permalink>-copy'.

---
id: KB-C3D589EA
subject: Page Builder Clone names the copy '<name> (copy)' / '<permalink>-copy' and increments to '(copy N)' / '-copy-N' when cloning a copy, but cloning the original twice yields duplicate '(copy)' names and permalinks
plane: experiential
question: How does Page Builder name and permalink a cloned page, and what happens on a second clone?
status: active
appliesTo:
  - axis: module
    value: page-builder
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/page-builder-shell
  - coordinate: POST /api/page-builder-pages/grouped
  - coordinate: POST /api/page-builder-pages/grouped/{targetGroupId}/content/{sourceGroupId}
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T08:45:04.298Z
    by: session:b1874592
    who: Lenajava1
    build: c4000d0bea152759
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: 4fd52d6a347c726e
    conditions: module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-178-add2
    at: 2026-10-23T11:04:13.063Z
    by: session:562b04df
    bodyHash: dc8b4e2fdf71
    who: Aleksandra-Mitricheva
    note: Clone of published valid page created Draft '<name> (copy)' / '<permalink>-copy', toast 'Page cloned successfully'. Clone of a page whose stored permalink is legacy-invalid now returns 400 (POST /api/page-builder-pages/grouped) with the permalink rule text in a blade alert + toast; no copy created.
  - method: observation
    deployment: vcptcore_qa
    build: 706d29d69470a72c
    conditions: module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-175-a3bc
    at: 2026-10-23T12:59:31.813Z
    by: session:0204f22f
    bodyHash: dc8b4e2fdf71
    who: Aleksandra-Mitricheva
    note: "Also holds for Clone on a Multilanguage language-version blade: two clones of the same de-DE version produced two standalone Landing pages '<name> (copy)' with the same '-copy' permalink."
---
On PageBuilderModule 3.1033.0-pr-170-236e: Clone of 'X' creates a Draft page 'X (copy)' with permalink '<p>-copy' and identical content (POST /api/page-builder-pages/grouped then content copy). Cloning 'X (copy)' produces 'X (copy 2)' / '-copy-2' (not '(copy) (copy)'); cloning that gives '(copy 3)'. Cloning the original 'X' again while 'X (copy)' exists creates a second 'X (copy)' with the same '-copy' permalink — no collision check, no error, both Drafts coexist. A clone of a Published page is created as Draft. Naming logic: incrementCopyName/incrementCopyPermalink in the shell's usePageBuilderDetails composable, applied to the source's own name only.

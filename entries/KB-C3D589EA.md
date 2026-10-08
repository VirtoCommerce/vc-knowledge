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
---
On PageBuilderModule 3.1033.0-pr-170-236e: Clone of 'X' creates a Draft page 'X (copy)' with permalink '<p>-copy' and identical content (POST /api/page-builder-pages/grouped then content copy). Cloning 'X (copy)' produces 'X (copy 2)' / '-copy-2' (not '(copy) (copy)'); cloning that gives '(copy 3)'. Cloning the original 'X' again while 'X (copy)' exists creates a second 'X (copy)' with the same '-copy' permalink — no collision check, no error, both Drafts coexist. A clone of a Published page is created as Draft. Naming logic: incrementCopyName/incrementCopyPermalink in the shell's usePageBuilderDetails composable, applied to the source's own name only.

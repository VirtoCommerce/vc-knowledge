---
id: KB-FE729FD7
subject: The Site Builder shell Draft list also lists Published pages that carry a pending draft, badged 'Published' + 'Has draft with changes'
plane: experiential
question: Which pages does the Site Builder shell Draft list (lifecycle drafts) contain, and what badges do they show?
questions:
  - text: Why does the Site Builder Draft list show a Published badge?
  - text: Does POST /api/site-builder-pages/list lifecycle drafts include published pages with unpublished edits?
  - text: Do the Site Builder sidebar counters equal the list rows?
status: active
appliesTo:
  - axis: module
    value: site-builder
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: POST /api/site-builder-pages/list
  - coordinate: /apps/site-builder-shell
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: platform=3.1099.0; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; store=B2B-store; role=admin
    at: 2026-11-19T20:12:34.417Z
    by: session:07a3812a
    who: Lenajava1
---
On B2B-store, #/site-builder-draft (POST /api/site-builder-pages/list {lifecycle:'drafts'}) returned 38 rows = the Draft sidebar counter: 33 Draft pages plus 5 pages whose group status is Published with hasChanges true (pages[] holds a Draft and a Published version); those 5 rows show the badges 'Published', 'Has draft with changes' (and 'Personalized' where set). A clean Published page with no pending draft is not listed. The Active list (lifecycle 'active') held only Published pages inside their start/end window; a future-start Published page is listed under Pending with 'Published, Scheduled'. Scheduled/Personalized badges accompany the status badge, never replace it. Sidebar counters are display-capped at '99+' (Archived read 99+ while the list held 169 rows) and refresh a few seconds after a status change. Columns: Name, Type, Languages, Permalink, Modified, Modified by, Status.

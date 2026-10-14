---
id: KB-AF27FAE8
subject: Creating a Page Builder page while the Pages search index cannot be created returns 500 with the raw provider error, yet the page is persisted
plane: experiential
question: What happens when a Page Builder page is created while the Pages search index cannot be created — does the create fail and is anything persisted?
status: active
appliesTo:
  - axis: build
    value: pagebuildermodule 3.1035.0-pr-171-61df
  - axis: search-provider
    value: azure
  - axis: surface
    value: page-builder-shell
anchors:
  - coordinate: POST /api/page-builder-pages/grouped
  - coordinate: Query.pageContext
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-14T13:31:19.966Z
    by: session:8b33e471
    who: kutasinaelena
---
On PageBuilderModule 3.1035.0-pr-171-61df with the Azure Search provider, the provider refused to create the Pages index (429 index quota exceeded). Creating a Draft page from the Page Builder shell (New page -> Name/Permalink/Language -> Save) made POST /api/page-builder-pages/grouped return 500; the blade showed the full raw Azure SDK error (index name, service name, request headers) as a toast and kept 'Has unsaved changes'. After Refresh the page was listed with the entered name and permalink and could be opened by its id — the write was committed and only the post-save indexing step failed, so the UI reported a failure for a write that succeeded (a retry would create a duplicate). In the same state the storefront Query.pageContext failed with code SEARCH for every permalink (slugInfo resolves through search), leaving the storefront and the Designer preview iframe blank and the Designer block library with no block types.

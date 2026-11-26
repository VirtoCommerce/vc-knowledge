---
id: KB-0E521EFB
subject: Facet term values are ordered by the store's per-property Sorting option in Catalog AggregationConverter; PriorityAscending/Descending use dictionary item priority, legacy "Priority" now means ascending
plane: experiential
question: How does the facet value Sorting option (Name/Numeric/Priority ascending/descending, Score) on a store Aggregation property order term_facets values in xAPI Query.products?
questions:
  - text: Does Priority ascending put the smallest dictionary priority number first in facet values?
  - text: Where do facet values without a dictionary item go when sorting by priority?
  - text: What does the Score sorting of facet values mean?
  - text: Does vc-frontend re-sort term facet values on the catalog page?
  - text: What does the legacy Priority facet sorting value map to after VCST-5851?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.products
evidence:
  - method: source
    deployment: vc-module-catalog@a1d2408
    at: 2026-11-05T07:38:55.951Z
    by: session:24aa76cb
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: store=B2B-store; module:VirtoCommerce.Catalog=3.1051.0; module:VirtoCommerce.Xapi=3.1029.0; module:VirtoCommerce.ElasticSearch8=3.1012.0-pr-47-d205
    at: 2026-11-05T07:42:25.695Z
    by: session:24aa76cb
    bodyHash: 8c79390bb730
    who: Lenajava1
    note: "Observed live: set Color through PUT /api/catalog/aggregationproperties/{storeId}/properties to each value in turn and read Query.products term_facets (no facet argument). NameAscending/NameDescending gave A-Z/Z-A by label; PriorityAscending and legacy \"Priority\" gave identical order (smallest priority first); PriorityDescending gave the reverse, with values that have no dictionary item (Blue & Gray, Gray, Gray & Black) last A-Z in both directions; ties broken A-Z (Brown before Grey, both 7). Score gave count desc then term A-Z. NumericAscending on the non-numeric Color fell back to A-Z; on Year (2020-2022) Numeric asc/desc ordered correctly (too few values to tell string from numeric order). The change applied immediately, with no reindex. The priority is the smallest non-zero value across same-named Color properties of linked catalogs; a Color property defined on the store's own (virtual) catalog was NOT counted (Gray has priority 1 there yet sorted as unprioritised). Storefront /catalog Color dropdown showed the exact xAPI order (no client re-sort). Only the top 20 values were returned, with size 0 configured."
  - method: observation
    deployment: vcst_qa
    build: c67268c734322d94
    conditions: store=B2B-store; storeCatalog=virtual; theme=2.63.0-alpha.2681
    at: 2026-11-26T14:58:45.879Z
    by: session:d2ee4088
    bodyHash: 8c79390bb730
    who: Lenajava1
    note: "Re-observed 2026-11-26 on /catalog (anonymous, default in-stock filter) with Color TermValuesSortingType set through the store setting Catalog.BrowseFilters.FilteredBrowsing (full-body PUT /api/stores): PriorityAscending and legacy \"Priority\" gave identical order; PriorityDescending gave highest first with ties A-Z in both directions and no-dictionary values (Gray, Gray & Black) last A-Z in both directions; Gray has priority 1 only on the virtual store catalog's own Color dictionary and still sorted as unprioritised. Name asc/desc A-Z/Z-A; Score = count desc then A-Z; NumericAscending on Weight (0.08..9.29, 21.8, 22.7, 32, 72) ordered numerically, not as strings, and on Resolution put the one numeric value (1800) first, then non-numeric in name order. Legacy \"Priority\" stayed \"Priority\" in GET /api/catalog/aggregationproperties/{storeId}/properties after the storefront read and after opening (not saving) the admin Facets > Color blade, which displayed \"Priority ascending\". DOM order == Query.products term_facets order."
---
Read in source, not observed live. In vc-module-catalog AggregationConverter.SortAggregationItemsAsync, each attribute (attr) aggregation is sorted by its AttributeFilter.TermValuesSortingType (Admin: Store > Aggregation properties > property > Sorting dropdown: Name ascending/descending, Numeric ascending/descending, Priority ascending/descending, Score). NameAscending/NameDescending: OrderBy raw value; xAPI FacetMapper.SortTermFacetResultByLabels then re-sorts by the localized Label for these two only, so the label order wins. NumericAscending/Descending: values parsed as double; non-numeric values always go last, alphabetical, in both directions. PriorityAscending: smallest dictionary item SortOrder first; PriorityDescending: largest first; values with no dictionary item always last; ties broken by value case-insensitive A-Z; for same-named properties (linked catalogs) the smallest non-zero SortOrder per alias is used. Legacy constant "Priority" (stores saved before VCST-5851, catalog commit 2026-09-10) behaves as PriorityAscending — the earlier code sorted "Priority" by SortOrder descending, so such a store's order flips after upgrade. Score (or any other value): engine order kept. Storefront vc-frontend useProducts calls termFacetToCommonFacet(f, false), so the catalog page shows the server order without client re-sort. Docs (Platform user guide, Configure facets) still list a single "Priority" option.

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
---
Read in source, not observed live. In vc-module-catalog AggregationConverter.SortAggregationItemsAsync, each attribute (attr) aggregation is sorted by its AttributeFilter.TermValuesSortingType (Admin: Store > Aggregation properties > property > Sorting dropdown: Name ascending/descending, Numeric ascending/descending, Priority ascending/descending, Score). NameAscending/NameDescending: OrderBy raw value; xAPI FacetMapper.SortTermFacetResultByLabels then re-sorts by the localized Label for these two only, so the label order wins. NumericAscending/Descending: values parsed as double; non-numeric values always go last, alphabetical, in both directions. PriorityAscending: smallest dictionary item SortOrder first; PriorityDescending: largest first; values with no dictionary item always last; ties broken by value case-insensitive A-Z; for same-named properties (linked catalogs) the smallest non-zero SortOrder per alias is used. Legacy constant "Priority" (stores saved before VCST-5851, catalog commit 2026-09-10) behaves as PriorityAscending — the earlier code sorted "Priority" by SortOrder descending, so such a store's order flips after upgrade. Score (or any other value): engine order kept. Storefront vc-frontend useProducts calls termFacetToCommonFacet(f, false), so the catalog page shows the server order without client re-sort. Docs (Platform user guide, Configure facets) still list a single "Priority" option.

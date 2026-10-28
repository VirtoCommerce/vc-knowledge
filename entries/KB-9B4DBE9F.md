---
id: KB-9B4DBE9F
subject: POST /api/pages/search filters by objectIds (IdsFilter), storeId, permalink, status, visibility; keyword searches the Content field
plane: experiential
question: Which filter fields does POST /api/pages/search accept to narrow the result to specific page documents?
questions:
  - text: How do I fetch one indexed page document by id from /api/pages/search?
  - text: Does /api/pages/search support paging with skip and take?
status: active
appliesTo:
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/pages/search
evidence:
  - method: source
    deployment: vc-module-pages@dev
    at: 2026-10-28T20:55:19.049Z
    by: session:9818a58d
    who: kutasinaelena
---
PageSearchRequestBuilder (vc-module-pages dev) maps PageDocumentSearchCriteria to a search request: objectIds becomes an IdsFilter; storeId, permalink and status become term filters; visibility defaults to Public, which adds a Visibility=Public term filter; translationGroupIds, languageCode (the requested culture plus the store's default language plus Any), certainDate (StartDate/EndDate range), userGroups and organizationId add more filters. keyword is parsed by the search phrase parser and searched in the Content field. skip and take are passed through unchanged, so an unfiltered search with take N returns only a window of N documents.

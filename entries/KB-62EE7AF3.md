---
id: KB-62EE7AF3
subject: some platform routes are hidden from the API document this base projects from
plane: experiential
question: why does an endpoint that plainly exists resolve to nothing in the derived plane
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: GET /api/platform/profiles/currentuser
evidence:
  - method: source
    module: VirtoCommerce.Platform
    version: 3.1007.26
    path: src/VirtoCommerce.Platform.Web/Controllers/Api/ProfilesController.cs
    url: https://raw.githubusercontent.com/VirtoCommerce/vc-platform/3.1007.26/src/VirtoCommerce.Platform.Web/Controllers/Api/ProfilesController.cs
    at: 2026-09-17T18:54:12.977Z
    by: session:773c585d
---

The derived plane is projected from the deployment's OpenAPI document, and a controller carrying [ApiExplorerSettings(IgnoreApi = true)] is absent from it however live it is. GET /api/platform/profiles/currentuser is the worked case: ProfilesController (Route "api/platform/profiles") serves the signed-in operator's UserProfile settings -- VirtoCommerce.Platform.UI.TimeZone, UI.Language, UI.RegionalFormat -- and is marked IgnoreApi, so nothing under /api/platform/profiles is in the document and no extraction of any freshness will ever hold it. api/platform/common and api/platform/localization are marked the same way. So an anchor in a fully projected namespace that resolves to nothing has THREE explanations, not two: misspelled, never there, or hidden -- and kb validate says which to check before correcting.

---
id: KB-BB6B9B7E
subject: GET /api/marketing/promotions (no id) returns HTTP 500 with a model-binding error naming ImportNotification instead of 404/405
plane: experiential
question: What does GET /api/marketing/promotions return — is there a GET list endpoint for promotions?
status: active
appliesTo:
  - axis: module
    value: marketing
  - axis: surface
    value: rest-api
anchors:
  - coordinate: GET /api/marketing/promotions
  - coordinate: POST /api/marketing/promotions/search
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-12T14:34:39.629Z
    by: session:624d35a1
    who: kutasinaelena
---
With an admin bearer token, GET /api/marketing/promotions and GET /api/marketing/promotions?take=10 both returned HTTP 500 with body {message:"Could not create an instance of type 'VirtoCommerce.MarketingModule.Core.Model.PushNotifications.ImportNotification'. Model bound complex types must not be abstract...", stackTrace:null}. There is no GET list endpoint; listing is POST /api/marketing/promotions/search (200, totalCount + results with id/name/isActive). The unsupported GET surfaces a model-binding exception and an internal type name rather than 404/405. Marketing module 3.1009.0.

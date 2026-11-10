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
    build: b6daa5d65748d7e7
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    build: 1daf7ccef257ac50
    conditions: platform=3.1085.0; module:VirtoCommerce.Marketing=3.1009.0
    at: 2026-10-27T21:39:44.949Z
    by: session:e75fe8a4
    bodyHash: 9f96b9f3f1e0
    who: Aleksandra-Mitricheva
    note: "Reproduced 2026-10-27 on vcptcore-qa with admin token: GET /api/marketing/promotions and ?take=10 → 500, same ImportNotification message."
  - method: observation
    deployment: vcptcore_regression
    build: 9648699210e92bd7
    conditions: platform=3.1076.13; module:VirtoCommerce.Marketing=3.1009.1
    at: 2026-11-10T05:05:44.561Z
    by: session:3f3bb000
    bodyHash: 9f96b9f3f1e0
    who: kutasinaelena
    note: "Still reproduces after upgrade to Platform 3.1076.13 / Marketing 3.1009.1: GET /api/marketing/promotions and ?take=10 -> 500, same ImportNotification message."
---
With an admin bearer token, GET /api/marketing/promotions and GET /api/marketing/promotions?take=10 both returned HTTP 500 with body {message:"Could not create an instance of type 'VirtoCommerce.MarketingModule.Core.Model.PushNotifications.ImportNotification'. Model bound complex types must not be abstract...", stackTrace:null}. There is no GET list endpoint; listing is POST /api/marketing/promotions/search (200, totalCount + results with id/name/isActive). The unsupported GET surfaces a model-binding exception and an internal type name rather than 404/405. Marketing module 3.1009.0.

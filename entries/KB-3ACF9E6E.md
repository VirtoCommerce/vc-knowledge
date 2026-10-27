---
id: KB-3ACF9E6E
subject: On vcst_qa the storefront /account/purchase-requests page is a 404 because the AI document-processing module is not installed, not because a setting is off
plane: experiential
question: /account/purchase-requests returns 404 on the storefront — is AIDocumentProcessing.EnablePurchaseRequests off, or is the module missing?
questions:
  - text: Why is the purchase requests page not found on vcst?
  - text: Is AIDocumentProcessing.EnablePurchaseRequests a registered platform setting on vcst?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/purchase-requests
  - coordinate: GET /api/platform/modules
evidence:
  - method: observation
    deployment: vcst_qa
    build: 24b60c4b061158d9
    conditions: theme=2.61.0-pr-2577-4c17; module:VirtoCommerce.Quote=3.1005.0; store=B2B-store; role=B2B buyer
    at: 2026-10-27T19:56:18.427Z
    by: session:e0e42495
    who: Lenajava1
---
Signed in as a B2B buyer on vcst_qa (theme 2.61.0-pr-2577-4c17), /account/purchase-requests renders the storefront 404 page (h1 "404"). A read-only GET of the platform setting AIDocumentProcessing.EnablePurchaseRequests returns HTTP 500 "Setting with name AIDocumentProcessing.EnablePurchaseRequests is not registered"; the B2B-store settings carry no AIDocumentProcessing or purchase-request setting, and GET /api/platform/modules lists no AI document-processing module (the only quote-related module is VirtoCommerce.Quote 3.1005.0). So on this stand the page is unreachable because the module that serves it is not installed; a test of purchase requests needs a stand with that module installed and the setting enabled.

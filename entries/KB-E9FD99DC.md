---
id: KB-E9FD99DC
subject: "On vcst_qa 2026-10-13 the product-configuration upload scope is not configured: uploads return INVALID_SCOPE under HTTP 200 and the PDP shows no error"
plane: experiential
question: Can a buyer upload a configurable-product File section file to POST /api/files/product-configuration on vcst_qa right now?
status: active
appliesTo:
  - axis: feature
    value: configurable-products
  - axis: module
    value: fileexperienceapi
  - axis: surface
    value: rest-api
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: POST /api/files/product-configuration
  - coordinate: Query.fileUploadOptions
evidence:
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1078.0; module:VirtoCommerce.FileExperienceApi=3.1008.0; theme=2.60.0-pr-2485-f6cd22b2; role=customer
    at: 2026-10-13T09:24:35.403Z
    by: session:a3a809eb
    who: Aleksandra-Mitricheva
---
2026-10-13 ~09:21Z, storefront theme 2.60.0-pr-2485 proxied to vcst_qa, buyer on B2B-store PDP of a configurable product with an optional File section: selecting 3 files (pdf 611 B, two txt) fired 3x POST /api/files/product-configuration, each HTTP 200 with body [{succeeded:false, errorCode:INVALID_SCOPE, errorMessage:"Unknown scope 'product-configuration'"}]. Query fileUploadOptions(scope:"product-configuration") returns null, while fileUploadOptions(scope:"return-attachments") returns 5 MB / .jpg .jpeg .png .webp .pdf. The PDP rendered no file rows and no error message; the section's None radio was merely unchecked and Add to cart became enabled. On 2026-10-08 the same scope accepted uploads on this stand (KB-45F13A37), so the FileUpload configuration was changed since — consistent with KB-E91989D9 (scopes come only from the FileUpload configuration section). Every file-attachment flow (cart/checkout/order/quote files) is unreachable on the stand until the scope is configured again.

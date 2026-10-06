---
id: KB-35F25BEE
subject: Purchase requests turn uploaded order documents into a quote and have no approve or reject step
plane: experiential
question: What does the purchase requests feature do, and does it have approve or reject mutations?
questions:
  - text: Can I upload a photo of a handwritten order and get it turned into a quote?
  - text: Is the purchase requests page an approval workflow for buyers, or something else?
  - text: Which mutation sequence turns uploaded documents into a quote with a quoteId?
  - text: Where does buyer approval live if purchase requests have no approve or reject mutation?
concepts:
  - id: purchase-request
  - id: quote
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: /account/purchase-requests
  - coordinate: Mutation.createPurchaseRequestFromDocuments
evidence:
  - method: observation
    deployment: virtostart
    at: 2026-09-25T11:18:17.818Z
    by: session:memimpor
    who: Lenajava1
    splitFrom: KB-432D02EF
---
The storefront's /account/purchase-requests is served by the AI document processing module: a buyer uploads order-like documents (PDF, image, even handwritten) and the pipeline turns them into a quote. The xAPI flow is createPurchaseRequestFromDocuments, then extractPurchaseRequestSourcesData (OCR and line extraction), then postProcessPurchaseRequestSources (catalog matching, which sets quoteId), then normal quote acceptance; there is no approve or reject mutation - buyer approval belongs to quote requests at /account/quotes. The module is flagged experimental.

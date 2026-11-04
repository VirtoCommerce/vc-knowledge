---
id: KB-BCA1C475
subject: POST /api/customerReviews returns HTTP 500 instead of a validation error when a review has no images field, or when the same user already reviewed the same product (the duplicate 500 leaks the SQL unique-index message)
plane: experiential
question: What does POST /api/customerReviews return for a review without images, or for a second review by the same user on the same product?
questions:
  - text: Why does the admin REST customer review create return Object reference not set to an instance of an object?
  - text: Does the REST customer reviews API reject a duplicate review per user and product with a validation error?
  - text: Is the images array required on POST /api/customerReviews?
  - text: Does creating a duplicate customer review through REST leak IX_CustomerReview_StoreId_EntityId_EntityType_UserId?
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/customerReviews
  - coordinate: Mutation.createReview
evidence:
  - method: observation
    deployment: vcst_qa
    build: 4fafc097f6a88f6c
    conditions: role=platform administrator (REST); store=B2B-store
    defect: suspected
    at: 2026-11-04T08:27:14.821Z
    by: session:7c4c844c
    who: Lenajava1
---
2026-11-04, admin bearer token, product in the B2B-store catalog, an AGENT-TEST buyer as userId. (1) POST /api/customerReviews with a one-element array whose review has no images property -> HTTP 500 {"message":"Object reference not set to an instance of an object.","stackTrace":null}; nothing is saved. Source (vc-module-customer-review dev 7485603): CustomerReviewsModuleController.Update dereferences x.Images.Count at line 151 and CustomerReview.Images has no default. (2) The same review with images: [] -> 204 and the review is saved (control). (3) Posting that same body again (same store, product, entityType, userId) -> HTTP 500 whose message is the raw SQL Server text "Cannot insert duplicate key row in object 'dbo.CustomerReview' with unique index 'IX_CustomerReview_StoreId_EntityId_EntityType_UserId'. The duplicate key value is (...)" including the store id, product id and user id; stackTrace is null. The REST path has no duplicate check before the save; the unique index is declared in CustomerReviewsDbContext.cs:30-32. The xAPI createReview mutation for the same user and product returns HTTP 200 with validationErrors DUPLICATE_REVIEW instead. Expected per BL-AUTH-017: a body the endpoint cannot accept yields 4xx, never an unhandled 500.

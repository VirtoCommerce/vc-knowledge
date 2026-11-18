---
id: KB-B833CFF8
subject: One order in a currency the platform no longer configures makes every CustomerOrder index-changes run fail with "Sequence contains no matching element", so CustomerOrder's IndexationDate never advances while that order is in the window
plane: experiential
question: Why does the scheduled index-changes job keep failing for CustomerOrder with "Sequence contains no matching element" and never advance its IndexationDate?
questions:
  - text: Why is the CustomerOrder IndexationDate stuck while other document types advance?
  - text: Can deleting a currency break order search indexing?
  - text: What does IndexProgressHandler 'Sequence contains no matching element' mean during scheduled indexing?
  - text: Why are recently changed orders missing from the order search index?
status: active
appliesTo:
  - axis: entity
    value: customer-order
  - axis: module
    value: search
  - axis: surface
    value: rest
anchors:
  - coordinate: GET /api/platform/settings/VirtoCommerce.Search.IndexingJobs.IndexationDate.CustomerOrder
  - coordinate: DELETE /api/currencies
  - coordinate: POST /api/search/indexes/index
evidence:
  - method: observation
    deployment: vcst_qa
    build: 2ff67d68f7987d9e
    conditions: platform=3.1099.0; module:VirtoCommerce.Orders=3.1021.0; module:VirtoCommerce.BackgroundJobs=3.1055.0; setting:BackgroundJobs:Provider=RabbitMQ; setting:VirtoCommerce.Search.IndexingJobs.CronExpression=0/59 * * * *
    defect: suspected
    at: 2026-11-18T15:00:31.876Z
    by: session:dde571fa
    who: Lenajava1
---
2026-11-18, admin token. On vcst_qa the recurring IndexChangesJob (cron 0/59) logged one "System.InvalidOperationException: Sequence contains no matching element" (source VirtoCommerce.SearchModule.Data.BackgroundJobs.IndexProgressHandler) per run at 13:59 and 14:00; afterwards VirtoCommerce.Search.IndexingJobs.IndexationDate.{Product,Category,Member,ContentFile,PickupLocation,Pages} were 14:00Z but IndexationDate.CustomerOrder stayed 2026-11-16T12:59:02Z (the job saves a type's date only after that type succeeds, so the failing window is re-run every time). Of the 9 029 orders modified since 2026-11-16 12:59, exactly one used a currency not in GET /api/currencies: test order AGENT-TEST-BD-UNCFG-X1 in XTS, modified 2026-11-16T13:06Z (the currency had been created, used and deleted). Test: POST /api/currencies XTS at 14:55:01Z; the 15:00 run succeeded and IndexationDate.CustomerOrder moved to 2026-11-18T15:00:01Z with no new error; DELETE /api/currencies?codes=XTS at 15:00:05Z. So the order in the deleted currency was what blocked CustomerOrder change indexing. Once the date passes the order's modifiedDate the job no longer touches it; modifying the order again would re-block it. The docs (VirtoOZ) do not cover deleting a currency that orders still use.

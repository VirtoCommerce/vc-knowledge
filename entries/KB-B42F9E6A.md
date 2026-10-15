---
id: KB-B42F9E6A
subject: "Generic export (Pricing > Price lists > Export, JSON provider) never completes with BackgroundJobs 3.1052.1: ExportJob fails every retry deserializing the abstract ExportDataQuery"
plane: experiential
question: Why does a price-list export from the Admin stay on 'Starting export task...' forever?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.backgroundjobs 3.1052.1
  - axis: module
    value: virtocommerce.export 3.1005.0
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: /workspace/pricing
  - coordinate: /hangfire/jobs/details
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-15T00:05:59.124Z
    by: session:624d35a1
    who: kutasinaelena
---
On Platform 3.1076.3 with VirtoCommerce.BackgroundJobs 3.1052.1 and VirtoCommerce.Export 3.1005.0, Pricing > Price lists > select one list > Export > JSON export provider > Start enqueues a Hangfire ExportJob (HangfireJobExecutor) whose payload carries Request.DataQuery as the abstract VirtoCommerce.ExportModule.Core.Model.ExportDataQuery; JsonJobPayloadSerializer.Deserialize throws Newtonsoft JsonSerializationException 'Could not create an instance of type ...ExportDataQuery. Type is an interface or abstract class' on every attempt (retry 1..3), the push notification 'ExportablePricelist export' never finishes and the Admin Export progress blade stays at 'Starting export task...'. Same failure family as the PaymentMethod payload entries. Stable 15 (no BackgroundJobs module, Export 3.1003.0) does not use this executor.

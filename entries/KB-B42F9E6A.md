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
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-15T09:30:15.056Z
    by: session:624d35a1
    who: kutasinaelena
    note: "Re-observed 2026-10-15 via REST: POST /api/export/run (ExportablePricelist, JsonExportProvider) -> notification stays 'Starting export task...' 60s+; Hangfire ExportJob fails 3/3 with JsonSerializationException at Path 'Request.DataQuery.Currencies'. Same request on vcptcore_stable (Export 3.1003.0, no BackgroundJobs module) finishes in <1s with a download URL."
  - method: observation
    deployment: vcptcore_regression
    build: 5609fc0c36e7302c
    conditions: platform=3.1076.7; module:VirtoCommerce.BackgroundJobs=3.1052.1; module:VirtoCommerce.Export=3.1005.0; module:VirtoCommerce.Pricing=3.1008.1; module:VirtoCommerce.Notifications=3.1018.0; role=Administrator
    at: 2026-10-21T15:18:35.293Z
    by: session:624d35a1
    bodyHash: 89d65046e634
    defect: suspected
    who: kutasinaelena
    note: "Re-observed 2026-10-21 on Platform 3.1076.7 through REST and through the Admin UI. REST POST /api/export/run (ExportablePricelist, JsonExportProvider) created job 445243, which ran 4 times (the first run plus retries 1-3 of 3). Every run failed with the same JsonSerializationException at Path 'Request.DataQuery.Currencies', in JsonJobPayloadSerializer.Deserialize (called from DefaultJobDispatcher.Dispatch and HangfireJobExecutor.Execute). The push notification never finished and carried no error. The same export from the Admin UI (job 445268) stayed on 'Starting export task...'. Control: on vcptcore_stable (Platform 3.1039.15, Export 3.1003.0, no BackgroundJobs module) the same request finished in about 0.9 s and returned a download URL. Source shows background-jobs 3.1052.1 JobJsonSettings has no converters. The upstream release 3.1052.2 (PR #8) adds the host MVC converters, but that fix was not verified live."
---
On Platform 3.1076.3 with VirtoCommerce.BackgroundJobs 3.1052.1 and VirtoCommerce.Export 3.1005.0, Pricing > Price lists > select one list > Export > JSON export provider > Start enqueues a Hangfire ExportJob (HangfireJobExecutor) whose payload carries Request.DataQuery as the abstract VirtoCommerce.ExportModule.Core.Model.ExportDataQuery; JsonJobPayloadSerializer.Deserialize throws Newtonsoft JsonSerializationException 'Could not create an instance of type ...ExportDataQuery. Type is an interface or abstract class' on every attempt (retry 1..3), the push notification 'ExportablePricelist export' never finishes and the Admin Export progress blade stays at 'Starting export task...'. Same failure family as the PaymentMethod payload entries. Stable 15 (no BackgroundJobs module, Export 3.1003.0) does not use this executor.

---
id: KB-5F7C8FC4
subject: an empty Tax providers widget is not evidence that a store has no tax provider
plane: experiential
question: how do I find out whether a store has a tax provider, and why does the Admin widget show nothing
status: active
refutableBy: observation
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: POST /api/taxes/search
arrivesAt:
  - coordinate: GET /api/stores/{id}
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-16T12:44:58+04:00
    by: session:09e39416
    from: C:/_VIRTO/_comparison-logs/arm-C/report.md
  - method: observation
    deployment: vcptcore_stable
    platformVersion: 3.1007.27
    at: 2026-09-16T12:44:58+04:00
    by: session:09e39416
    from: C:/_VIRTO/_comparison-logs/round2/arm-A/report.md
  - method: observation
    deployment: vcptcore_stable
    platformVersion: 3.1007.27
    at: 2026-09-16T12:44:58+04:00
    by: session:09e39416
    from: C:/_VIRTO/_comparison-logs/round2/arm-B/report.md
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-16T17:25:57.055Z
    by: session:26f59771
    note: "The widget rendered zero rows again for B2B-store, and the cause is visible in the console rather than only in the API: [ngRepeat:dupes] \"Repeater: data in blade.currentEntities track by data.typeName, Duplicate key: FixedRateTaxProvider\". The thrown error also leaks the row it choked on - storeId B2B-store, code FixedRate, isActive false, priority 0, setting VirtoCommerce.Core.FixedTaxRateProvider.Rate with value null - which is the same provider the search API reports. So the store DOES have a provider, the grid keys on typeName, and a store whose provider list holds two rows of one type renders nothing at all."
---

Read POST /api/taxes/search, not the Admin widget. Round two's arm A read the Store blade's Tax providers widget as a header row with NO rows and concluded zero providers were registered; round two's arm B found the grid broken by an unrelated ngRepeat defect and had to read one provider individually because the search API is POST-only; round one's arm C called the same search endpoint and got exactly ONE provider for B2B-store - FixedRate / FixedRateTaxProvider - with isActive false and its own rate setting unset. So the widget rendering nothing is a UI defect, not a statement about configuration, and two arms drew opposite conclusions from the same store on the same day because of it. The same call gives you a CONTROL, which is the part worth remembering: other stores on this deployment do have an active provider - Electronics with FixedRate active at rate 10, TestStorePostman active at 15 - so a zero tax on B2B-store is specific to that store and can be checked against a store where tax demonstrably runs. One more trap on the order blade: its widget reads AvaTax is not enabled for this order's store, which is about the AvaTax module specifically and is not a general statement that no tax provider exists.

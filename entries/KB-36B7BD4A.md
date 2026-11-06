---
id: KB-36B7BD4A
subject: Loyalty missions with EndDate before StartDate are accepted and persisted, via REST and via the Admin mission blade Save
plane: experiential
question: Does POST /api/loyalty-missions reject an EndDate earlier than the StartDate?
questions:
  - text: Is the loyalty mission date window validated?
  - text: Can the Admin loyalty mission blade save a mission whose end date precedes its start date?
  - text: Is StartDate equal to EndDate accepted on a loyalty mission?
concepts:
  - id: loyalty-mission
status: active
appliesTo:
  - axis: surface
    value: admin-ui
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/loyalty-missions
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 99880d06bbb9b4cf
    conditions: platform=3.1076.8; module:VirtoCommerce.Loyalty=3.1009.0; role=Administrator
    defect: suspected
    at: 2026-11-06T10:48:56.315Z
    by: session:6eca33a7
    who: Aleksandra-Mitricheva
    note: "distinct: KB-D64B1613 is about input the validator rejects; this is an input it does not check at all (date order), so the request is accepted with 200 and persisted."
---
On Loyalty 3.1009.0, POST /api/loyalty-missions with startDate 2027-01-01 and endDate 2026-01-01 returned 200 and the read-back kept the inverted window. In the Admin SPA mission blade (#/workspace/loyalty-missions, Add) with Start date Dec 1, 2026 and End date Nov 1, 2026, Save stayed enabled; the blade sent POST /api/loyalty-missions, got 200, and saved the mission as Draft with no message. startDate equal to endDate is also accepted (200). The 3.1009.0 LoyaltyMissionValidator has no date-order rule and the blade has no client-side check.

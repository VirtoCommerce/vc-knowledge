---
id: KB-813DD6AC
subject: loyaltyMissionProgress daysRemaining reads ceil of days to EndDate on the server clock
plane: experiential
question: How is loyaltyMissionProgress daysRemaining computed and which value does a mission N days out read?
questions:
  - text: How many days left does a loyalty challenge show when it ends in 20 minutes?
  - text: Query.loyaltyMissionProgress daysRemaining ceil EndDate UtcNow
  - text: Is loyalty mission daysRemaining stored or computed per request?
  - text: What does daysRemaining return for a mission without an EndDate?
  - text: How should EndDate be set so a mission reads exactly N days remaining?
concepts:
  - id: loyalty-mission
  - id: timestamp
  - id: graphql-api
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.loyaltyMissionProgress
  - coordinate: LoyaltyUserMission.daysRemaining
evidence:
  - method: observation
    deployment: vcst
    at: 2026-10-01T11:57:11.873Z
    by: session:2875d2a6
    who: Lenajava1
---
daysRemaining is computed per request, not stored: ceil((EndDate - UtcNow) in days), floored at 0, null when the mission has no EndDate. Missions published with EndDate = server time + N days - 12 h read exactly N (observed 1, 15, 16, 30, 31 and 60) and keep that value for 12 h. A mission whose EndDate is 20 minutes away reads 1, not 0. Observed on Loyalty 3.1009.0, vcst, 2026-10-01.

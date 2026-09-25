---
id: KB-6A20B0AF
subject: omitting an optional xAPI context argument returns 200 with wrong or empty data, never an error
plane: experiential
question: Why does an xAPI query such as loyaltyMissionProgress return null fields when cultureName, storeId, userId or organizationId is omitted?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.loyaltyMissionProgress
  - coordinate: LoyaltyMissionProgressType.description
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:19:01.767Z
    by: session:memimpor
    who: Lenajava1
---
Most xAPI queries and mutations take the ambient context - cultureName, storeId, userId, organizationId, plus currencyCode where money is involved (inside command for mutations) - and on most of them these are optional. Omitting one is not an error: the server substitutes a default and answers 200 with data that is wrong, empty or null. On the live schema about two-thirds of queries accept at least one of the four optionally, and cultureName is accepted by 59 queries but required by only 3. Example: loyaltyMissionProgress returns description null for every item without cultureName while name still resolves. A difference between two callers is a context difference until shown otherwise.

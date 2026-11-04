---
id: KB-8FC13CB4
subject: Concurrent updateMemberAddresses calls with the same new address can store 2 to 5 identical copies; the duplicate skip only holds for sequential calls
plane: experiential
question: Mutation.updateMemberAddresses — does the server's duplicate-address skip hold when the same new address is saved by several concurrent calls?
questions:
  - text: Can parallel address saves create duplicate addresses?
  - text: Is the updateMemberAddresses duplicate check race-safe?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.updateMemberAddresses
  - coordinate: Query.currentCustomerAddresses
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: module:VirtoCommerce.ProfileExperienceApiModule=3.1023.0; module:VirtoCommerce.Xapi=3.1029.0; account=personal
    defect: suspected
    at: 2026-11-04T12:31:02.111Z
    by: session:3f1092dc
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    at: 2026-11-04T12:37:49.653Z
    by: session:3f1092dc
    bodyHash: c8cdb37435fa
    defect: suspected
    who: Lenajava1
    note: "10 parallel calls (fresh connection each) on a book of 8, three runs: book 10, 9 and 18 (expected 9), identical rows; 5, 6 and 0 calls returned HTTP 200 with errors[] \"Error trying to resolve field 'updateMemberAddresses'.\" Existing-address parallel saves changed nothing."
---
As a personal storefront user with an empty address book, 5 sequential updateMemberAddresses calls with one identical new address left 1 copy (the duplicate skip works). 5 concurrent calls with that same address (a fresh connection each, no retry) left 2 to 5 identical copies in 7 of 12 rounds. In one round all 5 calls returned HTTP 200 with empty errors[] and 5 copies were stored. Some calls were refused with DB_UPDATE_CONCURRENCY (still HTTP 200), but not all, so that guard does not prevent the duplicates. Every response carried the same instance marker, so this was seen on a single instance; a cross-instance race was not tested.

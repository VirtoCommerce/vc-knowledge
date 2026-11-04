---
id: KB-6EBEE61A
subject: Parallel updateMemberAddresses of one new address on a fresh connection each creates duplicate rows and some calls fail with 'Error trying to resolve field'
plane: experiential
question: What happens when the same new address is saved by 10 parallel updateMemberAddresses calls?
status: active
appliesTo:
  - axis: role
    value: personal customer
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.updateMemberAddresses
  - coordinate: Query.currentCustomerAddresses
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: store=B2B-store
    defect: suspected
    at: 2026-11-04T12:37:37.940Z
    by: session:3f1092dc
    who: Lenajava1
---
On a personal contact with 8 addresses, 10 parallel Mutation.updateMemberAddresses calls (each on its own connection) with the same NEW address (identical in every field) left the book at 10, 9 and 18 across three runs (expected 9: one new row), with identical rows (all 10 copies in the 18 case) and 5, 6 and 0 of the calls returning HTTP 200 with errors[] message "Error trying to resolve field 'updateMemberAddresses'." (no code, no field). The same 10 parallel calls with an address that already existed changed nothing (book stayed 8, no errors). Sequential repeats of an identical address are skipped, so the duplicate check works one call at a time but not under concurrency.

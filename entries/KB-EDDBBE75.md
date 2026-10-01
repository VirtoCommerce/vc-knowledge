---
id: KB-EDDBBE75
subject: a loyalty balance has no write API and cannot be reset
plane: experiential
question: Can a loyalty member's points balance be set or reset to zero through /api/loyalty-program-operation-log?
questions:
  - text: Can support zero out my rewards points balance if it is wrong?
  - text: Is there an endpoint to set, correct or delete a member's loyalty points balance?
  - text: What happens to a customer's loyalty balance if their user account is deleted and recreated?
  - text: Why is the balance change an unreliable check when an order both earns and redeems points?
  - text: Which operation log rows should a test assert on to verify earn and redeem?
concepts:
  - id: loyalty
  - id: loyalty-history
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: /api/loyalty-program-operation-log/balance/{userId}
  - coordinate: /api/loyalty-program-operation-log/search
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:14:43.831Z
    by: session:memimpor
    who: Lenajava1
  - method: observation
    deployment: vcst
    at: 2026-10-01T08:22:39.238Z
    by: session:4b2eac5d
    who: Lenajava1
    note: "Observed the exact scenario: a customer account was recreated under the same email; the storefront balance (xAPI loyaltyBalance and points history) reads 0 with 'No records found', while the old security-account id still returns its full balance and 223 ledger rows from GET balance/user/{id}. The old account no longer exists in the platform. No purge path seen."
---
The loyalty module exposes the points operation log read-only - a balance read per user and an operation-log search - and offers no endpoint to write, delete or set a balance. Balances change only through the earn and redeem handling on orders. The balance is keyed on the security-account user id, so if that account is deleted and recreated the new account starts at zero and the old account's operation log is orphaned with no purge path. On a mixed cart that pays cash and redeems points, the earn and the redemption can cancel out, so the resulting balance delta is not a reliable signal; the Redeemed and Earned operation rows are.

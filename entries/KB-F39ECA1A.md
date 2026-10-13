---
id: KB-F39ECA1A
subject: Locking the platform account does not lock the user's organization membership; the two lock flags are independent
plane: experiential
question: Does POST /api/platform/security/users/{id}/lock (Admin 'Lock account') also set the organization membership isLocked flag?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/platform/security/users/{id}/lock
  - coordinate: GET /api/platform/security/users/{id}/locked
  - coordinate: POST /api/customer/organization-memberships/search
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T16:42:56.171Z
    by: session:624d35a1
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T17:55:52.619Z
    by: session:624d35a1
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T18:52:59.312Z
    by: session:624d35a1
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T19:46:48.208Z
    by: session:624d35a1
    who: kutasinaelena
---
On Stable 16 (Customer module 3.1029.0) a Customer account with one organization membership was locked from Admin Security > Users > Lock account (no confirm dialog; blade Locked state turns 'Locked', lockoutEnd 9999-12-31). While locked, the Contacts > Organization memberships > membership blade showed Locked state 'Unlocked' and POST /api/customer/organization-memberships/search by userId returned isLocked:false, isCurrentlyLocked:false, lockoutEnd:null. The platform account lock and the per-organization membership lock are separate mechanisms. Also: the security user DTO (GET /api/platform/security/users/{id}) carries no isLockedOut field at all (lockoutEnd and lockoutEnabled only, also on Stable 15); GET /api/platform/security/users/{id}/locked returns {locked:bool}, which is what the Admin blade calls.

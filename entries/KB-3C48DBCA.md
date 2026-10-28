---
id: KB-3C48DBCA
subject: Sign-in log search with signInTypes ["Passkey"] returns only Passkey rows with failure reasons InvalidPasskey / LockedOut / MissingParameter; Manager accounts without the sign-in-log permission get 403
plane: experiential
question: How does POST /api/platform/security/sign-in-log/search filter passkey sign-ins, and who may call it?
questions:
  - text: Which failureReason values does the sign-in log record for passkey sign-ins?
  - text: Can a Store access manager read the sign-in log?
  - text: Does the sign-in log filter by signInTypes Passkey?
status: active
appliesTo:
  - axis: feature
    value: passkey-sign-in
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/platform/security/sign-in-log/search
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: a5a20e99d84d8f1b
    conditions: platform=3.1091.0-pr-3144-e637; module:VirtoCommerce.Passkeys=3.1000.0-pr-1-a25f
    at: 2026-10-28T14:01:52.406Z
    by: session:56fb99bb
    who: Lenajava1
---
Admin token: POST /api/platform/security/sign-in-log/search {signInTypes:["Passkey"], take:200} -> 200, totalCount 73 of 3388 rows, every row signInType 'Passkey'; succeeded true 28, false with failureReason InvalidPasskey 34, LockedOut 6, MissingParameter 5. Criteria fields (swagger UserSignInLogSearchCriteria): userId, userName, succeeded, awaitingSecondFactor, signInTypes[], failureReasons[], ipAddress, storeId, withoutStore, organizationId, startDate, endDate. LockedOut does not distinguish a temporary lock from an admin lock (both seen as LockedOut, with user and store). Three non-admin Managers (roles 'Store access' with store:access/store:read, and a role holding only platform:security:loginOnBehalf) get 403 on the same endpoint, so the Admin sign-in log blade cannot be exercised with them.

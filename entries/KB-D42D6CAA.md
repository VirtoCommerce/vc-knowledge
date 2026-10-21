---
id: KB-D42D6CAA
subject: "Admin Sign-in log vocabularies disagree: with VirtoCommerce.MFA installed, the Type filter offers \"Sign-in code\" (no MFA, no OTP option) and selecting it lists rows whose Type column reads MFA; the Outcome filter gains \"Awaiting second factor\""
plane: experiential
question: What Type and Outcome values does the Admin Sign-in log expose once MFA and OTP are installed, and are the filter labels the same as the grid values?
status: active
appliesTo:
  - axis: feature
    value: sign-in-log
  - axis: surface
    value: admin-spa
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/platform/security/sign-in-log/search
evidence:
  - method: observation
    deployment: vcst_qa
    build: c65e39a5daec993f
    conditions: platform=3.1088.0-pr-3133-23a5-vcst-5904; module:VirtoCommerce.MFA=3.1000.0-pr-1-41b3; module:VirtoCommerce.OTP=3.1001.0-pr-3-d978
    at: 2026-10-21T16:33:23.304Z
    by: session:5fa7c31a
    who: Lenajava1
---
On vcst_qa (platform 3.1088.0-pr-3133, MFA 3.1000.0-pr-1-41b3, OTP 3.1001.0-pr-3-d978), Security > Sign-in log > View all records > filters: Outcome options All, Success, Failed, Awaiting second factor; Type options All, Password, External provider, On behalf, On behalf revert, Sign out, Sign-in code; Store options list every store that has a record. Choosing Type = "Sign-in code" listed records whose Type column reads "MFA" (Failed InvalidCode, InvalidTwoFactorToken; Success), so the filter's label and the grid's raw value differ, and no filter option corresponds to the OTP type that OTP sign-ins carry in the grid and in the API (signInType OTP). POST /api/platform/security/sign-in-log/search over the last 1000 records showed signInType values Password, MFA (provider Email), OTP, Impersonation, ImpersonationRevert, Logout, and Password rows with failureReason RequiresTwoFactor, CodeAttemptsExceeded, LockoutDisabled, ProviderNotAvailable; the record fields now include provider, sessionId, organizationId, organizationName, memberId and clientId. On the stand without MFA the API also showed signInType punchout (lower case) from the Punchout module.

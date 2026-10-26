---
id: KB-69C4BE5F
subject: Sign-in log dashboard Sign-ins tile counts every MFA step record (password step Awaiting second factor + each code attempt), not sign-ins; Failed attempts excludes awaiting rows; the attempts chart excludes them
plane: experiential
question: How does the Backoffice Sign-in log dashboard count MFA sign-ins — is a password step awaiting the second factor counted as a sign-in or a failed attempt?
questions:
  - text: Why is the Sign-ins tile higher than the number of people who signed in with MFA?
  - text: Does an abandoned MFA sign-in show as a failed attempt on the sign-in log dashboard?
  - text: What does awaitingSecondFactor:false do on POST /api/platform/security/sign-in-log/stats?
status: active
appliesTo:
  - axis: feature
    value: sign-in-log
  - axis: surface
    value: admin-spa
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/platform/security/sign-in-log/stats
  - coordinate: POST /api/platform/security/sign-in-log/search
evidence:
  - method: observation
    deployment: vcst_qa
    build: 8d4bda9222c60036
    conditions: platform=3.1090.0-pr-3133; setting:MultiFactorAuthentication.Enabled=false; setting:MultiFactorAuthentication.StoreMode=Enabled (disposable store)
    at: 2026-10-26T09:58:56.149Z
    by: session:7ed484a4
    who: kutasinaelena
---
One customer on an MFA-Enabled store made, via /connect/token: 1 complete MFA sign-in (password grant -> two_factor_required -> mfa grant ok), 2 abandoned (password grant only), 1 wrong code then right code on one ticket. The log holds 7 records for the user: 4 Password rows Outcome "Awaiting second factor" (succeeded=false, failureReason RequiresTwoFactor), 2 MFA Success, 1 MFA Failed InvalidCode. POST /api/platform/security/sign-in-log/stats filtered by userName (or keyword) over the window: totalCount 7, failedCount 1, distinctUserCount 1, impersonationCount 0, timeline 2 ok + 1 failed (awaiting rows absent from the chart), signInsByStore 7. With awaitingSecondFactor:false: totalCount 3, failedCount 1. With awaitingSecondFactor:true: totalCount 4, failedCount 0. The dashboard itself sends only startDate (no awaitingSecondFactor), so its Sign-ins tile (tooltip: "All sign-in attempts in the period, successful and failed") includes the awaiting rows, and its Sign-ins total exceeds the Success+Failed sum of the chart. The dashboard has no user filter (period buttons only); per-user numbers are only reachable via the grid keyword search or the stats API. storeIds and userIds in the stats body did not narrow the result on this build.

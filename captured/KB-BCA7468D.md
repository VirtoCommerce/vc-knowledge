---
id: KB-BCA7468D
subject: two endpoints disagree about whether a password hash is a secret
plane: experiential
question: can a security account's password hash be read back over the REST API
status: active
refutableBy: observation
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: GET /api/members/{id}
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-15T17:52:40.110Z
---

It can, over ONE of the two endpoints that return it. GET /api/platform/security/users/{userName} strips it: ReduceUserDetails blanks passwordHash unless ReturnPasswordHash is set, so anything reading accounts through the security endpoint sees null and may reasonably conclude the field is unreadable. GET /api/members/{id} does NOT strip it - the member payload carries securityAccounts with passwordHash populated, observed as an 84-character ASP.NET Identity v3 value on an account that had never completed registration. Two consequences. For a tester: do not infer from a null passwordHash on the security endpoint that no password exists, and do not paste a raw members payload into a report, a screenshot or a page snapshot, because the hash travels with it - three independent runs on 2026-09-15 each carried a live hash into their saved artefacts without noticing. For the platform: one object is redacted on one route and not on another, which is a defect worth reporting rather than a convention to rely on. Confirmed on platform 3.1007.27, Customer module 3.1000.7.

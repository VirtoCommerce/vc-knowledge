---
id: KB-AC4B6E46
subject: With Stores.EmailVerificationRequired=false, a self-registered storefront user signs in before confirming the email, and its contact is Approved, not Locked
plane: experiential
question: Can a newly self-registered storefront user sign in before confirming the email when the store's email verification is enabled but not required?
questions:
  - text: What contact status does /sign-up give a new personal user when email verification is not required?
  - text: Is unverified-email sign-in blocked when Stores.EmailVerificationEnabled is true but EmailVerificationRequired is false?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /sign-up
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcst_qa
    build: 3b66ce25e7926ad6
    conditions: platform=3.1097.0; theme=2.62.0-alpha.2647; store=B2B-store; setting:Stores.EmailVerificationEnabled=true; setting:Stores.EmailVerificationRequired=false
    at: 2026-11-11T21:55:35.226Z
    by: session:7e403256
    who: kutasinaelena
---
B2B-store with Stores.EmailVerificationEnabled=true and Stores.EmailVerificationRequired=false (read via GET /api/stores/B2B-store). A personal account registered on /sign-up lands on /successful-registration ('Registration completed ... check your mailbox to verify'). The security user reads back emailConfirmed=false, status Approved; its contact status is Approved (not Locked). Without confirming the email, signing in on /sign-in (password form) succeeds: POST /connect/token 200 and the header shows the user's name. So the verification mail is sent but sign-in is not gated on it.

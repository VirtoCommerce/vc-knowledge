---
id: KB-33B89BDD
subject: OTP request for an unregistered email returns succeeded:false with code user_not_found (DetailedErrors on); a 255-char email is refused 400 by MaxLength(254) while the storefront form lets it through
plane: experiential
question: What does POST /api/otp/request return for an unknown email or an over-long email, and which layer rejects the length?
questions:
  - text: Does the sign-in-by-code request reveal that an email address has no account?
  - text: POST /api/otp/request user_not_found succeeded false unknown email
  - text: What is the maximum email length accepted by the OTP request endpoint?
  - text: Does the storefront sign-in form block a 255-character email before sending the code request?
  - text: What 400 message does POST /api/otp/request return for an invalid email like john@?
concepts:
  - id: otp
  - id: api-error
status: active
appliesTo:
  - axis: feature
    value: otp-email-sign-in
  - axis: setting
    value: detailederrors-on
  - axis: surface
    value: rest
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: POST /api/otp/request
  - coordinate: /sign-in
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T22:34:59.360Z
    by: session:238f2094
    who: kutasinaelena
    build: 1212305283171b63
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: theme=2.61.0-alpha.2605; setting:PasswordLogin.DetailedErrors=on
    at: 2026-10-28T09:24:45.586Z
    by: session:7e8ea7fb
    bodyHash: fedac98d3cac
    who: Lenajava1
    contradicts: true
    contested: theme 2.61.0-alpha.2605 vs 2.59.0-pr-2477-ff59-ff590a58; virtocommerce.fileexperienceapi 3.1008.0 vs 3.1004.0; virtocommerce.otp 3.1001.0 vs 3.1000.0-pr-1-11d6; virtocommerce.profileexperienceapimodule 3.1022.0 vs 3.1019.0; virtocommerce.return 3.1006.0 vs 3.1003.0-pr-27-90cb; …
    note: On vcst_qa (theme 2.61.0-alpha.2605) the storefront sign-in email field refuses the 255-char address client-side with 'This field must not contain more than 254 characters' and sends NO POST /api/otp/request; the 254-char address is sent (200, user_not_found). So the form now has a client-side length limit, unlike the entry's earlier theme.
---
On vcptcore_qa1 (OTP module 3.1000.0-pr-1-11d6, DetailedErrors on, 2026-10-01) POST /api/otp/request {storeId, email} for a syntactically valid 254-character unregistered address returned 200 {succeeded:false, error:{code:"user_not_found", description:"No user with this email was found."}, maskedEmail:"a•••a@..."} — the response discloses that the address has no account. A 255-character address returned 400 validation problem "The field Email must be a string or array type with a maximum length of '254'."; "john@" returned 400 "The Email field is not a valid e-mail address.". The storefront sign-in form's client validator rejects john@ but has no length limit: both the 254 and 255-character addresses passed it and fired the request.

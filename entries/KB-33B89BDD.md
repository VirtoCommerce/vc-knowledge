---
id: KB-33B89BDD
subject: OTP request for an unregistered email returns succeeded:false with code user_not_found (DetailedErrors on); a 255-char email is refused 400 by MaxLength(254) while the storefront form lets it through
plane: experiential
question: What does POST /api/otp/request return for an unknown email or an over-long email, and which layer rejects the length?
status: active
appliesTo:
  - axis: feature
    value: otp-email-sign-in
  - axis: setting
    value: detailederrors-on
  - axis: surface
    value: api
anchors:
  - coordinate: POST /api/otp/request
  - coordinate: /sign-in
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T22:34:59.360Z
    by: session:238f2094
    who: kutasinaelena
---
On vcptcore_qa1 (OTP module 3.1000.0-pr-1-11d6, DetailedErrors on, 2026-10-01) POST /api/otp/request {storeId, email} for a syntactically valid 254-character unregistered address returned 200 {succeeded:false, error:{code:"user_not_found", description:"No user with this email was found."}, maskedEmail:"a•••a@..."} — the response discloses that the address has no account. A 255-character address returned 400 validation problem "The field Email must be a string or array type with a maximum length of '254'."; "john@" returned 400 "The Email field is not a valid e-mail address.". The storefront sign-in form's client validator rejects john@ but has no length limit: both the 254 and 255-character addresses passed it and fired the request.

---
id: KB-3DCCDC7A
subject: With storefront MFA required, Login on behalf asks the operator for a sign-in code only on the signed-out /account/impersonate/{userId} form; a signed-in operator clicking OK in the Company members dialog is impersonated at once with no code
plane: experiential
question: With MFA required, does a storefront operator have to enter an emailed sign-in code before Login on behalf impersonates a member?
questions:
  - text: Does the Company members Login on behalf dialog show an MFA code step?
  - text: What does /account/impersonate/{userId} do when the operator is not signed in and MFA is on?
  - text: Does a wrong MFA code on the impersonation form still impersonate the target?
status: active
appliesTo:
  - axis: feature
    value: mfa
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/members
  - coordinate: /account/impersonate/{id}
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcst_qa
    build: 5bd367d41cd309ee
    conditions: platform=3.1090.0-pr-3133-8b4a; module:VirtoCommerce.MFA=3.1000.0-pr-1-e8c7; theme=2.61.0-pr-2570-8619; store=B2B-store; setting:MultiFactorAuthentication.StoreMode=Enabled; role=Organization maintainer
    at: 2026-10-23T19:24:48.749Z
    by: session:7ed484a4
    who: kutasinaelena
---
On vcst_qa 2026-10-23 (B2B-store StoreMode=Enabled), an org maintainer signed in with password + emailed code (amr=mfa). On /company/members, Actions -> Login on behalf opened a confirm dialog with only Cancel/OK and no code field. Cancel closed it and sent no request. OK went to /account/impersonate/{targetUserId} ("Security verification"), which immediately POSTed grant_type=impersonate (200) and landed on / with the "<operator> logged in as <target>" banner; the token carried vc_operator_user_id and no amr. "Back to <operator>" restored the operator and went to /company/members. Signed out, opening /account/impersonate/{targetUserId} showed a "Security verification" form (operator email + password, Verify and continue / Cancel). Submitting it answered two_factor_required and showed the same 6-cell code step with a Cancel link. A wrong code showed "That code didn't work." and sent no impersonate request. Cancel went to / still signed out. The right code sent grant_type=mfa (200) then grant_type=impersonate (200) and showed the impersonation banner.

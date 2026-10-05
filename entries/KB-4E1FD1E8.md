---
id: KB-4E1FD1E8
subject: The impersonate OAuth grant gates on the operator's loginOnBehalf permission but applies NO guard on the target user (existence only) and mints the token from the target's identity, so impersonating an Administrator yields a token carrying the target's Administrator role
plane: experiential
question: Can a non-admin operator with loginOnBehalf impersonate an Administrator via the impersonate grant on /connect/token, and does the token carry admin rights?
status: active
appliesTo:
  - axis: grant
    value: impersonate
  - axis: surface
    value: admin-spa
  - axis: surface
    value: platform-oauth
  - axis: target
    value: administrator
anchors:
  - coordinate: /connect/token
  - coordinate: /account/impersonate/{userId}
  - coordinate: AuthorizationController.Exchange
evidence:
  - method: source-analysis (vc-platform master) + regression-run UI observation; live admin-API check pending
    deployment: vcst_qa
    at: 2026-10-03T04:41:34.244Z
    by: session:51fcfb97
    who: kutasinaelena
---
On vcst (Platform 3.1076.0) regression run REG-2026-10-02-2022 (suite 082 / IMP-022): a storefront operator holding only platform:security:loginOnBehalf (role 'Organization maintainer', non-admin) opened the built-in Administrator user record in the Admin SPA, which showed an ENABLED 'Login on behalf' button, and successfully switched the storefront into the admin account's session (banner 'John Mitchell logged in as admin'; target userId 1eb2fa8ac6574541afdb525833dadb46). Source confirms the mechanism: vc-platform AuthorizationController.Exchange() impersonate-grant block (POST /connect/token grant_type=impersonate) authorizes the OPERATOR's SecurityLoginOnBehalf permission (skipped for nested impersonation) but validates the TARGET only for existence — no IsAdministrator / user-type / lock-state / privilege guard — and builds the token from the target (context.User = impersonatedUser.CloneTyped(); CreateTicketAsync(impersonatedUser,...)), so the issued token carries the target's roles incl. Administrator. Same class as the punchout escalation in KB-ACC12F26 (that sibling grant was live-observed to yield role=__administrator / currentuser isAdministrator:true). NOT yet live-verified on 3.1076.0: calling an admin-only platform API with the admin impersonation token to read isAdministrator first-hand (the read-only repro was blocked by the test harness this session). Candidate privilege-escalation bug; contradicts BL-AUTH-005/006 and IMP-022 oracle (admins must not be impersonable).

---
id: KB-4E1FD1E8
subject: The impersonate OAuth grant gates on the operator's loginOnBehalf permission but applies NO guard on the target user (existence only) and mints the token from the target's identity, so impersonating an Administrator yields a token carrying the target's Administrator role
plane: experiential
question: Can a non-admin operator with loginOnBehalf impersonate an Administrator via the impersonate grant on /connect/token, and does the token carry admin rights?
questions:
  - text: Can a non-admin user with login-on-behalf rights log in as the administrator?
  - text: /connect/token grant_type=impersonate target Administrator guard AuthorizationController.Exchange
  - text: Does the impersonation token carry the target's Administrator role?
  - text: Is the Login on behalf button enabled on the Administrator user record for an Organization maintainer?
  - text: What does the impersonate grant check on the target user besides existence?
concepts:
  - id: impersonation
  - id: permission
  - id: access-token
status: active
appliesTo:
  - axis: grant
    value: impersonate
  - axis: target
    value: administrator
  - axis: surface
    value: admin-ui
  - axis: surface
    value: rest
anchors:
  - coordinate: /connect/token
  - coordinate: /account/impersonate/{userId}
  - coordinate: AuthorizationController.Exchange
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-03T04:41:34.244Z
    by: session:51fcfb97
    who: kutasinaelena
    note: "how: source-analysis (vc-platform master) + regression-run UI observation; live admin-API check pending"
    build: 59983c56933fe571
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: platform=3.1090.0; surface=admin-ui
    at: 2026-10-28T09:39:35.498Z
    by: session:7e8ea7fb
    bodyHash: 3ae5a715e779
    who: Lenajava1
    note: "REG-2026-10-28-1013 / IMP-022: Admin SPA Security > Users > demo_admin (Account type Administrator) blade toolbar shows an enabled 'Login on behalf' button (UI only; token not minted this run)."
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: platform=3.1090.0; module:VirtoCommerce.Customer=3.1031.0; module:VirtoCommerce.Xapi=3.1028.0; theme=2.61.0-alpha.2605; store=B2B-store; role=Organization maintainer
    at: 2026-10-28T20:42:56.723Z
    by: session:ccf68ec7
    bodyHash: 3ae5a715e779
    defect: suspected
    who: Lenajava1
    note: "Live, storefront flow: non-admin Organization maintainer with loginOnBehalf opened /account/impersonate/{id} of an Administrator-type user (isAdministrator=true, role Store administrator); POST /connect/token grant_type=impersonate answered 200 and the new session's GetPageContext user returned isAdministrator: true with operator = the org maintainer. Reverted with 'Back to'. Closes the 'live check pending' gap: the session does carry administrator status."
---
On vcst (Platform 3.1076.0) regression run REG-2026-10-02-2022 (suite 082 / IMP-022): a storefront operator holding only platform:security:loginOnBehalf (role 'Organization maintainer', non-admin) opened the built-in Administrator user record in the Admin SPA, which showed an ENABLED 'Login on behalf' button, and successfully switched the storefront into the admin account's session (banner 'John Mitchell logged in as admin'; target userId 1eb2fa8ac6574541afdb525833dadb46). Source confirms the mechanism: vc-platform AuthorizationController.Exchange() impersonate-grant block (POST /connect/token grant_type=impersonate) authorizes the OPERATOR's SecurityLoginOnBehalf permission (skipped for nested impersonation) but validates the TARGET only for existence — no IsAdministrator / user-type / lock-state / privilege guard — and builds the token from the target (context.User = impersonatedUser.CloneTyped(); CreateTicketAsync(impersonatedUser,...)), so the issued token carries the target's roles incl. Administrator. Same class as the punchout escalation in KB-ACC12F26 (that sibling grant was live-observed to yield role=__administrator / currentuser isAdministrator:true). NOT yet live-verified on 3.1076.0: calling an admin-only platform API with the admin impersonation token to read isAdministrator first-hand (the read-only repro was blocked by the test harness this session). Candidate privilege-escalation bug; contradicts BL-AUTH-005/006 and IMP-022 oracle (admins must not be impersonable).

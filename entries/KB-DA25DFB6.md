---
id: KB-DA25DFB6
subject: Admin SPA sign-in with a correct password for an account whose password has expired redirects to #!/changepassword and blocks the manager until a new password is set
plane: experiential
question: What happens when a non-admin manager account with an expired password signs in to the Admin SPA?
questions:
  - text: Why does Admin login land on #!/changepassword instead of the workspace?
  - text: Can a seeded restricted admin fixture with an expired password still be used for Admin SPA permission tests?
  - text: Does the Admin SPA enforce password expiry at sign-in?
status: active
appliesTo:
  - axis: module
    value: platform-security
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: POST /api/platform/security/login
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 1daf7ccef257ac50
    conditions: platform=3.1085.0; module:VirtoCommerce.SalesRep=3.1016.0-pr-26-92d4; role=non-admin Manager
    at: 2026-10-27T19:56:51.342Z
    by: session:e75fe8a4
    who: Aleksandra-Mitricheva
---
Signed in to the Admin SPA login form (POST /api/platform/security/login/) as two seeded non-admin Manager fixtures (one with customer:* only, one with catalog:read only) using their correct password. Both were redirected from #!/login to #!/changepassword with the text 'Your password has expired. You must change the password to continue using the manager', a read-only Login field and Current/New/Repeat new password inputs ('Change' disabled until filled). No workspace or menu is reachable, so any Admin-SPA permission/visibility case that signs in as such an account is BLOCKED until the fixture password is reset (e.g. by re-running the seeder). A wrong password would instead stay on the login form, so landing on #!/changepassword confirms the credential itself was accepted.

---
id: KB-AE7EF8A7
subject: With global MFA on, a customer of a Disabled store who holds the storefront role "Organization employee" in user.Roles is asked for the code in its own store; the same customer without that role is not
plane: experiential
question: Is a customer holding the Organization employee role in user.Roles asked for the MFA code at POST /connect/token in its own StoreMode=Disabled store while global MFA is on?
questions:
  - text: Does a storefront organization role make a buyer a Backoffice account for the Disabled-store MFA skip?
  - text: Why does isDisabledModeSkipped turn true after adding Organization employee to a customer?
  - text: "VCST-6686 sign-in half: org-role buyer on own Disabled store with global MFA on"
status: active
appliesTo:
  - axis: feature
    value: mfa-store-modes
  - axis: store-mode
    value: disabled
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /connect/token
  - coordinate: GET /api/mfa/modes/users/{id}/status
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f; setting:MultiFactorAuthentication.Enabled=true; setting:MultiFactorAuthentication.StoreMode=Disabled; role=Organization employee in user.Roles
    defect: VCST-6686
    at: 2026-11-04T20:08:11.673Z
    by: session:2f6b8998
    who: Lenajava1
    note: "distinct: KB-B881E350 is the org role held only on the membership (not asked); KB-DE731068 is a role-less own customer (not asked). This is the role held in user.Roles, which IS asked: the opposite outcome on the same rule."
---
vcst 2026-11-04, global MultiFactorAuthentication.Enabled written true for 6 s (19:58:21-19:58:27Z, restored false). Disposable store StoreMode=Disabled; two confirmed-email Customer accounts bound to it. One got the platform role "Organization employee" added to user.Roles by PUT /api/platform/security/users. Password grant naming the store: role-less customer -> 200 tokens, no code; the one holding Organization employee -> 400 two_factor_required. With global off, GET /api/mfa/modes/users/{id}/status for the same store row read isDisabledModeSkipped false before and true after adding the role. So a storefront org role held in user.Roles classifies the buyer as a Backoffice account and removes the Disabled-store skip (VCST-6686). An org role held only on the organization membership does not (KB-B881E350).

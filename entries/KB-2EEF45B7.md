---
id: KB-2EEF45B7
subject: "Both Backoffice MFA mode views (AngularJS blade and Admin Shell page) credit an administrator-set Enabled mode to the user: hint \"Turned on in the storefront…\" and Effective status reason \"The user turned it on.\""
plane: experiential
question: What do the Backoffice MFA mode blade and the Admin Shell mode page say after an administrator sets a user's mode to Enabled?
questions:
  - text: Does the Admin Shell MFA mode page show 'The user turned it on' for an Enabled mode the administrator set?
  - text: Can the Backoffice tell who set a user's MFA mode to Enabled?
  - text: What reason does the MFA Effective status give for a CustomerChoice store when the user mode is Enabled?
  - text: Does GET /api/mfa/modes/users/{id} return who changed the mode?
status: active
appliesTo:
  - axis: feature
    value: mfa-store-modes
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: PUT /api/mfa/modes/users/{id}
  - coordinate: GET /api/mfa/modes/users/{id}/status
  - coordinate: /apps/admin-shell#/security/user/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f; AdminShell=3.1093.0; setting:MultiFactorAuthentication.StoreMode=CustomerChoice; setting:MultiFactorAuthentication.Enabled=false; role=administrator
    defect: suspected
    at: 2026-11-04T18:50:49.138Z
    by: session:7f21950d
    who: Lenajava1
    note: "distinct: KB-C59DA55D says where the widget and Mode blade are and what they contain; this entry says which text the blade and the Admin Shell page show for an administrator-set Enabled (the attribution to the user) and that no API field records who set the mode."
---
vcst 2026-11-04, global MFA off, disposable CustomerChoice store, confirmed customer bound to it. Signed in to the AngularJS admin as administrator, opened Security > Users > the user > "Multi-factor authentication" widget, chose Enabled and saved (PUT /api/mfa/modes/users/{id} 200 {mode:Enabled}). The mode blade hint then reads "Turned on in the storefront. Counts in stores with Customer choice, where it can also be turned off." and the Effective status row of the CustomerChoice store reads "Required" / "The user turned it on." The Admin Shell (/apps/admin-shell/#/security/user/{id}, overflow "MFA: Enabled" > mode page) shows the same two texts after the administrator saved Off then Enabled there. The texts are chosen from the selected mode alone (description.<mode>; reason UserEnabled when requiredBy is User and the mode is not Required) and both admins share one locale file. Neither GET /api/mfa/modes/users/{id} ({mode} only) nor the /status response (requiredBy "User") says who set the mode, so the copy cannot be right for an admin-set Enabled, which the module README lists as settable by the administrator.

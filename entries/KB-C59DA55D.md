---
id: KB-C59DA55D
subject: "Backoffice shows a Multi-factor authentication widget (user blade, value Off/Enabled/Required) opening a Mode blade with an Effective status table per store; the Admin Shell shows it as \"MFA: Off\" inside the widget overflow menu"
plane: experiential
question: Where is the per-user MFA mode in the AngularJS admin and in the Admin Shell, and what does the mode blade show?
status: active
appliesTo:
  - axis: feature
    value: mfa-store-modes
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/admin-shell#/security/user/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f; module:VirtoCommerce.AdminAngularJs=3.1094.0; module:VirtoCommerce.AdminShell=3.1093.0
    at: 2026-11-04T11:18:03.256Z
    by: session:325c07df
    who: Lenajava1
---
2026-11-04: AngularJS admin user blade (Security > Users > a Customer user) has a widget "Multi-factor authentication" (value Off) between "Show contact" and "Roles"; it opens a blade "Multi-factor authentication of <login>" with a Mode select (Off / Enabled / Required), the hint "Not turned on. In stores with Customer choice it can be turned on in the storefront.", and an "Effective status" group: "Global setting: Off. It decides for the Backoffice sign-in and for stores set to Use global setting." plus a table Store / Store setting / Code at sign-in, one row per store with a reason ("The global setting is off.", "The store turned it off for its own customers." for a Disabled store). Admin Shell (/apps/admin-shell, user blade): the widget is not in the main widget bar (API keys, Sessions, Changes) but in the "more" overflow menu as "MFA: Off", opening a blade titled "Multi-factor authentication of <login>". No Sign-in methods widget exists on another user's blade in either UI.

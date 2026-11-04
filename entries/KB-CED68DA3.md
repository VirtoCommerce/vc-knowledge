---
id: KB-CED68DA3
subject: The organization MFA Mode blade is reached from Contacts ("Companies and contacts") via the organization row menu Manage; an Off organization in a CustomerChoice store reads "Per member — Each member decides in the storefront", Required reads "This organization requires it from all its members"
plane: experiential
question: Where is the organization MFA mode set in the AngularJS Backoffice, and what does its Effective status show?
questions:
  - text: How do I open the organization blade with the Multi-factor authentication widget?
  - text: What does Code at sign-in say for an organization with mode Off in a Customer choice store?
status: active
appliesTo:
  - axis: feature
    value: mfa-store-modes
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: PUT /api/mfa/modes/organizations/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f; module:VirtoCommerce.AdminAngularJs=3.1094.0
    at: 2026-11-04T13:06:46.833Z
    by: session:325c07df
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: platform=3.1094.0; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f; module:VirtoCommerce.AdminAngularJs=3.1094.0
    at: 2026-11-04T14:25:36.970Z
    by: session:7f21950d
    bodyHash: e218b67307f2
    who: Lenajava1
    note: "Re-observed: organization row kebab > Manage opens the blade (right-click on the row opens no menu and logs a vendor.js console error); Required preview reads \"This organization requires it from all its members.\"; Save persisted (GET read-back Required); preview without Save stores nothing."
---
AngularJS admin: Contacts opens "Companies and contacts" (no separate Organizations switcher); clicking an organization row opens its members list, the organization blade opens from the row menu (the kebab cell) > Manage. Its widget strip holds "Multi-factor authentication" (value Off). The Mode blade "Multi-factor authentication of <org name>" shows for the run CustomerChoice store, mode Off: "Per member — Each member decides in the storefront."; selecting Required (before Save): "Required — This organization requires it from all its members."; Save stores mode Required (GET /api/mfa/modes/organizations/{id}), and a member whose own mode is Off is then asked (status requiredBy Organization, password grant two_factor_required). A Manager whose role holds only mfa:read (and mfa:manage) sees no Security menu, so the user blade with the widget cannot be reached without a security read permission.

---
id: KB-BDC577C1
subject: The header Organizations panel renders a locked organization row disabled with a lock-closed icon and a title tooltip "This organization is locked. Contact your administrator for access."
plane: experiential
question: Does a locked organization row in the storefront header Organizations panel carry a tooltip, and what text?
questions:
  - text: Is the locked org row in the Organizations panel announced or tooltipped?
  - text: What text does the lock tooltip show in the org switcher?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: user
    value: multi-organization-member
anchors:
  - coordinate: Query.me.contact.organizations
evidence:
  - method: source
    deployment: vc-frontend@2d15bf088
    at: 2026-11-23T13:30:43.944Z
    by: session:7ce2b8e4
    who: Lenajava1
---
Read in vc-frontend source (feat VCST-6237, organization-list.vue): each option row sets :disabled and the radio's disabled from item.isLockedForCurrentUser (Query.me.contact.organizations), appends a lock-closed VcIcon, and binds :title to the i18n key shared.layout.header.top_header.organization_locked_tooltip (en: "This organization is locked. Contact your administrator for access.") only when locked. The click handler also returns early for a locked target resolved from organizations.value, and arrow-key navigation steps over disabled rows (:not(:disabled)). Source reading only; the 2026-11-23 ticket run's live check (checklist A8) saw the same tooltip text on the locked row. An older suite case asserted NO tooltip (known divergence F2) for the account-menu switcher.

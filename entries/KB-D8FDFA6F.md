---
id: KB-D8FDFA6F
subject: On /company/members a contact whose contact-level status is null has its NAME cell replaced by the literal text "Invite sent", while the Active column still shows a green "Active" tick.
plane: experiential
question: Why does the storefront company members list show "Invite sent" where a member's name should be, and why is that row still Active?
status: active
appliesTo:
  - axis: build
    value: 2.58.0-pr-2467
  - axis: principal
    value: org-maintainer
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: storefront-xapi
anchors:
  - coordinate: /company/members
  - coordinate: Query.organization.contacts
  - coordinate: ContactType.status
  - coordinate: ContactType.statusInOrganization
  - coordinate: ContactType.fullName
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-22T08:33:33.895Z
    by: session:0d74f522
---
Observed read-only on vcst_qa 2026-09-22, storefront build 2.58.0-pr-2467-89a3-89a38239, org AGENT-TEST-Org-AcmeCorp-20260310 (105c2c4e-23be-4258-8691-568a0ff190be), signed in as the org maintainer acme_store_maintainer_1@acme.com. One of the 16 roster rows renders its Name cell as the literal string "Invite sent" rather than a person's name. The GraphQL response for that same row (operationName GetOrganizationContacts) returns id 584bf5d5-f0f1-45ab-9630-d0faa6eca0d5, name "Sam Store", firstName "Sam", lastName "Store", fullName "Sam Store", emails ["agent-test-sr-secondstore@example.com"], status NULL, statusInOrganization "Approved", isLockedInOrganization false, and a populated securityAccounts entry (83317df7-980b-40ff-bdf0-cdac94285868). So the contact HAS a name and the API returns it in four separate fields; the page substitutes "Invite sent" for all of them. The discriminator is contact.status being null - every other row in the roster carried status "Approved" and rendered its real name. Consequence for a reader: the member's identity is not displayed at all on this row, only their email, so a maintainer auditing the roster cannot tell who the pending person is from the Name column, and any test asserting the rendered name against the API name will fail for this class of contact.

Second, independent half of the same row: despite status being null, the Active column renders the GREEN tick with img alt "Active". This is not the badge rule misfiring - it is BL-B2B-013's three-tier fallback applied server-side. The effective status is resolved before it reaches the page (membership.Status ?? contact.Status ?? Approved), so the null contact status is materialised as statusInOrganization "Approved" in the xAPI payload, and the page maps that to Active. A model that predicts the badge from the raw contact.status field will therefore mispredict this row as "Inactive"; the field the badge actually consumes is statusInOrganization.

Not established by this pass: which exact condition the page tests for "Invite sent" (status === null vs an invite-pending notion), because that needs the vc-frontend source rather than the live stand; and whether the substitution also suppresses the name in the Filters or search-by-name control.

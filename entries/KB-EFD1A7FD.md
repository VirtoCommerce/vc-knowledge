---
id: KB-EFD1A7FD
subject: "Sales-rep list share: notify only newly-added orgs, message-only edit sends nothing"
plane: experiential
question: Which organizations does the storefront notify when a sales rep saves a Customer-scope list share (Mutation.changeWishlist addSharedWithIds) and what does the push contain?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Mutation.changeWishlist
  - coordinate: Mutation.sendCustomerCommunication
  - coordinate: /account/lists
evidence:
  - method: observation
    deployment: vcptcore
    at: 2026-09-29T11:44:41.040Z
    by: session:p14892
    who: Aleksandra-Mitricheva
---
On save of a Specific-customers share the storefront sends one Mutation.sendCustomerCommunication whose organizationIds are only the orgs added in that save (existing recipients are not re-notified); a save that only changes the message sends no communication. Title is the fixed string 'A new list from your sales representative'; message is the note (or a default 'Hi! I've just shared the list ... with your organization.') plus the /shared-list/<key> link. Neither title nor body names the organization. The reader's push (pushMessages.shortMessage / header bell) is note + link only.

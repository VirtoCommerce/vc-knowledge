---
id: KB-EFD1A7FD
subject: "Sales-rep list share: notify only newly-added orgs, message-only edit sends nothing"
plane: experiential
question: Which organizations does the storefront notify when a sales rep saves a Customer-scope list share (Mutation.changeWishlist addSharedWithIds) and what does the push contain?
questions:
  - text: What notification does my company get when our account manager shares a product list with us?
  - text: If a rep adds another company to an existing list share, are earlier recipients notified again?
  - text: Does editing only the note of a shared list send a new customer communication?
  - text: Which organizationIds does the storefront pass to sendCustomerCommunication when a list share is saved?
  - text: Does the push text about a rep-shared list name the receiving organization?
concepts:
  - id: list-sharing
  - id: customer-communication
  - id: push-message
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
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
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-06T01:06:18.579Z
    by: session:b04eb8d9
    who: Aleksandra-Mitricheva
    note: "Re-observed on theme 2.59.0-pr-2476-0abb + sales-rep 3.1012.0-pr-21-8964: new share to 2 orgs -> ONE call organizationIds=[both]; adding a 3rd org -> organizationIds=[new org only], earlier orgs got no push; message-only edit and recipient removal send no SendCustomerCommunication; empty note -> default 'Hi! I've just shared the list \"<name>\" with your organization. Take a look:' + link."
---
On save of a Specific-customers share the storefront sends one Mutation.sendCustomerCommunication whose organizationIds are only the orgs added in that save (existing recipients are not re-notified); a save that only changes the message sends no communication. Title is the fixed string 'A new list from your sales representative'; message is the note (or a default 'Hi! I've just shared the list ... with your organization.') plus the /shared-list/<key> link. Neither title nor body names the organization. The reader's push (pushMessages.shortMessage / header bell) is note + link only.

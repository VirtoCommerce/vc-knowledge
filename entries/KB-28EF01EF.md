---
id: KB-28EF01EF
subject: On /account/addresses a personal user adds an address through a 'New address' dialog (Email required) saved by UpdateMemberAddresses, with no success toast; the row appears in the list
plane: experiential
question: What happens when a personal account user adds a new address on /account/addresses?
questions:
  - text: Does the storefront show a success toast after saving a personal address?
  - text: Which fields are required in the storefront New address dialog?
  - text: Which GraphQL mutation saves and deletes a personal account address?
  - text: Is the Email field required when adding a personal address on the storefront?
status: active
appliesTo:
  - axis: account
    value: personal
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/addresses
  - coordinate: Mutation.updateMemberAddresses
  - coordinate: Mutation.deleteMemberAddresses
  - coordinate: Query.currentCustomerAddresses
evidence:
  - method: observation
    deployment: vcst_qa
    build: 99622678db88d8be
    conditions: theme=2.63.0-pr-2666-7e02-7e022f10; account=personal, 0 organizations
    at: 2026-11-16T21:39:29.990Z
    by: session:9b516c10
---
Signed in as a personal account (0 organizations), /account/dashboard shows an 'Addresses' sidebar link to /account/addresses (docs: the item appears only in personal accounts). The page lists addresses in a table (Recipient name, Address, Phone, Email, Actions). 'Add new address' opens a modal 'New address' on the same URL (no navigation, no blank page) with First name*, Last name*, Email*, Phone, Country*, ZIP*, State/Province* (disabled until a country is chosen), City*, Address*, Apt. The 'Create' button stays disabled until all starred fields, Email included, are filled. Create sends mutation UpdateMemberAddresses (memberId + one address, addressType 3), returns 200 with no errors[], then GetCurrentUserAddresses refetches and the new row shows in the list. No success toast/notification appeared after save (the vc-frontend locales carry no success message under pages.account.addresses, unlike company addresses' address_saved_successful_message). Deleting: row Actions -> Delete -> confirm dialog 'Delete address' -> OK sends DeleteMemberAddresses keyed by the address id, and the list refetches without the row. No console errors or warnings throughout.

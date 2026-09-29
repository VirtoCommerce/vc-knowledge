---
id: KB-7C8B8B8B
subject: storefront 2.59 rep share dialog never writes the message to the list
plane: experiential
question: Does the storefront List settings dialog persist the Customer-share message on the wishlist, and is it shown on reopen?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/lists
  - coordinate: Mutation.changeWishlist
  - coordinate: Mutation.sendCustomerCommunication
  - coordinate: SharingSettingType.message
evidence:
  - method: observation
    deployment: vcst
    at: 2026-09-29T09:39:28.190Z
    by: session:p34188
    who: Lenajava1
  - method: observation
    deployment: vcptcore
    at: 2026-09-29T11:45:20.926Z
    by: session:p40616
    who: Aleksandra-Mitricheva
    contradicts: true
    note: On theme 2.59.0-pr-2476 (share-dialog rework) ChangeWishlist DOES carry message (with scope Customer, addSharedWithIds/removeSharedWithIds deltas); server returns sharingSetting.message and the Share dialog shows it on reopen (cap 250 via maxlength, whitespace-only saved as null). Entry holds only for the pre-rework 2.59 client.
  - method: observation
    deployment: vcptcore_qa
    at: 2026-09-29T12:22:17.199Z
    by: session:p20808
    who: Aleksandra-Mitricheva
    contradicts: true
    note: "Theme 2.59.0-pr-2476: ChangeWishlist from the Share dialog carries message; reopening Share (also after a full page reload) shows the saved message (e.g. 22/250) in the Message field, and a 250-char message persisted intact."
---
On theme 2.59 (before the share-dialog rework), choosing Sharing options = Customer and a customer reveals a Message textarea whose counter is 1000 minus the length of the share link plus two newlines (911 for a 87-char link). On Save, ChangeWishlist is sent with scope, sharingKey and the legacy single sharedWithId only - no message field - and the text travels solely in SendCustomerCommunication (organizationId singular, sendEmail and sendPush true) as '<message>\n\n<shared-list link>'. GetWishlists and ChangeWishlist do not select sharingSetting.message. Reopening the dialog shows Customer and the selected customer but no Message field at all, and Save stays disabled until a field changes; a save with the customer unchanged (e.g. description edit) sends ChangeWishlist without message and fires no SendCustomerCommunication. So on that client the backend's persisted share message is never written and never displayed.

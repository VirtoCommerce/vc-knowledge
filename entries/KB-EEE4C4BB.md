---
id: KB-EEE4C4BB
subject: Before the share-dialog rework, the storefront never writes the customer-share message to the wishlist
plane: experiential
question: Does the pre-rework storefront List settings dialog persist the Customer-share message on the wishlist?
questions:
  - text: Why is the note I wrote when sharing a list with a customer gone when I reopen the dialog?
  - text: On the older share dialog, how does the share message reach the customer if it is not saved on the list?
  - text: Does the legacy client's changeWishlist carry a message field, or only sendCustomerCommunication?
  - text: Why does editing a shared list's description not resend the customer communication?
concepts:
  - id: list-sharing
  - id: customer-communication
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
    splitFrom: KB-7C8B8B8B
  - method: observation
    deployment: vcptcore
    at: 2026-09-29T11:45:20.926Z
    by: session:p40616
    who: Aleksandra-Mitricheva
    contradicts: true
    note: On theme 2.59.0-pr-2476 (share-dialog rework) ChangeWishlist DOES carry message (with scope Customer, addSharedWithIds/removeSharedWithIds deltas); server returns sharingSetting.message and the Share dialog shows it on reopen (cap 250 via maxlength, whitespace-only saved as null). Entry holds only for the pre-rework 2.59 client.
    splitFrom: KB-7C8B8B8B
  - method: observation
    deployment: vcptcore_qa
    at: 2026-09-29T12:22:17.199Z
    by: session:p20808
    who: Aleksandra-Mitricheva
    contradicts: true
    note: "Theme 2.59.0-pr-2476: ChangeWishlist from the Share dialog carries message; reopening Share (also after a full page reload) shows the saved message (e.g. 22/250) in the Message field, and a 250-char message persisted intact."
    splitFrom: KB-7C8B8B8B
---
On theme 2.59 (before the share-dialog rework), choosing Sharing options = Customer and a customer reveals a Message textarea whose counter is 1000 minus the length of the share link plus two newlines (911 for a 87-char link). On Save, ChangeWishlist is sent with scope, sharingKey and the legacy single sharedWithId only - no message field - and the text travels solely in SendCustomerCommunication (organizationId singular, sendEmail and sendPush true) as '<message>\n\n<shared-list link>'. GetWishlists and ChangeWishlist do not select sharingSetting.message. Reopening the dialog shows Customer and the selected customer but no Message field at all, and Save stays disabled until a field changes; a save with the customer unchanged (e.g. description edit) sends ChangeWishlist without message and fires no SendCustomerCommunication. So on that client the backend's persisted share message is never written and never displayed.

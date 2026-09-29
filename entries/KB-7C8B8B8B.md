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
---
On theme 2.59 (before the share-dialog rework), choosing Sharing options = Customer and a customer reveals a Message textarea whose counter is 1000 minus the length of the share link plus two newlines (911 for a 87-char link). On Save, ChangeWishlist is sent with scope, sharingKey and the legacy single sharedWithId only - no message field - and the text travels solely in SendCustomerCommunication (organizationId singular, sendEmail and sendPush true) as '<message>\n\n<shared-list link>'. GetWishlists and ChangeWishlist do not select sharingSetting.message. Reopening the dialog shows Customer and the selected customer but no Message field at all, and Save stays disabled until a field changes; a save with the customer unchanged (e.g. description edit) sends ChangeWishlist without message and fires no SendCustomerCommunication. So on that client the backend's persisted share message is never written and never displayed.

---
id: KB-40BBC91A
subject: With Return.NotifyOrganizationEmail on, every buyer return email is copied to the organization's first email unless that address is the buyer's To
plane: experiential
question: Who receives return emails when the organization copy setting is on?
status: active
appliesTo:
  - axis: surface
    value: notifications
anchors:
  - coordinate: Return.NotifyOrganizationEmail
  - coordinate: POST /api/notifications/journal
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-06T17:53:21.738Z
    by: session:b6bd53fc
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T15:24:13.606Z
    by: session:62b7421e
    who: Lenajava1
    note: "Re-observed on vcst_qa: with the setting on, Registered, Approved and Cancelled-after-submit each went once to the buyer and once to the organization's first email (cc/bcc null, both Sent); an organization email equal to the buyer's in different case produced one email; an organization with no email produced only the buyer's Rejected email, no error; a draft cancelled without submit produced nothing; a buyer already removed from the organization still triggered the copy. Setting off: one email only. The buyer's To is the order's address email, not the account email."
---
Return.NotifyOrganizationEmail is a store-level Boolean, default false, not public, labelled "Copy return emails to the organization". With it on (and Return.SendNotifications on), each buyer return email — Registered, Cancelled, Approved, also when an admin authorizes — is sent a second time To the organization's FIRST email with the same subject and body and empty CC/BCC. No copy when that address equals the buyer's To (compared case-insensitively), when the organization has no email, or when the return has no organization; with SendNotifications off no email is sent at all. The push still goes to the buyer's contact only. Draft create, edit and cancel send nothing. A buyer removed from the organization still triggers copies to the former organization. Writes took effect on the next submit.

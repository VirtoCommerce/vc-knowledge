---
id: KB-4E45A8BA
subject: admin contact Status picker cannot represent Invited or Locked
plane: experiential
question: Can an administrator put a contact back into the Invited state, or read the invitation state from the Admin contact blade?
status: active
refutableBy: observation
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: GET /api/members
  - coordinate: ContactType.status
evidence:
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-12T08:25:53.998Z
    by: session:f00f5968
---

The Status control on the Admin CONTACT blade has the same defect as the one on the account blade, with different missing values and a worse consequence. Its dropdown offers exactly four items - New, Approved, Rejected, Deleted - while the platform writes two more into that same field: Invited, set when a member is invited, and Locked, set when a maintainer uses Block user on the storefront roster. Both render in the control as their literal text with no hint that they are outside its own list. So the invitation state is visible in Admin but NOT settable from Admin: once a contact leaves Invited - by registering, or by a block/unblock cycle, or by an operator touching this picker - no administrator can put it back, and the whole invitation lifecycle has no reverse gear on either surface. Read this control as display-only unless you intend one of the four values, and never open it on a contact reading Invited or Locked expecting to restore what was there.

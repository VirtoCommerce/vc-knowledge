---
id: KB-E17E4CEF
subject: admin account Status picker cannot represent the values it displays
plane: experiential
question: Why does the Admin account Status field show a value that is not in its own dropdown?
questions:
  - text: Why does a user's status show Locked but the status list only offers four other choices?
  - text: Is it safe for an admin to change the account Status picker on a Locked or PendingApproval user?
  - text: "How should I stop an account from signing in: the Status field or the Lock account button?"
  - text: Is the security account status a free string, and which values can the fixed picker not represent?
  - text: What does the Status control show for a freshly invited back-office account?
concepts:
  - id: security-account
  - id: account-lockout
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: GET /api/platform/security/users
evidence:
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-11T18:01:19.199Z
    by: session:8df1fb2c
---

The Status control on the Admin security account blade is a fixed four-item picker offering Approved, Deleted, New and Rejected, but the underlying field is a free string and the platform writes values into it that the picker does not contain - Locked and PendingApproval were both observed rendered in that same control on live accounts. The consequence is a one-way door: an operator who opens the picker on an account whose status is Locked or PendingApproval cannot put the original value back, because it is not among the four, and the control gives no sign that the current value is outside its own list. Treat this field as display-only unless you intend one of the four; to change whether an account can sign in, use the Lock/Unlock account toolbar button, which writes the lockout and leaves Status alone. Note also that a freshly invited account has this field empty, showing the 'Select ...' placeholder rather than any status.

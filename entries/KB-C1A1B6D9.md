---
id: KB-C1A1B6D9
subject: The sales-rep 'Send a message' modal from the customer profile Send email quick action is a centred 564 px panel at 1920 px and full-screen at 768 px and 375 px, with Cancel and Send pinned to the bottom
plane: experiential
question: How does the Send a message modal opened from the Send email quick action on /company/my-customers/{id} render at desktop 1920, tablet 768 and mobile 375 px widths?
questions:
  - text: Is the sales rep Send a message dialog full-screen on mobile?
  - text: At 768 px is the customer communication modal on the customer profile a panel or full screen?
  - text: Where are the Cancel and Send buttons of the Send a message modal on a phone-width viewport?
  - text: Does the Send a message modal clip or overflow horizontally at tablet or mobile width?
status: active
appliesTo:
  - axis: module
    value: salesrep
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers/{id}
evidence:
  - method: observation
    deployment: local
    conditions: theme=2.60.0; store=B2B-store; role=sales-rep; browser=chrome
    at: 2026-10-20T15:39:52.067Z
    by: session:70c763de
    who: yuskithedeveloper
    note: "distinct: KB-5F164230 records what the modal sends (Mutation.sendCustomerCommunication arguments) and what it shows for an org without recipients; this entry records only its responsive layout - centred panel at 1920 px versus full-screen with a bottom-pinned Cancel/Send footer at 768 and 375 px - which that entry does not state."
---
Signed in as a sales representative who serves the organization, on /company/my-customers/{id} a click on the Quick actions 'Send email' tile opens the dialog 'Send a message'. At a 1920x1080 viewport it is a 564x522 px panel centred in the viewport (left 678, top 279): header 'Send a message' with a Close button, subtitle 'Reaches every member of <org>', Title, Message*, Send via* (Email, Push notification), then a footer with Cancel (left) and Send (right), 128x44 each. At 768x1024 and at 375x812 the same dialog fills the whole viewport (768x1024 and 375x812): header and Close (68x68) at the top, the form full width inside 24 px side gutters (Title 712 px / 319 px wide, Message 720 px / 327 px wide), and the footer pinned to the bottom edge of the viewport with Cancel on the left and Send on the right (128x44 at 768, 154x44 at 375). Nothing is clipped and nothing overflows horizontally at either width; at 375 the subtitle wraps onto two lines. Send stays disabled on a fresh draft at every width. The width at which the panel turns full-screen was not probed between 768 and 1920.

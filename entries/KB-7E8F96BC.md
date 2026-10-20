---
id: KB-7E8F96BC
subject: On /account/profile an emptied required First name inserts a 16 px 'This field is required' row that pushes Last name, Email, Default language/currency and Update down 16 px
plane: experiential
question: On /account/profile, does the 'This field is required' message under an emptied First name move the fields and the Update button below it?
questions:
  - text: Does a validation error on the profile page cause a layout shift?
  - text: Do the required-field messages on /account/profile reserve their space?
  - text: Why does the Update button on the profile page jump down when First name is cleared?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/profile
evidence:
  - method: observation
    deployment: local
    conditions: browser=firefox-151; viewport=1440x900; role=Sales Representative; theme=2.60.0
    defect: suspected
    at: 2026-10-20T19:02:01.574Z
    by: session:70c763de
    who: yuskithedeveloper
---
Firefox 151, 1440x900, theme 2.60.0, signed in as a Sales Representative. On /account/profile the First name was emptied with Ctrl+A, Delete and Tab. Measured with getBoundingClientRect before and after: the First name details row goes from display none to flex (margin-top 4px + min-height 12px, a 10 px font), and the First name wrapper grows from 66 to 82 px. Last name, Email, the Default language and Default currency selects and the Update button all move down 16 px. Nothing moves horizontally and nothing above First name moves. The cause is in the kit: VcInputDetails hides an empty details row (empty:hidden) unless the consumer sets showEmptyDetails, which VcInput supports, and the profile page does not set it. Typing the value back removes the message and the layout returns. Nothing was saved; Update stays disabled.

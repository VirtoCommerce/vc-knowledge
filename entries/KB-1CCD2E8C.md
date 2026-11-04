---
id: KB-1CCD2E8C
subject: On /account/security the two-factor widget drops keyboard focus to the document body after a confirmed change and after Cancel; the outcome is announced only by the global Notifications toast, and the initial "code sent" is not announced
plane: experiential
question: Where does keyboard focus go after the two-factor change code is confirmed or cancelled on /account/security, and which of its messages reach a live region?
questions:
  - text: Does the MFA Turn on / Turn off button get focus back after the code step closes?
  - text: Is the two-factor authentication outcome announced to screen readers on the storefront?
  - text: Is the 'we sent a code to' message of the MFA change step announced?
  - text: What happens to focus when Cancel is pressed in the two-factor code step?
status: active
appliesTo:
  - axis: feature
    value: mfa-self-service
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/security
  - coordinate: Mutation.updateMfaUserMode
  - coordinate: Mutation.requestMfaChangeCode
evidence:
  - method: observation
    deployment: vcst_qa
    build: ad2ed2b97c10a544
    conditions: theme=2.62.0-pr-2622-caac-caac275a; module:VirtoCommerce.MFA=3.1002.0-pr-3-1a0f; platform=3.1094.0; setting:MultiFactorAuthentication.StoreMode=CustomerChoice; store=B2B-store; lane=playwright-chrome
    defect: suspected
    at: 2026-11-04T18:28:26.975Z
    by: session:7f21950d
    who: Lenajava1
    note: "distinct: KB-26E71681 is the Passkeys card (Add passkey, Delete); this is the Two-factor authentication widget's code step (Turn on/off, Cancel, change code) and adds what is and is not announced."
---
Live 2026-11-04, Chrome, keyboard only, CustomerChoice store, customer with a confirmed email. Tab to Turn on, Enter: the button unmounts, the inline code step opens with focus in the "Code" field; its subtitle "To turn on two-factor authentication, enter the code we sent to a•••d@example.com." is a plain paragraph, the polite sr-only region and the role=alert slot of the step are empty, and the global "Notifications" region gets nothing, so the send is not announced. Cancel (Enter): the step unmounts, a new Turn on button renders, activeElement is the document body and the next Tab lands on the footer's first link. A correct code (Turn on twice, Turn off once): same, focus on body; the outcome "Two-factor authentication is on." / "...is off." appears as a toast inside the page-level region "Notifications" (notifications host, aria-live polite in source), which exists before the toast is inserted. Wrong code: role=alert "That code didn't work.", field aria-invalid, focus kept. Resend puts its message in the step's polite region.

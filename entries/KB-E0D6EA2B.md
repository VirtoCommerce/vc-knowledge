---
id: KB-E0D6EA2B
subject: Storefront /account/profile and the /account/addresses dialog show a rejected name or address field inline on its input (theme 2.61.0-pr-2597); the header Ship-to "Add new" dialog shows only a toast
plane: experiential
question: How does the storefront show an updateContact or updateMemberAddresses validation error on /account/profile and the address dialog?
questions:
  - text: Does /account/profile flag the Last name input when the name contains a digit?
  - text: Why does the Ship to Add new address dialog show only a toast for an invalid name?
  - text: What does the address dialog show when the first name contains a digit?
status: active
appliesTo:
  - axis: account
    value: personal
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/profile
  - coordinate: /account/addresses
  - coordinate: Mutation.updateMemberAddresses
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: c50527824dbc1f59
    conditions: platform=3.1085.0; theme=2.61.0-pr-2597-372a; module:VirtoCommerce.ProfileExperienceApiModule=3.1022.0-pr-150-afd6
    at: 2026-10-27T07:27:00.578Z
    by: session:980244dd
    who: Aleksandra-Mitricheva
---
As a personal storefront user on theme 2.61.0-pr-2597 with ProfileExperienceApi 3.1022.0-pr-150: on /account/profile, Last name "Runner2" + Update shows "Contains invalid characters" under the Last name input, no toast, nothing saved (3/3); two invalid names flag both inputs; editing a flagged input clears only its own error and Update stays disabled while a server error is shown. On /account/addresses, Add new address with First name "Jane2" keeps the dialog open, flags First name, creates nothing (request sends only the new address, error field addresses[0].firstName); "<b>x</b>" in Address shows "HTML tags are not allowed". The header Ship to → Add new dialog sends the existing address plus the new one, gets the error at addresses[1].firstName, and shows only the toast "Some of the addresses are not valid" with no inline field error. Messages are translated text, no console errors.

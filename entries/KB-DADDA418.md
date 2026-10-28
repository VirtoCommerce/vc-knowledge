---
id: KB-DADDA418
subject: Contact.selectedAddressId is a per-user, per-organization customer preference; updateContact with selectedAddressId "" stores an empty string (me returns "" not null), and only deleting the preference row restores null
plane: experiential
question: How is me.contact.selectedAddressId stored and scoped, and how do you clear it back to null?
questions:
  - text: Does the selected ship-to address follow the active organization after an organization switch?
  - text: Why does me.contact.selectedAddressId return an empty string instead of null?
  - text: Where is the SelectedAddressId preference stored on the platform?
status: active
appliesTo:
  - axis: module
    value: profile-experience-api
  - axis: surface
    value: xapi-graphql
  - axis: user
    value: organization-member
anchors:
  - coordinate: Mutation.updateContact
  - coordinate: Query.me
  - coordinate: POST /api/customer-preferences/search
  - coordinate: DELETE /api/customer-preferences
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 0361e2adb11cbebb
    conditions: module:VirtoCommerce.ProfileExperienceApiModule=3.1022.0-pr-149-2242; module:VirtoCommerce.Customer=3.1031.0; module:VirtoCommerce.Xapi=3.1028.0; store=B2B-store
    at: 2026-10-28T23:44:32.516Z
    by: session:faa575c7
    who: kutasinaelena
---
Signed in as a storefront buyer (B2B-store), updateContact(command:{id, firstName, lastName, selectedAddressId:<org address id>}) made Query.me contact.selectedAddressId return that id; an admin POST /api/customer-preferences/search for the user showed one row named "SelectedAddressId.<organizationId>". For a two-organization member, after selecting an address in organization A, a refresh_token grant with organization_id=B made me.contact.selectedAddressId null (B has its own key), and switching back to A returned A's address again. Sending selectedAddressId:"" stores an empty value: me then returns "" (not null). A null selectedAddressId in the command is ignored (handler writes only when non-null). Deleting the row via DELETE /api/customer-preferences?ids=<id> made me return null again. Same behaviour on the OLD build (3.1022.0) and the PR #149 build.

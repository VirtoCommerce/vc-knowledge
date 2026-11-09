---
id: KB-6CC1EFE6
subject: Deleting a security account leaves its contact (and vice versa); an invited contact may be missing from member keyword search
plane: experiential
question: How do I fully remove a test member, and why can I not find the leftover afterwards?
questions:
  - text: How do I completely remove a test user so neither their login nor their contact record remains?
  - text: Does deleting a back-office security account also delete the linked customer contact, or vice versa?
  - text: Contacts search says No data after I deleted someone - does that prove the contact is gone?
  - text: How can I find an invitation-created contact with an empty name that keyword search does not return?
  - text: Which admin list bypasses the member search index and reliably confirms an account was deleted?
concepts:
  - id: member-deletion
  - id: security-account
  - id: contact
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: DELETE /api/platform/security/users
evidence:
  - method: observation
    deployment: vcptcore_stable
    at: 2026-09-11T18:06:58.509Z
    by: session:8df1fb2c
    build: be10ad6a04a12ec3
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_regression
    build: 9648699210e92bd7
    conditions: platform=3.1076.13; module:VirtoCommerce.Customer=3.1029.1
    at: 2026-11-09T15:44:13.970Z
    by: session:cb5a6bb9
    bodyHash: 0bb78cda38a5
    who: Aleksandra-Mitricheva
    note: "Contact-side account delete (Contacts > contact > Accounts > Delete) also left the contact in the DB. Additionally observed: right after that delete, the contact's Member index document was gone (GET /api/search/indexes/index/Member/{id} -> []), so keyword search returned No data for a contact that had been indexed minutes earlier (it was not an invited contact)."
---
A contact and its security account are two records with two delete paths, and neither cascades to the other. Deleting the account under Security > Users names only '1 account(s)' in its confirmation and leaves the contact intact; deleting the contact under Companies and contacts names only '1 contact(s)'. Removing someone completely therefore takes both deletes, in either order. The trap is verifying it: a contact created by an invitation is often not in the member search index, so the 'Companies and contacts' keyword box returns a clean 'No data' for a contact that is still there - the same index behaviour recorded separately for member keyword search. Do not read 'No data' as proof of deletion. Sort the unfiltered list by Name instead: a contact created by an invite has an empty name and sorts to the top, and opening it puts its member id in the URL so you can match it against the id you are chasing. Security > Users does not go through the index, so it is the reliable check for the account half.

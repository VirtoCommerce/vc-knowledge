---
id: KB-D173BBB8
subject: PUT /api/sales-rep rewrites the rep contact's organizations to the served list; an org dropped from it loses the rep's whole membership
plane: experiential
question: Does PUT /api/sales-rep with an organizations list delete the rep's organization memberships for organizations not in that list?
questions:
  - text: What happens to a sales rep's other organization memberships when the admin edits the rep's served organizations?
  - text: Does removing a served organization from a sales rep remove only the sales-rep role or the whole membership?
  - text: Is it safe to call PUT /api/sales-rep on a user who is also a buyer member of other organizations?
status: active
appliesTo:
  - axis: actor
    value: admin
  - axis: surface
    value: platform-api
anchors:
  - coordinate: PUT /api/sales-rep
  - coordinate: POST /api/customer/organization-memberships/search
  - coordinate: GET /api/sales-rep/{id}
evidence:
  - method: observation
    deployment: virtostart
    build: f4e35ed21176a918
    conditions: module:VirtoCommerce.Customer=3.1029.0; role=Advanced Sales Representative
    at: 2026-10-22T13:21:15.154Z
    by: session:da35f399
    who: Lenajava1
---
Rep account (Advanced Sales Representative) with 10 organization memberships, only 2 of them carrying the sales-rep role, so GET /api/sales-rep/{id} listed 2 served orgs. (1) PUT /api/sales-rep echoing the same organizations back: nothing changed. (2) PUT adding one org where the user already had a buyer membership (Organization maintainer + Organization returns manager): the sales-rep role was ADDED to that membership, the other roles kept, all 10 memberships survived; the contact's organizations[] was overwritten to exactly the 3 served orgs. (3) PUT removing that org again: the WHOLE membership was deleted, buyer roles included, not just the sales-rep role. The other 7 non-served memberships survived both single-org calls. Earlier, a PUT that added 3 orgs to this rep, at a time when the contact's organizations[] listed 12 orgs, was followed by 8 of the user's non-served memberships disappearing; consistent with memberships following the contact's organizations[] when it shrinks. So treat PUT /api/sales-rep as destructive to any membership the rep holds outside the served list, and re-check memberships after it.

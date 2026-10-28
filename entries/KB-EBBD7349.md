---
id: KB-EBBD7349
subject: With PR #149, a wrong-type member id in contact(id)/organization(id) answers Forbidden like a nonexistent member, and an Employee user's me.contact is null with no errors
plane: experiential
question: What do Query.contact(id) with an Organization id, Query.organization(id) with a Contact id, and me.contact for an Employee user return on xAPI with PR #149?
questions:
  - text: Does contact(id) with an organization id still error on the PR #149 build?
  - text: Is me.contact null for an Employee storefront user?
status: active
appliesTo:
  - axis: module
    value: profile-experience-api
  - axis: surface
    value: xapi-graphql
anchors:
  - coordinate: Query.contact
  - coordinate: Query.organization
  - coordinate: Query.me
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 0361e2adb11cbebb
    conditions: module:VirtoCommerce.ProfileExperienceApiModule=3.1022.0-pr-149-2242; module:VirtoCommerce.Customer=3.1031.0; module:VirtoCommerce.Xapi=3.1028.0; platform=3.1090.0; store=B2B-store
    at: 2026-10-28T23:21:17.473Z
    by: session:faa575c7
    who: kutasinaelena
---
On the PR #149 build, signed in as a single-org storefront buyer (B2B-store), contact(id:<own Organization id>), organization(id:<own Contact id>) and contact(id:<Employee member id>) each returned Forbidden, the same answer as contact(id:<nonexistent GUID>), with no NULL_REFERENCE. Signed in as a storefront user whose member is an Employee, me{id memberId contact{id memberType}} returned contact null and no errors. The factory now returns null for a member of another type, so these lookups take the not-found path.

---
id: KB-CBA98E35
subject: On ProfileExperienceApi 3.1022.0 (no PR #149), a member id of the wrong type in contact(id)/organization(id) answers NULL_REFERENCE, and an Employee user's me.contact is an object whose Contact fields fail
plane: experiential
question: What do Query.contact(id) with an Organization id, Query.organization(id) with a Contact id, and me.contact for an Employee user return on xAPI without PR #149?
questions:
  - text: Why does contact(id) return NULL_REFERENCE 'Error trying to resolve field contact'?
  - text: What does me.contact return for a storefront user whose member is an Employee?
  - text: Does organization(id) accept a contact id?
concepts:
  - id: graphql-api
  - id: contact
  - id: organization
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
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: module:VirtoCommerce.ProfileExperienceApiModule=3.1022.0; module:VirtoCommerce.Customer=3.1031.0; module:VirtoCommerce.Xapi=3.1028.0; store=B2B-store
    defect: VCST-6280
    at: 2026-10-28T23:21:29.773Z
    by: session:faa575c7
    who: kutasinaelena
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: module:VirtoCommerce.ProfileExperienceApiModule=3.1022.0; module:VirtoCommerce.Customer=3.1031.0; module:VirtoCommerce.Xapi=3.1028.0; store=B2B-store
    at: 2026-10-28T23:44:16.959Z
    by: session:faa575c7
    bodyHash: 326d2df70945
    who: kutasinaelena
    note: "Re-run independently: also contact(id:<Vendor id>) and organization(id:<Employee id>) answer NULL_REFERENCE; a nonexistent GUID answers Forbidden."
---
Signed in as a single-org storefront buyer (B2B-store), POST /graphql contact(id:<the buyer's Organization id>), organization(id:<the buyer's Contact id>) and contact(id:<an Employee member id>) each returned data null with errors[] NULL_REFERENCE "Error trying to resolve field 'contact'/'organization'" (an NRE inside the profile authorization handler, which dereferences the aggregate's null Contact/Organization). contact(id:<nonexistent GUID>) returned Forbidden instead. Signed in as a storefront user whose member is an Employee, me{contact{id memberType}} returned an object with memberType Employee, and me{contact{organizationId selectedAddressId fullName}} returned three NULL_REFERENCE errors. Cause: MemberAggregateFactory.Create<T> wrapped any member type. Addressed by vc-module-profile-experience-api PR #149 (VCST-6280), unmerged as of 2026-10-29.

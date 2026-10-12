---
id: KB-E129A8EF
subject: "PUT /api/organizations is a full replace: omitting status and description sets them to null; memberships survive"
plane: experiential
question: Does a rename through PUT /api/organizations with only id, memberType, name and emails keep the organization's other fields?
status: active
appliesTo:
  - axis: actor
    value: admin
  - axis: surface
    value: rest
anchors:
  - coordinate: PUT /api/organizations
  - coordinate: GET /api/organizations/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-12T10:46:55.290Z
    by: session:573e5582
    who: Lenajava1
---
2026-10-12 on vcst_qa. An Approved organization with a description and one organization membership (with a membership role) was renamed by an admin through PUT /api/organizations with the body {id, memberType: Organization, name, emails}. GET /api/organizations/{id} afterwards showed the new name and the same emails, but status went from Approved to null and description to null; nothing else differed apart from modifiedDate. The membership (POST /api/customer/organization-memberships/search by organizationId) was unchanged, roles and status included. Follows: a rename or a restore must GET the organization and PUT it back whole with only the name changed. A body carrying only some fields silently nulls the omitted scalars. Two other test organizations on the same stand, written earlier by such minimal bodies, already carried status null.

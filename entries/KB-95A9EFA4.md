---
id: KB-95A9EFA4
subject: Anonymous Query.whiteLabelingSettings(storeId) answers themePresetName on POST /graphql of the platform; /xapi/graphql returns 404 on both platform and storefront hosts
plane: experiential
question: Which URL serves the xAPI GraphQL endpoint on the QA stand, and can an anonymous caller read the store's theme preset name?
questions:
  - text: what is the GraphQL endpoint path for xapi on the stand
  - text: does whiteLabelingSettings need a token
status: active
appliesTo:
  - axis: auth
    value: anonymous
  - axis: surface
    value: xapi-graphql
anchors:
  - coordinate: POST /graphql
  - coordinate: Query.whiteLabelingSettings
evidence:
  - method: observation
    deployment: vcst_qa
    build: 24b60c4b061158d9
    at: 2026-10-27T19:45:30.642Z
    by: session:e0e42495
    who: Lenajava1
---
POST {platform host}/graphql with {whiteLabelingSettings(storeId:"B2B-store"){themePresetName}} and no Authorization header returned HTTP 200 with data.whiteLabelingSettings.themePresetName = "red". The same body on {platform host}/xapi/graphql and on {storefront host}/xapi/graphql returned HTTP 404.

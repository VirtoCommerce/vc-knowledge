---
id: KB-3C5DB2E7
subject: Concurrent identical saves of a localizable dictionary setting did not store duplicate translations on vcptcore_dev
plane: experiential
question: Can firing identical concurrent POST /api/platform/localizable-settings/{name}/dictionary-items requests reproduce duplicate PlatformLocalizedItem rows?
status: active
appliesTo:
  - axis: setting
    value: order.status
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/platform/localizable-settings/{name}/dictionary-items
  - coordinate: GET /api/platform/localizable-settings
evidence:
  - method: observation
    deployment: vcptcore_dev
    build: 16ef5057616c6cbb
    conditions: platform=3.1084.0-pr-3139-3b31; role=Administrator
    at: 2026-10-14T18:40:41.460Z
    by: session:55081948
    who: yuskithedeveloper
---
On vcptcore_dev, 148 concurrent identical POST /api/platform/localizable-settings/Order.Status/dictionary-items requests (rounds of 2 to 24 in parallel, each adding one new translation to one value) all returned 204, and the following GET /api/platform/localizable-settings never listed the language twice: 84 on the pre-fix build 3.1084.0-alpha.13422, 64 on 3.1084.0-pr-3139-3b31. Duplicates that make every later save 500 (VCST-6148) therefore cannot be manufactured over REST on this stand; each save first re-saves the setting and publishes ObjectSettingChangedEvent before it reads the stored translations, which spreads the requests apart. A double-clicked Save in the Admin editor did send two requests on the pre-fix build, but they did not store a duplicate here either. To test the 'save heals an existing duplicate' path, seed the duplicate row in the database instead of racing requests.

---
id: KB-AB9E68CC
subject: "A Boolean store setting cannot be put back to null (inherit) through REST: value null is stored as false and value \"\" returns 500"
plane: experiential
question: How do I clear a Boolean store setting back to the inherited default through PUT /api/stores?
questions:
  - text: Does writing value null to a Boolean store setting restore the default?
  - text: Why did my store setting become false after I sent null?
  - text: Can POST /api/platform/settings delete an object-level Boolean setting value?
status: active
appliesTo:
  - axis: objecttype
    value: store
  - axis: surface
    value: rest
  - axis: valuetype
    value: boolean
anchors:
  - coordinate: PUT /api/stores
  - coordinate: POST /api/platform/settings
  - coordinate: GET /api/stores/{id}
evidence:
  - method: observation
    deployment: vcptcore_qa1
    conditions: store=B2B-store; setting=ProductSubscription.Enabled
    at: 2026-11-16T17:45:51.658Z
    by: session:d9b8e380
    who: kutasinaelena
    note: "distinct: KB-1F34FB02 is about a ShortText setting, which \"\" clears to null; this is about a Boolean setting, where \"\" fails with 500 and null is silently stored as false, so it cannot be cleared at all."
---
B2B-store, ProductSubscription.Enabled (valueType Boolean, defaultValue true) read value null / itHasValues false. After it was set to false with a full-store PUT /api/stores, sending the same settings[] entry with value null (PUT /api/stores 204, and POST /api/platform/settings with the object entry 204) read back value false, not null — the feature stayed OFF, silently. Sending value "" returned HTTP 500 "String '' was not recognized as a valid Boolean." Source (vc-platform SettingEntity.FromModel -> SettingValueEntity.SetValue) converts with Convert.ToBoolean(value), and Convert.ToBoolean(null) is false. So a Boolean object setting once written can only be set to true or false via REST; the only way to get the effective default back is to write the default value explicitly (here true), which leaves an explicit store row (itHasValues true) instead of inheritance. ShortText/LongText settings in the same store DO clear with value "" (read back null, itHasValues false). Restoring a Boolean toggle with null is therefore a trap: it leaves the opposite of the default.

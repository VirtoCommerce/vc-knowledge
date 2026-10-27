---
id: KB-FCAEBC07
subject: Admin Shell fulfillment center blade shows only Products and Audit log widgets; the classic blade also has a Dynamic properties widget
plane: experiential
question: Which widgets does the fulfillment center blade have in the Admin Shell compared with the classic admin?
questions:
  - text: Does the Admin Shell fulfillment center blade have dynamic properties?
  - text: fulfillment center widgets vc-shell vs AngularJS admin
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/admin-shell#/inventory
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: fcd091cc4597b860
    conditions: platform=3.1090.0-alpha.13438; module:VirtoCommerce.AdminShell=3.1000.0-pr-1-34b3; module:VirtoCommerce.Inventory=3.1011.0-pr-169-4a65
    defect: suspected
    at: 2026-10-27T10:19:20.433Z
    by: session:5de4e186
    who: kutasinaelena
---
Opened the same fulfillment center (Chicago Branch) in both UIs without editing. Classic blade (#!/workspace/inventory) shows widgets including "Dynamic properties" (count 0) and Products. Admin Shell blade (#/inventory) shows only Products (counter "99+" instead of the exact 127) and Audit log records; no dynamic properties anywhere on the blade. The Admin Shell rich-text editor also mis-parses the stored HTML description table (the SUN row is lost, raw </td></tr> markup appears in a cell).

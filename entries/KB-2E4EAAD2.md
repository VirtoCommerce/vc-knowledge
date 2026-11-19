---
id: KB-2E4EAAD2
subject: The Admin Shell Orders blade has no Export command, with or without selected rows; the stand serves no AngularJS admin
plane: experiential
question: Can orders be exported from the Orders blade of the Vue Admin Shell, and does Export respect selected rows?
questions:
  - text: Is there an Export button in Admin Shell Orders list?
  - text: Where is the AngularJS admin on vcptcore-qa?
  - text: How to reproduce 'export selected orders exports all' on the Admin Shell
status: active
appliesTo:
  - axis: surface
    value: admin-shell
anchors:
  - coordinate: GET /api/platform/modules
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: c2cbb357a76b771a
    conditions: platform=3.1099.0; module:VirtoCommerce.AdminShell=3.1095.0; module:VirtoCommerce.Orders=3.1020.0; module:VirtoCommerce.Export=3.1006.0; role=administrator
    at: 2026-11-19T06:44:40.581Z
    by: session:cf1a4a19
    who: kutasinaelena
---
Signed in as admin to the Admin Shell (root URL serves the shell; /api/platform/modules lists AdminShell 3.1095.0 and no AngularJS admin module, platform 3.1099.0). Orders > Orders (#/orders, 2661 orders): toolbar is Refresh / Delete / Send to AvaTax only. Selecting two rows enables Delete and Send to AvaTax; no Export command appears. So the AngularJS 'Export selected orders exports everything' scenario cannot be reproduced on this stand. Source: vc-module-order dev AngularJS scripts (53 files) register no export toolbar command either, so an 'Export orders' blade on the classic Customer orders list comes from a custom (non-platform) module.

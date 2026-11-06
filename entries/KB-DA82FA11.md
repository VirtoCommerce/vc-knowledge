---
id: KB-DA82FA11
subject: On a Published or Archived loyalty mission the Admin details blade keeps its inputs editable but disables Save, Reset and Publish; typed edits are never saved
plane: experiential
question: Are the fields of a Published loyalty mission read-only in the Admin mission blade?
questions:
  - text: Can I type into the Name of a published loyalty mission in Admin?
  - text: How does the Admin loyalty mission blade enforce immutability?
concepts:
  - id: loyalty-mission
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: PUT /api/loyalty-missions
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 99880d06bbb9b4cf
    conditions: platform=3.1076.8; module:VirtoCommerce.Loyalty=3.1009.0; role=Administrator
    at: 2026-11-06T10:48:42.948Z
    by: session:6eca33a7
    who: Aleksandra-Mitricheva
---
On Loyalty 3.1009.0, opening a Published mission in the Admin SPA shows the banner "Only draft missions can be edited. Published or archived missions are read-only and cannot be changed." The Name input still accepts typing (its value changes), but Save, Reset and Publish stay disabled and only Archive is enabled. Nothing is persisted: the mission's read-back is unchanged after reload. Immutability is enforced by the toolbar commands (canSave requires Draft) and by the server (PUT of a Published mission is refused), not by read-only inputs.

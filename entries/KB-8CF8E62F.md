---
id: KB-8CF8E62F
subject: In the Admin Shell, a classic AngularJS screen opened via #/legacy?url= renders in light classic styling regardless of the shell Appearance (dark or light), and the rail highlight does not follow the legacy route
plane: experiential
question: Does the classic AngularJS frame inside the Admin Shell follow the shell's dark/light appearance, and which rail section is highlighted on #/legacy routes?
questions:
  - text: Why is the classic orders blade light inside a dark Admin Shell?
  - text: Which rail item is highlighted when opening #/legacy?url=/workspace/orders?
status: active
appliesTo:
  - axis: surface
    value: admin-shell
anchors:
  - coordinate: /apps/admin-shell#/legacy?url=/workspace/orders
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: fcd091cc4597b860
    conditions: platform=3.1090.0-alpha.13438; module:VirtoCommerce.AdminShell=3.1000.0-pr-1-34b3; vc-shell=2.6.3-rc.0
    at: 2026-10-26T19:00:36.648Z
    by: session:5de4e186
    who: kutasinaelena
    note: "distinct: KB-92B2EE06 is about how menu items resolve to the legacy route; this is about how the legacy frame renders (theme, geometry, rail highlight) once open."
---
Signed in as admin, opened /apps/admin-shell/#/legacy?url=/workspace/orders (iframe loads /?EmbeddedMode=true). With Appearance=Dark and Appearance=Light the classic frame looked identical: light content area with slate-grey blade headers; it does not adopt the shell appearance. The iframe fills the content area exactly (shell top bar 60px above it, no right gap) at 1920x1080 and 1440x900. Rail highlight under the legacy route is not derived from the route: on a fresh deep link it shows Home; after clicking another rail section (e.g. Products) that section stays highlighted while the classic Orders screen is displayed. Order blade Store field resolved to its store name (POST /api/stores/search 200). Frame was blank ~6-10 s before the classic grid rendered.

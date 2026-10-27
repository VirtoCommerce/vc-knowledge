---
id: KB-5E30CF5F
subject: Admin Shell order list column chooser offers raw model properties but no Currency or Confirmed/IsApproved column that the classic order grid shows by default
plane: experiential
question: Which columns can the Admin Shell orders list show compared to the classic admin order grid?
status: active
appliesTo:
  - axis: surface
    value: admin-shell
anchors:
  - coordinate: /apps/admin-shell#/orders
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: fcd091cc4597b860
    conditions: shell=@vc-shell/framework 2.6.3-rc.0; platform=3.1090.0
    at: 2026-10-27T09:51:13.145Z
    by: session:5de4e186
    who: kutasinaelena
---
Signed in as admin, opened the classic #!/workspace/orders grid: default columns Order number, Customer, Store, Total, Currency, Confirmed, Status, Created. Opened the Admin Shell #/orders list: default columns Order number, Customer, Store, Created, Status, Total; the Show/Hide Columns menu lists ~70 raw CustomerOrder property names (Row Version, Addresses, Items, Scopes, Fee Details, Operations Log, etc.) but no Currency and no Confirmed/Is Approved entry.

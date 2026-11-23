---
id: KB-F5DF5D55
subject: On the Sales Rep customer profile, the Sales statistics block shows only three KPI tiles for a closed quarter (Q3), leaving the fourth grid cell empty
plane: experiential
question: How many KPI tiles does the Sales statistics block on /company/my-customers/:id render for a closed quarter, and does the two-column grid stay filled?
status: active
appliesTo:
  - axis: feature
    value: sales-rep-hub-plugin
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers/{id}
  - coordinate: /company/customer-sales
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 36b0ead443a99f6c
    conditions: module:VirtoCommerce.SalesRep=3.1026.0-pr-38-5280; theme=2.63.0-pr-2635-8318; browser=firefox; viewport=1280
    at: 2026-11-23T23:39:11.153Z
    by: session:2d819cd0
    who: kutasinaelena
---
Signed in as a sales rep, opened /company/my-customers/<guid>, clicked the Q3 period tab (Jul-Sep 2026, closed). The KPI grid (two columns, 12px gap) rendered Sales, Orders and Average order value only; the 'month to date' tile is absent for a closed period, so the second row holds one tile and the right cell is empty. For the 6 months period the same grid renders four tiles. Orders tile did appear in the first row; layout otherwise unchanged. The same panel on /company/customer-sales uses four columns at wide widths.

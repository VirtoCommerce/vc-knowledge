---
id: KB-5DEDBAA9
subject: Sales Rep hub surfaces with every block hidden render an "All blocks are hidden" empty state; Restore default layout issues one SaveSalesRepLayout carrying registry defaults (incl. maxRows) and does not enter edit mode
plane: experiential
question: What does the Sales Rep hub dashboard / customer profile show when every block is hidden, and what does Restore default layout send?
status: active
appliesTo:
  - axis: module
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/dashboard
  - coordinate: /company/my-customers/{orgId}
  - coordinate: Mutation.saveSalesRepLayout
  - coordinate: Query.salesRepLayout
evidence:
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-07T14:19:59.264Z
    by: session:89850094
    who: Aleksandra-Mitricheva
---
On theme 2.59.0 PR build 2539, hiding all stats and widgets on /company/dashboard (scope dashboard) or /company/my-customers/{orgId} (scope customerProfile) and saving replaces the regions and foot Edit layout toggle with an empty state: H2 "All blocks are hidden", body text, primary "Restore default layout" and outline "Edit layout". After the Save focus lands on the empty-state Edit layout button. On reload only SalesRepLayout plus page-level queries fire (no widget/stat queries). Empty-state Edit layout opens edit mode with every block in Hidden stats / Hidden widgets and sends no save; Cancel returns the empty state. Restore default layout (even on double-click) sends exactly one SaveSalesRepLayout with all blocks hidden:false in registry order and default settings (dashboard: orders maxRows 5, top_sellers maxRows 5, tasks maxRows 5 — a user-set orders maxRows 3 is reset), stays out of edit mode, announces "Default layout restored." (localized, e.g. de "Standardlayout wiederhergestellt.") in the aria-live paragraph and focuses the surface root. The customerProfile document is per rep: the empty state shows on every served customer. The build does not render the design's "Nothing is lost" hint chip.

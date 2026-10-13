---
id: KB-8BE777BB
subject: Switching organization relabels the storefront header account control at once; the control renders empty only on pages where an injected ProductGuide widget hides it
plane: experiential
question: After a multi-org buyer switches organization from the account menu, does the header show the new organization name?
questions:
  - text: After I switch to another company in the account menu, why does the top bar no longer say which company I'm buying for?
  - text: Does a multi-organization buyer's header button show the new organization name straight after switching, or only after reload?
  - text: Why does the account menu button lose its text and accessible label after a client-side cart or organization update?
  - text: How does the header organization slot differ between a buyer in one organization and a buyer in several?
  - text: Should an end-to-end case reload the page before asserting the header shows the organization chosen via changeOrganization?
concepts:
  - id: multi-organization
  - id: site-header
status: active
appliesTo:
  - axis: actor
    value: multi-org-buyer
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/members
  - coordinate: Query.me
  - coordinate: Mutation.changeOrganization
evidence:
  - method: observation
    deployment: virtostart
    at: 2026-09-23T10:13:24.367Z
    by: session:cdb27d99
    who: Dan-BV
    build: c48b806793d9e97d
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-08T12:21:33.732Z
    by: session:b9db3f52
    who: Aleksandra-Mitricheva
    contradicts: true
    note: "On vcptcore_qa (theme 2.59.0-pr-2458) the header was relabelled immediately when a multi-org sales rep switched org from the account menu while on the home page (Purple Pink123) or on /company/my-customers/{id} (Watermelon123): \"Org / Alla Volkova\" plus the logo label rendered right after the switch reload. The blank header happened only when the switch reload landed on a product page (3 of 3)."
    resolved: claim-amended
    resolvedAt: 2026-10-13T06:14:58.487Z
    resolvedBy: session:34990ccd
    resolvedIn: judge/list-sharing-20261012
    resolution: The switch relabels the header at once (vcst 6/6, vcptcore off widget pages); it blanks only where the stand's ProductGuide widget injects '.hidden{display:none !important}', as the dispute's PDP-only cases suggested (vcptcore, 2026-10-13).
    amendedFrom: 80dee7c80526
    build: f960736e1109b3de
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.60.0-pr-2511-81f1; role=multi-org-buyer; state:productguide-widget=absent
    at: 2026-10-13T06:14:46.185Z
    by: session:34990ccd
    note: "2026-10-13 1920px, 12-org buyer: switch from / x2, /catalog x2, two PDPs -> header 'ACME Store N / John Doe' right after the reload and after SPA nav, every time; org restored."
    investigation: true
    bodyHash: 5b0e39ec7940
    build: 4ca08b7825765bae
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    conditions: theme=2.60.0-pr-2522-6f90; role=sales-rep; state:productguide-widget=on some PDPs
    at: 2026-10-13T06:14:47.986Z
    by: session:34990ccd
    note: "2026-10-13: switch from /, /catalog, Xerox PDP -> labelled; from the Epson ET-2550 PDP (widget mounts) labelled ~1.3 s then empty ~2.1-2.6 s (2/2), empty after SPA nav; label span computed display none, hidden by pg-widget-styles '.hidden{display:none !important}'. top-header.vue identical at 81f18d01 and 6f902c9e. Org restored."
    investigation: true
    bodyHash: 5b0e39ec7940
    build: c55c1a124b302d3b
    buildFrom: deploy-history
---
After choosing another organization in the account menu the context switches (theme, cart) and the header account button is relabelled 'NewOrg / UserName' right after the switch reload: vcst (theme 2.60.0-pr-2511-81f1, 12-organization buyer, 2026-10-13) every time from /, /catalog and two PDPs; vcptcore (2.60.0-pr-2522-6f90, multi-org sales rep) from /, /catalog and a PDP without the widget. It renders empty (chevron only) when the switch reload lands on a PDP where the ProductGuide widget mounts: on vcptcore the Epson ET-2550 PDP showed the label at ~1.3 s and lost it at ~2.1-2.6 s, 2/2, while the logo-side org label kept the new organization. The account button's org/name label is a span with classes 'hidden min-w-0 gap-1 font-bold xl:flex' (top-header.vue). The ProductGuide widget (cdn.useproductguide.com/pg.js, injected by the stand, not theme code) adds <style id="pg-widget-styles"> with '.hidden { display: none !important; }' when it mounts on a page; the !important beats xl:flex, so the label stays in the DOM with display none and the button shows only its chevron. The style survives SPA navigation, so the button stays empty on later routes until a full load of a page without the widget. So the blank header is a stand-specific third-party CSS collision, not a defect of the organization switch; the virtostart 2.58.0 observation (blank after the switch and after a plain add-to-cart) was not re-checked and is most likely the same collision. A multi-org buyer gets 'OrgName / UserName' inside the account button, a single-org buyer the org name in a slot beside the logo.

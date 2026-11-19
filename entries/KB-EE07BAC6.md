---
id: KB-EE07BAC6
subject: The flag-on storefront preview answers the designer's connect with a loaded message carrying protocol 2, capabilities and template conditions, reports each resolution with the fields white-labeling overrides, and after a full reload inside the designer frame stays on /designer-preview and handshakes again
plane: experiential
question: What does the storefront send in the Site Builder preview v2 handshake, and does the preview survive a reload inside the designer frame?
questions:
  - text: Which capabilities does the storefront preview advertise?
  - text: What is in the resolved message of the designer preview?
  - text: Does a language or currency switch inside the designer preview break the preview?
status: active
appliesTo:
  - axis: module
    value: sitebuilder
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /designer-preview
  - coordinate: /apps/site-builder-shell
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: theme=2.63.0-pr-2676; module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-28d0; store=B2B-store
    at: 2026-11-19T18:27:40.042Z
    by: session:51ac2fe3
    who: Lenajava1
---
Recorded postMessage traffic in the designer and the frame. Designer to frame: connect {protocol:2, source:builder}, auth {token,userId}, inspector, colorScheme {scheme}, draft {theme:{rules, header, footer, palette}}, render {document, page, cultureName, sectionLabels}. Frame to designer: loaded {data:{blocks, sections, shared, objects, templates, settingsSchema, paletteTokens, hash, protocol:2, capabilities:[templates, layouts, groups, inline-edit, color-scheme, inspector], conditions:[inCategory, productType, property, path, language, source, setting], builtInTemplates}}, then resolved {url:/designer-preview, pageType:page, template:page, ruleId:null, overriddenByWhiteLabeling:[brand.logoUrl, brand.secondaryLogoUrl, palette, menus.header, menus.footer]} on B2B-store. The designer's Dark theme radio sent colorScheme dark and the frame's html got class dark. With the inspector off, choosing Deutsch in the preview header or EUR in the currency menu reloaded the frame; it stayed at /designer-preview (no /de prefix), re-handshaked (new loaded and resolved), kept the page's culture en-US from the render message (pinnedLocale de-DE stored but not applied), and showed EUR.

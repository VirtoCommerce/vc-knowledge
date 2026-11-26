---
id: KB-6C0AA08A
subject: Store setting WhiteLabeling.WhiteLabelingEnabled=false makes the storefront apply no white labeling at all, including an enabled organization's branding
plane: experiential
question: "Which switch turns off organization white labeling on the storefront: the store White labeling blade's Enabled toggle or the store setting WhiteLabeling.WhiteLabelingEnabled?"
questions:
  - text: Does WhiteLabeling.WhiteLabelingEnabled=false on a store hide an organization's logo and theme on the storefront?
  - text: What is the master switch for white labeling on the storefront?
  - text: Why does org branding still show after disabling the store's white labeling record?
status: active
appliesTo:
  - axis: feature
    value: white-labeling
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: PUT /api/stores
  - coordinate: Query.whiteLabelingSettings
evidence:
  - method: observation
    deployment: vcst_qa
    build: 56f07d69d90abb81
    conditions: platform=3.1101.0; theme=2.63.0-alpha.2681; store=B2B-store; module:VirtoCommerce.WhiteLabeling=3.1008.0
    at: 2026-11-26T16:54:34.510Z
    by: session:d2ee4088
    who: Lenajava1
    note: "distinct: KB-A23A4A41 is about the store white-labeling RECORD (xAPI merge); this is the separate store SETTING WhiteLabeling.WhiteLabelingEnabled, which the storefront honours as a master switch that suppresses org branding too."
---
On vcst_qa 2026-11-26 (B2B-store, theme 2.63.0-alpha.2681), a buyer of an organization whose white-labeling record is enabled (own logo, favicon, Watermelon preset, own main-menu link list). With only the store's White labeling blade toggle OFF (store record isEnabled=false), the home page still showed the org preset, org main menu and requested the org logo/favicon. After setting the store setting WhiteLabeling.WhiteLabelingEnabled=false (full-body PUT /api/stores), reloading / showed the default Virto Commerce logo, the default palette, the default catalog main menu, and sent no request for any customization asset or preset. So the store SETTING is the storefront master switch; the blade's Enabled toggle only removes the store record's own contribution.

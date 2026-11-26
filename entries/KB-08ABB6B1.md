---
id: KB-08ABB6B1
subject: "On vcst_qa (theme 2.63.0-alpha.2681) the ProductComparison module is not installed: /compare is a 404 and the storefront shows no Compare header link or Add to Compare control"
plane: experiential
question: Why does /compare return 404 and no Add to Compare button appear on the vcst_qa storefront?
questions:
  - text: Is product comparison available on vcst_qa?
  - text: Is VirtoCommerce.ProductComparison deployed on vcst_qa?
  - text: Can the compare regression cases run on vcst_qa?
status: active
appliesTo:
  - axis: deployment
    value: vcst_qa
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /compare
  - coordinate: Mutation.saveProductsToCompareList
evidence:
  - method: observation
    deployment: vcst_qa
    build: 1fe74fdd17a1b14e
    conditions: platform=3.1101.0; theme=2.63.0-alpha.2681; store=B2B-store; module:VirtoCommerce.ProductComparison=absent
    at: 2026-11-26T02:29:21.277Z
    by: session:779b94fc
    who: kutasinaelena
    note: "distinct: Module absent from the deployment, not the ProductComparison.Enabled setting turned off."
---
Guest session, Firefox, 2026-11-26 on vcst_qa (platform 3.1101.0, theme 2.63.0-alpha.2681): the header main menu lists Bulk order, Lists, Orders, Cart (no Compare entry); catalog cards and the PDP sidebar show 'Add to list'/Share/Print but no 'Add to Compare'; navigating to /compare renders 'Page not found' (title '404 Page not found'). The stand's declared module list (/api/platform/modules via preflight) contains no VirtoCommerce.ProductComparison module. Since compare moved into that module (theme 2.63 line), compare cases cannot run on this stand until the module is deployed; this is environment delivery, not a storefront defect.

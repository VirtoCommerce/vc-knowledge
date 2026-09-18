---
id: KB-8A82CF4D
subject: switching a product's configurability on and off
plane: experiential
question: how do I turn a product's configurability off, and how soon does the storefront see it
status: active
appliesTo:
  - axis: surface
    value: rest
  - axis: surface
    value: graphql
anchors:
  - coordinate: ProductConfiguration.isActive
  - coordinate: Product.isConfigurable
  - coordinate: POST /api/catalog/products/configurations/search
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T12:39:10.172Z
    by: session:a1f22912
---

isActive on the ProductConfiguration is the switch, it is honoured immediately, and NO REINDEX is involved. Saving the configuration with isActive false through POST /api/catalog/products/configurations flipped the storefront's Product.isConfigurable from true to false on the very next query, and turning it on had flipped it to true the same way seconds after the configuration was first created -- so isConfigurable is computed from the configuration entity at resolve time, not baked into the search index document, and waiting for or triggering a reindex to make a configurability change visible is wasted work. The product's own index entry is untouched by any of this: price, buyability and stock are unchanged, and the Admin product blade's index timestamp does not move. One counting trap follows from the same field. POST /api/catalog/products/configurations/search with an EMPTY criteria body counts configurations, not CONFIGURABLE PRODUCTS -- it includes deactivated ones. Measured on one product: empty body totalCount 1, {isActive:true} totalCount 0, {isActive:false} totalCount 1, with the product reading isConfigurable false. Since nothing can delete a configuration, a deactivated row is the permanent end state of every configuration anyone ever experiments with, so this gap widens over a store's life and never closes. Ask with {isActive:true} when the question is how many products a buyer can actually configure.

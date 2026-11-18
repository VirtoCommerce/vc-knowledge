---
id: KB-68CFCE57
subject: POST /api/catalog/products/configurations updates options in place only when the section has allowPredefinedOptions true; option ids are preserved when sent, and two isDefault options normalise to one
plane: experiential
question: What does the product configuration REST upsert need so that options survive, and what happens to option ids and to two isDefault options?
status: active
appliesTo:
  - axis: module
    value: catalog
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/catalog/products/configurations
  - coordinate: GET /api/catalog/products/configurations/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: e0865db93540946f
    at: 2026-11-18T13:08:48.239Z
    by: session:25a30bd6
    who: Lenajava1
    note: "distinct: Adds the allowPredefinedOptions requirement, option id preservation and the 409 on stale ids; KB-CD451F5C covers only the sections field name."
---
2026-11-18, fixture configuration with one Product section of 3 options. (1) A body with id (configuration), productId (the configured product), isActive true and a section with id, configurationId, type Product, allowPredefinedOptions true and options each carrying id, sectionId, productId (the option's PRODUCT id), quantity, isDefault returned 200; option ids were preserved (update in place). (2) The same body with the section lacking allowPredefinedOptions returned 200 but the section was saved with allowPredefinedOptions false and options [] (all options dropped); re-posting the old option ids then answered 409 (expected to affect 1 row, 0 affected). Options re-posted WITHOUT ids were created with new ids. (3) With two options isDefault true and the rest as in (1), the read-back had exactly one isDefault option (the first sent) and 3 options. A body using configurableProductId, a section key sectionId and an option id in options[].productId answered 500.

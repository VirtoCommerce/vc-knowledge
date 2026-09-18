---
id: KB-7AB0B13E
subject: removing a product configuration
plane: experiential
question: can a product configuration be deleted once it has been created
status: active
appliesTo:
  - axis: surface
    value: rest
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: POST /api/catalog/products/configurations
  - coordinate: ProductConfiguration.isActive
  - coordinate: "#!/workspace/catalog?productId="
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-14T12:17:17.498Z
    by: session:a1f22912
---

No. A ProductConfiguration is create-and-update only, on both surfaces, and this is worth knowing BEFORE you create one. The REST contract exposes exactly three routes -- POST /api/catalog/products/configurations (create or update), GET /api/catalog/products/configurations/{id}, POST /api/catalog/products/configurations/search -- plus PATCH on {id}; there is no DELETE, and the Admin SPA's Angular $resource for it declares only search and update over api/catalog/products/configurations/:id. The Admin's Product configuration blade DOES show a Delete button and the platform DOES define a catalog:configurations:delete permission, which together look like a removal path and are not one: that command's executeMethod is _.difference(currentEntity.sections, gridApi.selection.getSelectedRows()), so it removes the SELECTED SECTIONS from the in-memory entity and never calls any API. Its canExecuteMethod additionally requires sections to exist AND grid rows to be selected, so with the configuration itself selected and no section row ticked the button simply stays disabled. The nearest thing to removal is therefore a neutralising save through the same create-or-update POST -- isActive false and sections emptied -- which leaves the row, its id, its createdBy and its createdDate on the deployment forever. Also note the blade lies gently about existence: its loader calls search by productId and, on totalCount 0, synthesises {productId, isActive:false} locally, so an unconfigured product and a deactivated one present identically in the UI.

---
id: KB-C27CECBE
subject: When the Admin Shell (Vue) back office owns the root URL, the catalog 'Import CSV' blade is the CSV module's Vue page with inline column mapping, not the AngularJS 'Map columns' wizard; the AngularJS #!/ routes are not served
plane: experiential
question: Which catalog CSV import UI does an admin get on a stand where the Admin Shell (vc-shell) serves the root URL?
questions:
  - text: Is the classic AngularJS CSV import wizard reachable when the Admin Shell module is installed?
  - text: Does the Admin Shell CSV import blade remember an edited column mapping?
  - text: Why can't I find the Map columns blade in the back office?
status: active
appliesTo:
  - axis: module
    value: catalogcsvimportmodule@3.1009.0
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: POST /api/catalogcsvimport/import
  - coordinate: GET /api/catalogcsvimport/import/mappingconfiguration
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 77b26db77fe5423f
    conditions: platform=3.1094.0; module:VirtoCommerce.AdminShell=3.1093.0; module:VirtoCommerce.CatalogCsvImportModule=3.1009.0
    at: 2026-11-06T09:54:36.558Z
    by: session:1842610b
    who: kutasinaelena
---
On platform 3.1094.0 with Admin Shell 3.1093.0 and CatalogCsvImportModule 3.1009.0, the root URL serves the Admin Shell; '#!/workspace/...' AngularJS routes are rewritten to the shell's '#/' and no AngularJS UI module is installed, so the AngularJS 'VirtoCommerce CSV import' wizard (and its Map columns blade) is unreachable. Catalog > Import CSV opens the module's Vue page (src/...Web/vue/src/modules/pages/import.vue): delimiter, file upload, and an inline 'Column mapping' section with per-field CSV-column selects (with a clear 'x') and a fixed value. Live: after clearing CategoryPath and Vendor and mapping Name to AltName, Start import sent POST /api/catalogcsvimport/import with propertyMaps CategoryPath csvColumnName null, Vendor null, Name 'AltName'; the products got Name = AltName value, no Vendor, no category created. Start import stores the mapping in localStorage key catalog.csvImport.lastMapping with the mapping's eTag, and a later upload whose GET /api/catalogcsvimport/import/mappingconfiguration returns the same eTag (same header) restores it; a file with a different header gets the auto-mapping. Consequence: AngularJS-only wizard fixes (e.g. the Map columns OK/working-copy fix) cannot be verified on such a stand.

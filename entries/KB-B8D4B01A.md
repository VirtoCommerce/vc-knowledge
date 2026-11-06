---
id: KB-B8D4B01A
subject: Catalog CSV import accepts a CategoryId of another catalog (product lands in the target catalog but under the foreign category, invisible there, and blocks catalog deletion); an unknown CategoryId aborts the whole file with KeyNotFoundException
plane: experiential
question: What does POST /api/catalogcsvimport/import do when a CSV row's CategoryId belongs to another catalog or does not exist?
questions:
  - text: Why are CSV-imported products missing from the target catalog tree?
  - text: Why does deleting a catalog fail with FK_Item_Catalog_CatalogId after a CSV import?
  - text: CSV import KeyNotFoundException in LoadProductDependencies
concepts:
  - id: catalog
status: active
appliesTo:
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: POST /api/catalogcsvimport/import
  - coordinate: DELETE /api/catalog/catalogs/{id}
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 99880d06bbb9b4cf
    conditions: platform=3.1076.8; module:VirtoCommerce.CatalogCsvImportModule=3.1007.0
    defect: VCST-6664
    fixedIn: VirtoCommerce.CatalogCsvImportModule@3.1009.0
    at: 2026-11-06T12:31:32.001Z
    by: session:1842610b
    who: kutasinaelena
---
Classic Admin, CatalogCsvImport 3.1007.0. Created catalog A with one category and empty catalog B; imported into B a comma CSV Sku,Name,CategoryId with CategoryId mapped. (1) A row whose CategoryId is A's category: "Import finished successfully", 1 processed, 0 errors; the product GET shows catalogId = B, categoryId = A's category, outline under A only; catalog B lists nothing (POST /api/catalog/listentries totalCount 0) while A's category lists the product. DELETE /api/catalog/catalogs/{B} then returns 500 "conflicted with the REFERENCE constraint FK_Item_Catalog_CatalogId"; deleting the product first unblocks it (204). (2) A row with a non-existent CategoryId GUID: "Import finished with errors", Total 0 / Processed 0, the only error is a raw System.Collections.Generic.KeyNotFoundException stack trace from CsvCatalogImporter.LoadProductDependencies with no line number, and the other valid row in the same file is not imported either.

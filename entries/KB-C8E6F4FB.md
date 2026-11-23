---
id: KB-C8E6F4FB
subject: vcptcore stable, regression and qa1 serve assets from the FileSystem provider, not Azure Blob, although both provider modules are installed
plane: experiential
question: Which asset storage provider (Azure Blob or FileSystem) is active on the vcptcore stands?
questions:
  - text: Can an Azure Blob asset error be reproduced on vcptcore stable or regression?
  - text: Is the asset store on vcptcore-qa1 Azure blob storage?
status: active
appliesTo:
  - axis: deployment
    value: vcptcore_stable,vcptcore_regression,vcptcore_qa1
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/assets
  - coordinate: POST /api/assets/folder
evidence:
  - method: observation
    deployment: vcptcore_stable
    build: fa0d93fd60387c3f
    conditions: platform=3.1039.19; module:VirtoCommerce.Assets=3.1005.2; module:VirtoCommerce.AzureBlobAssets=3.1006.1
    at: 2026-11-23T12:06:38.284Z
    by: session:895ac5e9
    who: kutasinaelena
---
2026-11-23, admin over REST on vcptcore_stable (platform 3.1039.19), vcptcore_regression (3.1076.13) and vcptcore_qa1 (3.1101.0): both VirtoCommerce.AzureBlobAssets and VirtoCommerce.FileSystemAssets are installed. A file uploaded with POST /api/assets gets a url under <host>/cms-content/assets/..., and GET of it returns no x-ms-* headers and an ASP.NET static-file style ETag ("1dd7b43..."), not an Azure "0x8D..." ETag; a new folder created with POST /api/assets/folder lists empty (no .keep). So the active provider on all three is FileSystem: an Azure-only failure such as InvalidResourceName cannot be reproduced on them. On all three, creating a child folder inside a freshly created parent returns 204.

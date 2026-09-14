---
id: KB-3CC87F1E
subject: rest-api-platform-modules
plane: derived-first
question: Which endpoints does this deployment serve under /api/platform/modules, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Platform
    version: 3.1007.26
    versionedAs: platform-host
anchors:
  - coordinate: GET /api/platform/modules
    operationId: Modules_GetModules
    hash: 550f0276f8e8
  - coordinate: GET /api/platform/modules/{moduleId}/versions/{version}/validate
    operationId: Modules_ValidateModuleVersion
    hash: 93e82adb22e3
  - coordinate: GET /api/platform/modules/loading-order
    operationId: Modules_GetModulesLoadingOrder
    hash: 8c81e2590db0
  - coordinate: POST /api/platform/modules/{moduleId}/install
    operationId: Modules_InstallModule
    hash: 19495880afe2
  - coordinate: POST /api/platform/modules/{moduleId}/uninstall
    operationId: Modules_UninstallSingleModule
    hash: ad67c27a9e87
  - coordinate: POST /api/platform/modules/{moduleId}/versions/{version}/install
    operationId: Modules_InstallModuleVersion
    hash: 0dc9ee406721
  - coordinate: POST /api/platform/modules/autoinstall
    operationId: Modules_TryToAutoInstallModules
    hash: 2291b3520876
  - coordinate: POST /api/platform/modules/getdependents
    operationId: Modules_GetDependingModules
    hash: c30726172773
  - coordinate: POST /api/platform/modules/getmissingdependencies
    operationId: Modules_GetMissingDependencies
    hash: 1944bf6ecae6
  - coordinate: POST /api/platform/modules/install
    operationId: Modules_InstallModules
    hash: 8236c859931a
  - coordinate: POST /api/platform/modules/install/v2
    operationId: Modules_InstallModuleRequests
    hash: 64d08dfdd2cb
  - coordinate: POST /api/platform/modules/localstorage
    operationId: Modules_UploadModuleArchive
    hash: 85ee00b40ac0
  - coordinate: POST /api/platform/modules/reload
    operationId: Modules_ReloadModules
    hash: 808d5b8a6a60
  - coordinate: POST /api/platform/modules/restart
    operationId: Modules_Restart
    hash: 99d23668010c
  - coordinate: POST /api/platform/modules/uninstall
    operationId: Modules_UninstallModule
    hash: 444dd86a10df
  - coordinate: POST /api/platform/modules/uninstall/v2
    operationId: Modules_UninstallModuleRequests
    hash: 8c2b6775372f
  - coordinate: POST /api/platform/modules/update
    operationId: Modules_UpdateModules
    hash: fe3c2d4be9be
  - coordinate: POST /api/platform/modules/update/v2
    operationId: Modules_UpdateModuleRequests
    hash: 1040896e81d6
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/platform/modules

18 operations under `/api/platform/modules`, served by module `VirtoCommerce.Platform`, published under the tag "VirtoCommerce Platform".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `GET /api/platform/modules`<br>`Modules_GetModules` | Get installed modules | — | `ModuleDescriptor[]` |
| `GET /api/platform/modules/{moduleId}/versions/{version}/validate`<br>`Modules_ValidateModuleVersion` | Validate that a specific module version package exists at the download URL. | `moduleId` (path), `version` (path) | `boolean` |
| `GET /api/platform/modules/loading-order`<br>`Modules_GetModulesLoadingOrder` | Get module loading order | — | `string[]` |
| `POST /api/platform/modules/{moduleId}/install`<br>`Modules_InstallModule` | Install the latest available version of a module. | `moduleId` (path) | `ModulePushNotification` |
| `POST /api/platform/modules/{moduleId}/uninstall`<br>`Modules_UninstallSingleModule` | Uninstall a module. | `moduleId` (path) | `ModulePushNotification` |
| `POST /api/platform/modules/{moduleId}/versions/{version}/install`<br>`Modules_InstallModuleVersion` | Install a specific version of a module. Validates the package URL, registers the custom version, and schedules installation. | `moduleId` (path), `version` (path) | `ModulePushNotification` |
| `POST /api/platform/modules/autoinstall`<br>`Modules_TryToAutoInstallModules` | Auto-install modules with specified groups | — | `ModuleAutoInstallPushNotification` |
| `POST /api/platform/modules/getdependents`<br>`Modules_GetDependingModules` | Get all dependent modules for a module | body `ModuleDescriptor[]` (optional) | `ModuleDescriptor[]` |
| `POST /api/platform/modules/getmissingdependencies`<br>`Modules_GetMissingDependencies` | Returns a flat expanded list of modules that depend on passed modules | body `ModuleDescriptor[]` (optional) | `ModuleDescriptor[]` |
| `POST /api/platform/modules/install`<br>`Modules_InstallModules` | Install modules | body `ModuleDescriptor[]` (optional) | `ModulePushNotification` |
| `POST /api/platform/modules/install/v2`<br>`Modules_InstallModuleRequests` | Install modules using lightweight requests | body `ModuleInstallRequest[]` (optional) | `ModulePushNotification` |
| `POST /api/platform/modules/localstorage`<br>`Modules_UploadModuleArchive` | Upload module package for installation or update | — | `ModuleDescriptor` |
| `POST /api/platform/modules/reload`<br>`Modules_ReloadModules` | Reload modules | — | 204 |
| `POST /api/platform/modules/restart`<br>`Modules_Restart` | Restart web application | — | 204 |
| `POST /api/platform/modules/uninstall`<br>`Modules_UninstallModule` | Uninstall module | body `ModuleDescriptor[]` (optional) | `ModulePushNotification` |
| `POST /api/platform/modules/uninstall/v2`<br>`Modules_UninstallModuleRequests` | Uninstall modules using lightweight requests | body `ModuleInstallRequest[]` (optional) | `ModulePushNotification` |
| `POST /api/platform/modules/update`<br>`Modules_UpdateModules` | Update modules | body `ModuleDescriptor[]` (optional) | `ModulePushNotification` |
| `POST /api/platform/modules/update/v2`<br>`Modules_UpdateModuleRequests` | Update modules using lightweight requests | body `ModuleInstallRequest[]` (optional) | `ModulePushNotification` |

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-platform-modules.json`.

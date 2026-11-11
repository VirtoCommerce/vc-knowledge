---
id: KB-B11D22FA
subject: "VirtoCommerce.ApplicationInsights 3.1007.0 with VirtoCommerce.Xapi 3.1030.0 stops platform startup: Xapi fails with TypeLoadException ITelemetryProcessor, every GraphQL module fails post-init, host exits with IGraphQLTextSerializer not set"
plane: experiential
question: Can VirtoCommerce.ApplicationInsights 3.1007.0 run with VirtoCommerce.Xapi 3.1030.0?
questions:
  - text: Why does the platform fail to start with Could not load type Microsoft.ApplicationInsights.Extensibility.ITelemetryProcessor?
  - text: Platform startup fails with IGraphQLTextSerializer not set in DI container after a module upgrade
  - text: Which Xapi version is needed for ApplicationInsights 3.1007.0 (Application Insights 3.0 / OpenTelemetry)?
  - text: Every x-module fails post-initialize with No service for type GraphiQLOptions
status: active
appliesTo:
  - axis: surface
    value: rest
  - axis: surface
    value: xapi
anchors:
  - coordinate: POST /graphql
  - coordinate: GET /api/platform/modules
evidence:
  - method: observation
    deployment: vcst_qa
    build: 4605db2ed710c357
    conditions: platform=3.1097.0; module:VirtoCommerce.ApplicationInsights=3.1007.0; module:VirtoCommerce.Xapi=3.1030.0 (fails) / 3.1031.0-pr-70-108f (starts)
    at: 2026-11-11T15:24:09.963Z
    by: session:7e403256
    who: kutasinaelena
---
On vcst_qa, an env upgrade moved VirtoCommerce.ApplicationInsights 3.1005.0 -> 3.1007.0 (release note: migrate to Microsoft.ApplicationInsights.AspNetCore 3.0, VCST-4717) while VirtoCommerce.Xapi stayed 3.1030.0. The platform pod log at startup: 'Failed to initialize module VirtoCommerce.Xapi — System.TypeLoadException: Could not load type Microsoft.ApplicationInsights.Extensibility.ITelemetryProcessor from assembly Microsoft.ApplicationInsights, Version=3.1.2.115'. Then 18 modules that build on Xapi (XCatalog, XCart, XOrder, ProfileExperienceApiModule, SalesRep, Loyalty, Quote, MFA, Passkeys, News, PushMessages, XPickup, Skyflow, BackInStock, OrderManagement, Return, CustomerReviews) failed post-initialize with 'No service for type Func<GraphiQLOptions> has been registered', and the host stopped: FTL 'IGraphQLTextSerializer not set in DI container', 'Hosting failed to start'. Pinning the Xapi build from vc-module-x-api#70 (3.1031.0-pr-70-108f, which references VirtoCommerce.ApplicationInsights.Core and an OpenTelemetry processor instead of ITelemetryProcessor; its manifest declares ApplicationInsights 3.1007.0 and platform 3.1097.0) made the platform start: 92 modules, 0 load errors, /graphql answering. So ApplicationInsights 3.1007.0 needs an Xapi built against Application Insights 3.x; Xapi 3.1030.0 and earlier cannot load beside it.

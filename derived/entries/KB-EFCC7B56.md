---
id: KB-EFCC7B56
subject: no-rest-virtocommerce-xapi
plane: derived-first
question: Does VirtoCommerce.Xapi expose REST endpoints on this deployment?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.Xapi
    version: 3.1003.2
anchors:
  - coordinate: document VirtoCommerce.Xapi
    hash: 42c15180e501
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# VirtoCommerce.Xapi exposes no REST surface

The deployment serves an OpenAPI document for `VirtoCommerce.Xapi` and that document declares zero paths. This is a fact about the module, not a failed extraction: 16 of 58 documents on this deployment are in this state by construction — payment, search and asset providers, SSO, telemetry, and every xAPI module, whose entire contract is served through GraphQL instead.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`.

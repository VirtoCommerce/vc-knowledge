---
id: KB-ADFD93AB
subject: org-specific pricing runs through the organization's user groups, not through organizationId
plane: experiential
question: What makes a B2B customer who belongs to an organization get a different price?
status: active
refutableBy: observation
appliesTo:
  - axis: surface
    value: admin-ui
  - axis: surface
    value: storefront-ui
  - axis: principal
    value: org-member
anchors:
  - coordinate: PriceEvaluationContext.userGroups
  - coordinate: PriceEvaluationContext.organizationId
  - coordinate: Contract.basePricelistAssignmentId
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-16T12:27:46.196Z
    by: session:8ec21246
---

PriceEvaluationContext carries BOTH organizationId and userGroups, but the only lever the Admin assignment UI can write is UserGroupsContainsCondition -- the 'Eligible shoppers' condition menu offers Location (time zone, zip, city, country, region), Browse behaviour (searched phrase, language) and Shopper profile (age, gender, user group contains), and there is NO organization condition of any kind. What makes it work anyway is that the group set the condition is matched against includes the ORGANIZATION's groups, not just the contact's. Established directly: a contact with groups [] whose single organization carried groups ['Standard Customers','store-acme'] was served a pricelist gated on UserGroupsContains 'store-acme', while an anonymous caller on the same store and product at the same moment got the ungated list. So 'contract pricing for customer X' is really 'a group on X's organization plus an assignment conditioned on that group', and the Contracts module is exactly that pattern packaged: a Contract holds basePricelistAssignmentId and priorityPricelistAssignmentId, and the two assignments it owns here are store-scoped, priority 10000/10001, each gated on UserGroupsContains '<contract code>'. Practical consequence: to work out why a B2B customer sees a price, read the ORGANIZATION's groups, not the contact's -- a contact with no groups of its own is not a contact with no pricing conditions.

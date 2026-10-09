---
id: KB-ED1C55C0
subject: organizationReturns and returns answer a negative after cursor with an errors[] code SQL and data null; the server does not clamp it
plane: experiential
question: What do the xAPI queries organizationReturns and returns answer for a negative after cursor such as "-20"?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.organizationReturns
  - coordinate: Query.returns
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-09T20:34:24.222Z
    by: session:61a7b05a
    who: yuskithedeveloper
---
On Return 3.1005.0-pr-28-fb4f (Xapi 3.1027.0-alpha.216), a holder's organizationReturns(storeId, organizationId, first: 20, after: "-20") and after: "-1" answered HTTP 200 with errors[0] "Error trying to resolve field 'organizationReturns'." extensions.code SQL and data null; returns(storeId, first: 20, after: "-20") answered the same for 'returns'. after "0" and "20" returned pages normally (20 and the remaining rows of the same totalCount). The storefront theme of this change clamps its page parameter on the client; the server residual sits in the shared X-API paging and was left out of scope by the developer.

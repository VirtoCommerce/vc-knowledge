---
id: KB-40B9AEF4
subject: Member keyword search misses an organization only when its Member document is absent from the search index; hyphenated names are found normally
plane: experiential
question: Does POST /api/members/search with keyword find an organization named like AGENT-TEST-Org-Kingsbridge-Imports-20260514?
questions:
  - text: Why can't I find an organization by its full name in the members search?
  - text: POST /api/members/search keyword hyphenated organization name totalCount 0
  - text: Is members keyword search reliable for a find-or-create lookup of an organization?
  - text: How do I list every organization through the REST API without a keyword?
concepts:
  - id: member-search
  - id: organization
status: active
appliesTo:
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/members/search
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-05T09:55:28.969Z
    by: session:51fcfb97
    who: kutasinaelena
  - method: observation
    deployment: vcst
    at: 2026-10-05T20:19:11.378Z
    by: session:51fcfb97
    who: kutasinaelena
    contradicts: true
    note: "Hyphenation is not the cause. Re-probed 2026-10-05 with keyword=<full name> + objectIds=[id] for all 217 organizations: 204 are found, including many hyphenated AGENT-TEST-* names created the same day and later. Exactly 13 are missed, and they are one contiguous seed batch created within ~12 s (all 11 *-20260514 orgs plus Empty-Org and Suspended-Corp); every keyword, even the full exact name, misses them, while keyword-less search lists them. Those Member documents are simply absent from the search index (Member index otherwise current). Fix is a reindex of those member ids, not a lookup strategy change."
    resolved: claim-amended
    resolvedAt: 2026-10-08T11:18:48.520Z
    resolvedBy: session:d3bcc6b6
    resolvedIn: judge/theme-search-stock-20261008
    resolution: Hyphenated names are found normally on two stands (live 2026-10-08); the misses are Member documents absent from the index - 2 still missing on vcst_qa. Subject and body corrected.
  - method: observation
    deployment: vcst_qa
    conditions: platform=3.1076.0; theme=2.59.0; module:VirtoCommerce.WhiteLabeling=3.1005.0; module:VirtoCommerce.Customer=3.1029.0; module:VirtoCommerce.ElasticSearch8=3.1011.0; module:VirtoCommerce.Inventory=3.1010.0; module:VirtoCommerce.Store=3.1009.0; store=B2B-store; preset=Mercury
    at: 2026-10-08T11:18:44.497Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: fresh hyphenated org found by full name and by a token 1.5 s after creation; 40/40 recent hyphenated orgs found; of the 13 previously missed, 11 now found, 2 (created 2026-09-19, never modified) still missed by every keyword while keyword-less listing and objectIds return them."
  - method: observation
    deployment: vcptcore_qa1
    conditions: platform=3.1077.0-pr-3123-6664; theme=2.59.0-pr-2506-bb0f; module:VirtoCommerce.WhiteLabeling=3.1005.0; module:VirtoCommerce.Customer=3.1028.0; module:VirtoCommerce.ElasticSearch8=3.1011.0; module:VirtoCommerce.Inventory=3.1010.0; module:VirtoCommerce.Store=3.1009.0; store=B2B-store; preset=Red
    at: 2026-10-08T11:18:45.300Z
    by: session:d3bcc6b6
    who: Dan-BV
    note: "2026-10-08: fresh hyphenated org found in under 1 s; the same seed batch (15 orgs) all found; 22/22 sweep found."
---
A hyphen in an organization's name does not stop POST /api/members/search {memberType:'Organization', keyword} from finding it: fresh organizations named like AGENT-TEST-JUDGE-Org-Kingsbridge-Imports-<stamp> were found by the full name and by a single token within 1-2 s of creation on vcst_qa and vcptcore_qa1 (2026-10-08), and so were 40 of 40 recent hyphenated organizations on vcst_qa.

What does miss is an organization whose Member document never reached the search index: every keyword, even the exact full name, returns totalCount 0, while GET /api/members/{id} reads it and a keyword-less paged listing {memberType:'Organization', take, skip} includes it. On vcst_qa on 2026-10-05 thirteen organizations from one seed batch created within ~12 s were in that state; by 2026-10-08 eleven had been indexed and two (created 2026-09-19 and never modified) were still missing. The fix is a reindex of those member ids. Until then, a find-or-create keyed on a keyword lookup misses the existing organization and creates a duplicate on every run - resolve by a pinned id or filter a keyword-less listing by exact name.

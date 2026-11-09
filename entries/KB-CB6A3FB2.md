---
id: KB-CB6A3FB2
subject: "The group-filtered member list opened by 'Show customers' exports the whole member database: its Export ignores the groups filter"
plane: experiential
question: Does Export on the Show customers (user-group filtered) member blade export only the group's members?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: POST /api/members/search
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 9648699210e92bd7
    conditions: platform=3.1076.13; module:VirtoCommerce.Customer=3.1029.1; module:VirtoCommerce.CustomerExportImport=3.1002.0
    defect: suspected
    at: 2026-11-09T15:44:15.196Z
    by: session:cb5a6bb9
    who: Aleksandra-Mitricheva
---
Promotion condition User group is <group> > Show customers opens Companies and contacts filtered by POST /api/members/search {groups:[g], rootMembersOnly:false} (2 rows for Wholesaler). Its Export button opens 'All 336 contacts & 99 organizations will be exported to a CSV file'; the count requests behind the dialog are POST /api/members/search {memberType, objectIds:[], keyword:''} with no groups field. Export was not confirmed.

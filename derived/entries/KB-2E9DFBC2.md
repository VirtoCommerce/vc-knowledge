---
id: KB-2E9DFBC2
subject: rest-api-sql-queries
plane: derived-first
question: Which endpoints does this deployment serve under /api/sql-queries, and what does each one require?
status: active
refutableBy: derivation
appliesTo:
  - module: VirtoCommerce.SqlQueries
    version: 3.1005.0
anchors:
  - coordinate: DELETE /api/sql-queries
    operationId: SqlQueries_Delete
    hash: d839a2bc9cb1
  - coordinate: GET /api/sql-queries/{id}
    operationId: SqlQueries_GetById
    hash: 28aa93c06545
  - coordinate: GET /api/sql-queries/database-information
    operationId: SqlQueries_GetDatabaseInformation
    hash: a323c24e89b6
  - coordinate: GET /api/sql-queries/formats
    operationId: SqlQueries_GetReportFormats
    hash: 435da8135b9e
  - coordinate: POST /api/sql-queries
    operationId: SqlQueries_Create
    hash: 71f78601395d
  - coordinate: POST /api/sql-queries/execute-preview
    operationId: SqlQueries_ExecuteQuery
    hash: ffbcaf9b0728
  - coordinate: POST /api/sql-queries/execute-query/{id}
    operationId: SqlQueries_ExecuteQueryById
    hash: a1d8ac015503
  - coordinate: POST /api/sql-queries/execute/{id}/{format}
    operationId: SqlQueries_ExecuteReport
    hash: ce71a0fb18e1
  - coordinate: POST /api/sql-queries/reports
    operationId: SqlQueries_OnlyReports
    hash: 50d4dd621884
  - coordinate: POST /api/sql-queries/search
    operationId: SqlQueries_Search
    hash: bfbd5701f58a
  - coordinate: PUT /api/sql-queries
    operationId: SqlQueries_Update
    hash: 1cf27147b998
evidence:
  - method: extraction
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
---

# /api/sql-queries

11 operations under `/api/sql-queries`, served by module `VirtoCommerce.SqlQueries`, published under the tag "Sql Queries".

| operation | what the platform says it does | required input | returns |
|---|---|---|---|
| `DELETE /api/sql-queries`<br>`SqlQueries_Delete` | — | — | 200 |
| `GET /api/sql-queries/{id}`<br>`SqlQueries_GetById` | — | `id` (path) | `SqlQuery` |
| `GET /api/sql-queries/database-information`<br>`SqlQueries_GetDatabaseInformation` | — | — | `DatabaseInformation` |
| `GET /api/sql-queries/formats`<br>`SqlQueries_GetReportFormats` | — | — | `string[]` |
| `POST /api/sql-queries`<br>`SqlQueries_Create` | — | body application/json (optional) | `SqlQuery` |
| `POST /api/sql-queries/execute-preview`<br>`SqlQueries_ExecuteQuery` | — | body application/json (optional) | `SqlQueryExecuteResult` |
| `POST /api/sql-queries/execute-query/{id}`<br>`SqlQueries_ExecuteQueryById` | — | `id` (path), body application/json (optional) | `SqlQueryExecuteResult` |
| `POST /api/sql-queries/execute/{id}/{format}`<br>`SqlQueries_ExecuteReport` | — | `format` (path), `id` (path), body `SqlQueryParameter[]` (optional) | `SqlQuerySearchResult` |
| `POST /api/sql-queries/reports`<br>`SqlQueries_OnlyReports` | — | body application/json (optional) | `SqlQuerySearchResult` |
| `POST /api/sql-queries/search`<br>`SqlQueries_Search` | — | body application/json (optional) | `SqlQuerySearchResult` |
| `PUT /api/sql-queries`<br>`SqlQueries_Update` | — | body application/json (optional) | `SqlQuery` |

Module `VirtoCommerce.SqlQueries` — Sql Queries: Adds SQL query/report capabilities to the VirtoCommerce platform, enabling advanced data access directly through SQL.

Generated from `vcptcore_stable` at pin `c2f9c438eba4cd95`. Table: `derived/rest/rest-api-sql-queries.json`.

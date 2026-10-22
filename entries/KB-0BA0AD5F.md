---
id: KB-0BA0AD5F
subject: A PageBuilder permalink longer than 2048 characters returns HTTP 500 with the raw Postgres error instead of a 400
plane: experiential
question: What happens when a PageBuilder page permalink exceeds the database column length?
questions:
  - text: Is there a max length on PageBuilder permalinks?
  - text: Why does create-group-page return 500 value too long?
status: active
appliesTo:
  - axis: db
    value: postgresql
  - axis: module
    value: virtocommerce.pagebuildermodule
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/page-builder-pages/create-group-page
  - coordinate: PUT /api/page-builder-pages/grouped
evidence:
  - method: observation
    deployment: local
    build: 3a492aac51b809f5
    conditions: platform=3.1089.0; module:VirtoCommerce.PageBuilderModule=3.1036.0-pr-178-e4ff
    defect: suspected
    at: 2026-10-22T15:15:07.074Z
    by: session:2c92b249
    who: Aleksandra-Mitricheva
---
On PageBuilderModule 3.1036.0-pr-178 with PostgreSQL, a valid-character permalink of 2048 characters is stored verbatim (200), but 2049 or 10000 characters on POST create-group-page, POST grouped or PUT grouped return 500 application/json {"message":"22001: value too long for type character varying(2048)","stackTrace":null}; the stored page is unchanged. The new permalink validation does not cover length. The Page Builder shell permalink input caps typing at 1024 characters, so the UI cannot reach it, but API/AI-tool callers can.

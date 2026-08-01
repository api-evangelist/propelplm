---
name: Create and release a Propel Change (ECO)
description: Open a Change/ECO against affected items in Propel, inspect its full release package, and update it.
api: openapi/propelplm-core-openapi.yml
operations: [createChange, getChangeOrder, changePut]
---

# Create and release a Propel Change (ECO)

Governs revisions of items through Propel's change process.

## Auth
Salesforce OAuth 2.0 bearer token (`Authorization: Bearer <access_token>`).

## Steps
1. **Open the Change** — `POST /change` (`createChange`) with `{ "categoryName": "ECO", "itemNames": ["<itemNumber>"] }`.
2. **Inspect the release package** — `GET /v3/change/{changeId}?affectedItems=true&bom=true&amls=true&attachments=true`
   (`getChangeOrder`) to pull affected items, BOM, approved manufacturer lists, and attachments.
3. **Update the Change** — `PUT /v2/change/{changeId}` (`changePut`) e.g. `{ "addAffectedItems": ["<itemNumber>"] }`.

## Rules
- Change reads use the v3 Release Package endpoint; writes use v2.
- Respect Salesforce object/field permissions of the acting user; a 403 means the user cannot act on that record.

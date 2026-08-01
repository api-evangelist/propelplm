---
name: Create a Propel Item and build its BOM
description: Create a new PLM Item in Propel, attach a document or CAD link, and define its Bill of Material of child items.
api: openapi/propelplm-core-openapi.yml
operations: [createItem, updateBom, uploadAttachment]
---

# Create a Propel Item and build its BOM

Use this to onboard a new part into Propel PLM and stand up its assembly structure.

## Auth
Obtain a Salesforce OAuth 2.0 access token (Connected App, authorization-code flow) and send it as
`Authorization: Bearer <access_token>`. All calls run against `https://{orgURL}.salesforce.com/services/apexrest/PDLM/api`.

## Steps
1. **Create the Item** — `POST /item` (`createItem`) with `{ "category": "<category>", "details": { "Description": "..." } }`.
   Send custom fields WITHOUT the `PDLM__`/`PLMJ__` namespace prefix (e.g. `Revision__c`, not `PDLM__Revision__c`).
2. **Attach a document or link** — `POST /attachment` (`uploadAttachment`) with `itemName`, `objectName: "Item__c"`,
   and either a base64 file or a `link` (e.g. an Onshape/CAD URL) plus `docName`/`description`.
3. **Define the BOM** — `PUT /bom/{itemNumber}` (`updateBom`) with `{ "bomList": [ { "name": "<childItem>", "quantity": N } ] }`.
   A single BOM call supports up to 400 children; chunk larger assemblies.

## Rules
- No idempotency-key mechanism; re-`POST` creates a new record. Key updates on Item Number / record Id.
- Errors are Salesforce Apex REST JSON (see errors/propelplm-problem-types.yml); handle 400 (bad field), 401 (token), 403 (permissions).

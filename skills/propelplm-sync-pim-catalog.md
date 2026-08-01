---
name: Sync a product catalog from Propel PIM
description: Read published products, categories, channels, variants, and digital assets from Propel PIM to syndicate to a commerce or marketing channel.
api: openapi/propelplm-pim-openapi.yml
operations: [getAllCategroies, getCategorySProducts, getProductInfo, getProductSAssets]
---

# Sync a product catalog from Propel PIM

Pull published product data out of Propel PIM for a downstream channel.

## Auth
Salesforce OAuth 2.0 bearer token. PIM base URL is `{pimBaseUrl}/api/v1`.

## Steps
1. **List categories** — `GET /categories` (`getAllCategroies`).
2. **List a category's products** — `GET /categories/{categoryId}/products` (`getCategorySProducts`).
3. **Fetch product detail** — `GET /products/{productId}` (`getProductInfo`), then
   `GET /products/{productId}/attributes` and `GET /products/{productId}/variants` for full data.
4. **Fetch assets** — `GET /products/{productId}/assets` (`getProductSAssets`) for images/documents.
5. Repeat per channel via `GET /channels` and `GET /channels/{channelId}/products`.

## Rules
- PIM API is read-only; it exposes published (channel-ready) data, not draft PLM records.
- Errors are Salesforce Apex REST JSON; handle 401 (token) and 404 (unpublished/absent).

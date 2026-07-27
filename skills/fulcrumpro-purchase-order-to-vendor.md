---
name: Purchase order to vendor
description: Create a vendor in Fulcrum and issue a purchase order to source materials.
api: openapi/fulcrumpro-openapi-original.json
operations: [ListVendor, CreateVendor, CreatePurchaseOrder, GetPurchaseOrder, StatusUpdatePurchaseOrder]
---

# Purchase order to vendor (Fulcrum)

Source materials by issuing a PO to a supplier.

## Auth
`Authorization: Bearer <token>` (site-scoped JWT). Base URL
`https://api.fulcrumpro.com/api/`.

## Steps
1. Find or create the vendor. Search with `ListVendor` (POST `/api/vendors/list`);
   if new, create with `CreateVendor` (POST `/api/vendors`) and keep the id.
2. Create the purchase order with `CreatePurchaseOrder`
   (POST `/api/purchase-orders`), referencing the vendor via `vendorId` and
   adding line items.
3. Read it back with `GetPurchaseOrder` (GET `/api/purchase-orders/{purchaseOrderId}`).
4. Approve/advance the PO with `StatusUpdatePurchaseOrder`
   (POST `/api/purchase-orders/{purchaseOrderId}/status`).

## Rules
- Errors are RFC 9457 `application/problem+json`; 403 means the token lacks the
  required purchasing permission.
- No idempotency key — re-list before retrying a create.
- This flow is also exposed via the Fulcrum MCP server (approve purchase orders
  conversationally); see mcp/fulcrumpro-mcp.yml.

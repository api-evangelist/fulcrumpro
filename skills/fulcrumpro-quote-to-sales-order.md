---
name: Quote to sales order
description: Create a quote in Fulcrum for a customer and convert it into a sales order.
api: openapi/fulcrumpro-openapi-original.json
operations: [CreateCustomer, ListCustomer, CreateQuote, GetQuote, ConvertToSalesOrder, GetSalesOrder]
---

# Quote to sales order (Fulcrum)

Turn a customer inquiry into a booked sales order.

## Auth
All calls use `Authorization: Bearer <token>` — a site-scoped JWT from
Business Setup > System Data > Advanced > Public API Setup. Base URL
`https://api.fulcrumpro.com/api/`.

## Steps
1. Find or create the customer. Search existing customers with `ListCustomer`
   (POST `/api/customers/list`, offset paging via Skip/Take). If none, create
   one with `CreateCustomer` (POST `/api/customers`) and keep the returned id.
2. Create the quote with `CreateQuote` (POST `/api/quotes`), referencing the
   customer via `customerId`. (Do not use `CreateQuote_deprecated` at `/api/quote`.)
3. Read it back with `GetQuote` (GET `/api/quotes/{quoteId}`) to confirm pricing
   and line items.
4. Convert to a sales order with `ConvertToSalesOrder`
   (POST `/api/quotes/{quoteId}/to-sales-order`).
5. Confirm with `GetSalesOrder` (GET `/api/sales-orders/{salesOrderId}`); the
   order carries `createdFromQuoteId` back to the originating quote.

## Rules
- Errors are RFC 9457 `application/problem+json`; a 404 means the referenced
  record does not exist, 403 means the token lacks the required Fulcrum permission.
- No idempotency key is supported — do not blindly retry POSTs; re-list to check
  whether a create already succeeded.

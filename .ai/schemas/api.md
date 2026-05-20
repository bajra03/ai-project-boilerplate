# API Contracts

REST endpoints exposed by this app. Update when routes change.

## Conventions

- **Base path:** `/api`
- **Auth:** `Authorization: Bearer <token>` or session cookie (see `.ai/patterns/api-routes.md`)
- **Content type:** `application/json` request + response
- **IDs:** UUIDs in URLs
- **Pagination:** `?cursor=<id>&limit=<n>` (cursor-based)

## Response shapes

```ts
// Success
{ "data": T, "meta"?: { "cursor"?: string } }

// Error
{ "error": { "code": "...", "message": "...", "details"?: {} } }
```

## Status codes

- 200 OK
- 201 Created
- 204 No Content (DELETE)
- 400 Bad Request (validation)
- 401 Unauthorized (no/bad token)
- 403 Forbidden (auth but no permission)
- 404 Not Found
- 409 Conflict (duplicate / state mismatch)
- 429 Rate Limited
- 500 Internal Server Error

## Endpoints

<!-- TODO: list endpoints. Example: -->

### `GET /api/tours`

List tours.

**Query:** `?operator_id=<uuid>&status=published&cursor=<id>&limit=20`

**Response 200:**
```json
{ "data": [{ "id": "...", "title": "...", "price_usd": "120.00" }], "meta": { "cursor": "..." } }
```

### `POST /api/bookings`

Create a booking.

**Body:**
```json
{ "tour_id": "uuid", "guests": 2, "date": "2026-06-12" }
```

**Response 201:** `{ "data": { "id": "...", "status": "pending" } }`
**Errors:** 400 invalid_body · 404 tour_not_found · 409 no_availability

<!-- ... -->

## Webhooks

Incoming webhooks land at `/api/webhooks/<provider>`.

Pattern: **verify signature → idempotency check → process → 200**

```
POST /api/webhooks/stripe
POST /api/webhooks/<other-provider>
```

Auth: raw HMAC signature in `Stripe-Signature` header (or equivalent). Do **not** use Bearer tokens.

Rules:
- Always read body as raw text before verifying — `req.text()`, not `req.json()`.
- Return `200` even if event was already processed — vendors retry on non-2xx.
- Reject unknown/unverifiable payloads with `400 invalid_signature`.

<!-- TODO: list registered webhook URLs and which events each handles, once configured -->

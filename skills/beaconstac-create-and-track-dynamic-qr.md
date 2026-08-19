---
name: Create a dynamic QR Code and read its scan analytics
description: Create an editable, trackable QR Code on Uniqode, download the image, repoint it later, and pull its scan performance from the reporting API.
api: collections/beaconstac-uniqode-api.postman_collection.json
generated: '2026-08-13'
method: generated
source: collections/beaconstac-uniqode-api.postman_collection.json
operations:
  - POST /api/2.0/qrcodes/
  - GET /api/2.0/qrcodes/{id}/
  - GET /api/2.0/qrcodes/{id}/download/
  - PUT /api/2.0/qrcodes/{id}/
  - PUT /api/2.0/qrcodes/{id}/activate/
  - DELETE /api/2.0/qrcodes/{id}/activate/
  - POST /reporting/2.0/?method=Products.getPerformance
---

# Create a dynamic QR Code and read its scan analytics

Use this when the goal is a QR Code whose destination can change after it is printed, and whose
scans need to be measured. A **static** QR Code (`qr_type: 1`) cannot be edited and cannot be
tracked — if either matters, it must be dynamic (`qr_type: 2`).

## Before you start

- **Auth.** Every request carries `Authorization: Token <API_KEY>`. The key comes from the API
  section of the Uniqode dashboard sidebar. HTTPS only — plain HTTP fails.
- **Entitlement.** API access requires a **Pro plan or above**. On Free/Starter/Lite there is no
  key to issue, so a 401 here may be a billing fact, not a credential mistake.
- **Tenancy.** Org-scoped calls take an `organization` id. On multi-user accounts it is required.
- **Budget.** 10 req/sec on Pro, 25 on Plus, 100 on Business+, against a monthly quota. No
  rate-limit headers are returned, so throttle client-side; you cannot read your remaining budget.
- **No idempotency.** There is no idempotency key. A retried `POST /api/2.0/qrcodes/` creates a
  **second QR Code**. Only retry a create after a `GET` confirms the first one did not land.

## Steps

1. **Create the code.** `POST /api/2.0/qrcodes/` with `qr_type: 2`, a `name`, the `organization`
   id, and a `campaign` object. The campaign is what makes it dynamic; its type decides behaviour
   (Custom Url, Landing Page, Feedback Form, PDF, Menu, vCard Plus, App Links, Coupon, Social
   Media, Facebook Page, Business Card, Schedule, Multi-language Content, Phone Call, SMS, Email).
   Optional: `template` (a `QRCodeTemplate` id) for house styling, `attributes` for inline design,
   `tags` for reporting rollups, `place` for location, `view_limit` to cap scans,
   `location_enabled` to collect GPS.
2. **Read it back.** `GET /api/2.0/qrcodes/{id}/`. Store the returned `id` and `url` — the `url`
   is what the code encodes and it does not change when you repoint the campaign.
3. **Download the image.** `GET /api/2.0/qrcodes/{id}/download/?size=1024&error_correction_level=2&canvas_type=pdf`.
   Choose `canvas_type` for the medium (raster for screens, vector for print).
4. **Repoint it later.** `PUT /api/2.0/qrcodes/{id}/` with the updated `campaign`. This is the
   whole point of a dynamic code: the printed artwork stays valid.
5. **Pause instead of deleting.** `DELETE /api/2.0/qrcodes/{id}/activate/` moves `state` to `S`
   (sleeping) and the code stops resolving; `PUT` on the same path brings it back to `A`.
   `DELETE /api/2.0/qrcodes/{id}/` is a hard delete and is **not** reversible — never use it to
   "turn off" a code that is already in the field.
6. **Measure it.** `POST /reporting/2.0/?organization={org}&method=Products.getPerformance` with
   `from` and `to` as **epoch milliseconds** and `product_type: "qr"` in the body. The response is
   `{"points": [...], "columns": [...]}`. Other methods on the same endpoint:
   `Products.getOsDistribution`, `Products.getCityDistribution`, `Products.getLocationDetail`,
   `Products.getVisitorDistribution`, `Products.getTimeOfDayDistribution`, and
   `Csv.getLocationData` for a CSV export.

## Rules and gotchas

- Reporting lives on a **different base** (`/reporting/2.0/`) and is **RPC, not REST** — the
  operation is chosen by the `method` query parameter, and everything is a `POST`.
- Every path ends in a **trailing slash**. Dropping it returns an HTML 404, not JSON.
- Time windows in reporting are epoch **milliseconds**, not seconds and not ISO 8601.
- Errors are `{"detail": "..."}` — a human string with no machine-readable code. Branch on the
  HTTP status, not on the message text. See `errors/beaconstac-problem-types.yml`.
- List endpoints paginate with `page` and `page_size`; response envelope field names are not
  published, so read them from the first response rather than assuming.
- A QR Code stops working if the account lapses. Deactivation is not the only way a code dies.

## Related

- `conventions/beaconstac-conventions.yml` — pagination, filtering, error envelopes, tenancy
- `data-model/beaconstac-data-model.yml` — the QRCode / Campaign / Template / Tag / Place graph
- `rate-limits/beaconstac-rate-limits.yml` — per-plan limits and quotas

---
name: Generate QR Codes in bulk from a CSV
description: Validate a CSV/XLSX and generate a batch of Uniqode QR Codes from it, then collect the rendered images.
api: collections/beaconstac-uniqode-api.postman_collection.json
generated: '2026-08-13'
method: generated
source: collections/beaconstac-uniqode-api.postman_collection.json
operations:
  - POST /api/2.0/bulkqrcodesv2/csv-validation/
  - POST /api/2.0/bulkqrcodesv2/
  - GET /api/2.0/media/
  - GET /api/2.0/media/{id}/
---

# Generate QR Codes in bulk from a CSV

Use this when the batch is large enough that one-at-a-time `POST /api/2.0/qrcodes/` would burn
the per-second rate limit — asset tagging, per-store signage, per-unit packaging codes.

## Use V2. The original bulk resource is deprecated.

`/api/2.0/bulkqrcodes/` is marked **deprecated** in Uniqode's own reference: *"This object has
been deprecated. Please use Bulk QR codes V2 under the QR codes section."* It is also restricted
to static codes. No sunset date is published and no `Sunset`/`Deprecation` header is sent, so the
only warning a client will ever get is this sentence — build against
**`/api/2.0/bulkqrcodesv2/`**.

## Steps

1. **Validate first.** `POST /api/2.0/bulkqrcodesv2/csv-validation/` with the CSV/XLSX file. Do
   not skip this — there is no idempotency key, so a rejected-then-retried generate can leave a
   partial batch behind, and validation is the only cheap way to find out before you commit.
2. **Generate.** `POST /api/2.0/bulkqrcodesv2/` with:
   - `qr_type` — `1` static, `2` dynamic (default `2`)
   - `qr_data_type` — the campaign type. Static: `1` URL, `2` SMS, `3` PHONE, `4` EMAIL,
     `5` VCARD, `6` PLAIN_TEXT. Dynamic: `7` URL, `8` SMS, `9` PHONE, `10` EMAIL, `11` APP_LINK,
     `12` VCARD_PLUS.
   - `attributes` — the same design object the single-create endpoint accepts; per-row attribute
     columns in the CSV override it, which is how one batch produces differently-styled codes.
   - `file` — the CSV/XLSX, in the layout of the template in the dashboard's BULK-UPLOAD section.
3. **Collect the output.** Bulk output is delivered through a `Media` object; the download is a
   zip of the rendered images (JPEG, PDF, PNG, SVG). List with `GET /api/2.0/media/` or fetch one
   with `GET /api/2.0/media/{id}/`.

## Rules and gotchas

- `qr_data_type` numbering is **different for static and dynamic**. `1` means URL for static and
  nothing for dynamic; dynamic URL is `7`. Getting this wrong produces a valid batch of the wrong
  kind of code, with no error.
- The CSV layout is not published as a schema — Uniqode distributes it as a downloadable template
  from the dashboard. Do not hand-build the header row from guesswork.
- Bulk generation is asynchronous in effect: poll the resulting object rather than expecting the
  images in the create response.
- No progress, failure-row, or partial-success payload is documented. If the batch matters,
  reconcile by listing `GET /api/2.0/qrcodes/` afterwards and counting.

## Related

- `lifecycle/beaconstac-lifecycle.yml` — the deprecated bulk resource and the absence of a sunset policy
- `data-model/beaconstac-data-model.yml` — BulkQRCodeV2, Media, and the S3 upload pattern
- `conventions/beaconstac-conventions.yml` — the two-step csv-validation → create pattern

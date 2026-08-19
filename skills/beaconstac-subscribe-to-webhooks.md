---
name: Subscribe to Uniqode events with a webhook
description: Register an organization webhook on Uniqode and select which QR Code, NFC Tag, Beacon, Geofence and Form events are delivered to it.
api: collections/beaconstac-uniqode-api.postman_collection.json
generated: '2026-08-13'
method: generated
source: collections/beaconstac-uniqode-api.postman_collection.json
operations:
  - POST /api/2.0/organizationwebhooks/
  - GET /api/2.0/organizationwebhooks/
  - GET /api/2.0/organizationwebhooks/{id}/
  - PUT /api/2.0/organizationwebhooks/{id}/
---

# Subscribe to Uniqode events with a webhook

Use this to react to scans and form submissions in real time instead of polling the reporting
endpoint. A Uniqode webhook is registered **per organization**: one destination URL plus a set of
per-object-type event selections on a single webhook object.

## Steps

1. **Register.** `POST /api/2.0/organizationwebhooks/` with `organization`, the destination `url`,
   and one or more event-group objects.
2. **Choose your events.** Only these exist:

   | Group | Subject | Events |
   |---|---|---|
   | `qr_code_events` | QR Code | `create`, `update`, `delete`, `view` |
   | `nfc_tag_events` | NFC Tag | `update`, `view` |
   | `beacon_events` | Beacon | `update`, `delete`, `view`, `notification_sent` |
   | `geofence_events` | Geofence | `create`, `update`, `view`, `notification_sent` |
   | `form_events` | Feedback Form | `form_response_submitted` |

   `view` is the scan/view event — for a QR Code, that is the one that fires on every scan and is
   therefore the highest-volume subscription by a wide margin. Subscribe to it deliberately.
3. **Verify the registration.** `GET /api/2.0/organizationwebhooks/` to list, or
   `GET /api/2.0/organizationwebhooks/{id}/` to read one back.
4. **Change it.** `PUT /api/2.0/organizationwebhooks/{id}/`.

## Rules and gotchas

- **There is no documented DELETE.** The published reference documents list, create, retrieve and
  update only. To stop delivery, `PUT` the webhook with its event selections emptied — do not
  assume `DELETE` on the resource works.
- **No payload schema is published.** Uniqode documents which events exist but not what the
  delivery body looks like. Write the receiver defensively: accept unknown fields, key off the
  event group and name, and log a rejected sample rather than hard-failing.
- **No signature or verification scheme is published.** There is no documented HMAC header or
  shared secret, so the endpoint cannot cryptographically prove a delivery came from Uniqode.
  Treat the URL itself as the only secret: use an unguessable path over HTTPS, and re-fetch
  anything consequential through the API rather than trusting the payload.
- **No retry or failure policy is published.** Assume at-most-once until proven otherwise;
  reconcile against the reporting API for anything that must not be missed. Also assume the
  opposite may be true — make the handler idempotent so a duplicate delivery is harmless.
- **Beacon, NFC Tag and Geofence events exist for products with no CRUD surface** in the published
  API. You can subscribe to their events but you cannot create or manage those objects over HTTP.

## Related

- `asyncapi/beaconstac-webhooks.yml` — the full captured event catalogue and the webhook object
- `data-model/beaconstac-data-model.yml` — the Webhook object and its organization scoping
- `conventions/beaconstac-conventions.yml` — tenancy, trailing slashes, error envelopes

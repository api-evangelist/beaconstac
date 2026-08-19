---
name: Issue Digital Business Cards for a team and collect leads
description: Bulk-create Uniqode Digital Business Cards for an organization, distribute their QR images and wallet passes, and pull the leads they capture.
api: collections/beaconstac-uniqode-api.postman_collection.json
generated: '2026-08-13'
method: generated
source: collections/beaconstac-uniqode-api.postman_collection.json
operations:
  - POST /api/2.0/dbc/csv-validation/
  - POST /api/2.0/dbc/bulkcreate/
  - GET /api/2.0/dbc/
  - GET /api/2.0/dbc/{id}/
  - PUT /api/2.0/dbc/{id}/
  - GET /api/2.0/dbc/{id}/download/
  - POST /api/2.0/dbc/{id}/walletpass/
  - GET /api/2.0/dbc/templates/
  - GET /api/2.0/lead/
  - POST /api/2.0/lead/export/
  - POST /api/2.0/dbc/bulkdelete/list/
---

# Issue Digital Business Cards for a team and collect leads

Use this for HR/onboarding and field-sales rollouts: provision a card per employee, hand each
person a QR image and a wallet pass, then harvest the contacts the cards capture.

## Every call in this flow is organization-scoped

Unlike the QR Code endpoints, the DBC endpoints in Uniqode's own published requests carry
`?organization={ORG_ID}` on **list, retrieve, create, update, delete, activate, download and
wallet pass**. Omitting it on a multi-org account is the most common failure in this flow.

## Steps

1. **Pick a template (optional).** `GET /api/2.0/dbc/templates/?organization={org}&page_size=50&page=1`.
   A card template is just a `DigitalBusinessCard` with `is_template: true`, so it carries the
   same fields — use one to keep branding consistent across the batch.
2. **Validate the roster.** `POST /api/2.0/dbc/csv-validation/` with the CSV/XLSX. Uniqode
   publishes CSV and XLSX data templates; use them rather than inventing a header row.
3. **Bulk create.** `POST /api/2.0/dbc/bulkcreate/`. For one-off cards use
   `POST /api/2.0/dbc/?organization={org}` instead, with at minimum `first_name`, `url` and
   `organization`; `customizations`, `social_links`, `phone_v2`, `email_v2`, `website_v2` and
   `logo_url`/`user_image_url` carry the branding.
4. **List what landed.** `GET /api/2.0/dbc/?organization={org}&page_size=10&page=1&state=A` —
   `state=A` is active, `state=S` is sleeping. Filter by owner with `card_owner__is={user_id}`.
5. **Distribute.** Per card: `GET /api/2.0/dbc/{id}/download/?canvas_type=png&size=1024&error_correction_level=2&organization={org}`.
   Supported types are `png`, `jpeg`, `svg`, `pdf`, `eps`; supported sizes are `512`, `1024`,
   `2048`, `4096` — other values are not supported. For a batch, use
   `POST /api/2.0/dbc/qrcode/list/?organization={org}` (explicit ids) or
   `POST /api/2.0/dbc/qrcode/filter/?organization={org}` (a filter).
6. **Wallet passes.** `POST /api/2.0/dbc/{id}/walletpass/?organization={org}` creates an Apple or
   Google Wallet pass; `GET` on the same path lists the passes already issued for the card.
7. **Harvest leads.** `GET /api/2.0/lead/` returns the contacts captured through two-way sharing
   and forms — `first_name`, `last_name`, `email`, `company`, `phone`, `designation`, `notes`,
   `dbc` (the card that captured them), `lead_owner`, `creation_source`, `location`, `source_url`,
   `created`. For a file, `POST /api/2.0/lead/export/`.

## Rules and gotchas

- **Offboarding: deactivate, don't delete.** `DELETE /api/2.0/dbc/{id}/activate/?organization={org}`
  moves the card to sleeping, and its URL and QR then land on a "No Digital Business Card found"
  page. `DELETE /api/2.0/dbc/{id}/` destroys the card and the printed/shared QR dies with it.
  `POST /api/2.0/dbc/bulkdelete/list/` is a **hard** bulk delete — there is no bulk deactivate.
- **Leads are personal data.** `GET /api/2.0/lead/` returns names, emails and phone numbers of
  people who are not your users. Uniqode is SOC 2 Type II / ISO 27001:2022 / HIPAA / GDPR
  compliant, but that is Uniqode's posture, not yours — do not pull, cache or forward lead
  records without a lawful basis, and never write them into logs or agent transcripts.
  There is **no** delete-lead bulk endpoint; erasure is one-at-a-time via
  `DELETE /api/2.0/lead/{id}/`.
- No idempotency key exists. A retried `bulkcreate` issues a **second** card to every person in
  the file. Reconcile with a list call before retrying.
- Cards go dark if the account lapses, exactly like QR Codes.

## Related

- `data-model/beaconstac-data-model.yml` — DigitalBusinessCard, Lead, and their relationships
- `conventions/beaconstac-conventions.yml` — the activate/deactivate soft-delete pattern
- `security/beaconstac-trust-center.yml` — the compliance posture and the subprocessor list

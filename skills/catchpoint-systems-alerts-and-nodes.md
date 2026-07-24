---
name: Review Catchpoint alerts and monitoring nodes
description: Read alert state, manage alert settings, and enumerate the Catchpoint monitoring node network via the REST API v2.
api: openapi/catchpoint-systems-rest-api-v2-openapi.json
operations:
- GET /v2/tests/alerts
- PATCH /v2/tests/alerts
- GET /v2/nodes
- GET /v2/nodes/all
- GET /v2/iw/outages
---

# Review Catchpoint alerts and monitoring nodes

Base host `https://io.catchpoint.com/api`; bearer (JWT) auth in the `Authorization` header.

## Steps
1. **List active alerts** — `GET /v2/tests/alerts` (optionally `GET /v2/tests/alerts/{alertIds}` for
   specific ids). Page with `pageNumber`/`pageSize`.
2. **Update alert settings** — `PATCH /v2/tests/alerts` to adjust alert configuration.
3. **Enumerate the node network** — `GET /v2/nodes` (or `GET /v2/nodes/all`) to list the monitoring
   nodes available to your account; `GET /v2/nodes/state/{nodeIds}` for node health.
4. **Correlate with internet outages** — `GET /v2/iw/outages` returns Internet Weather / Internet Sonar
   outage signals to contextualize alert spikes.

## Stream events instead of polling
For push delivery, configure Alert Webhooks and Test Data Webhooks (see
`asyncapi/catchpoint-systems-webhooks.yml`) rather than polling these endpoints.

## Conventions & errors
- Rate limits: 7/sec, 20/min, 500/hr, 2000/day per requestor IP — honor `429`.
- Errors are ErrorCode objects; `401`/`403` indicate auth/permission problems. See
  `errors/catchpoint-systems-problem-types.yml`.

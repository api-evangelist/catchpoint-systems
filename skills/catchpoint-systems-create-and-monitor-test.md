---
name: Create and monitor a Catchpoint synthetic test
description: Authenticate, create a synthetic test, list/read it, and pull its performance results using the Catchpoint REST API v2.
api: openapi/catchpoint-systems-rest-api-v2-openapi.json
operations:
- POST /v2/tests
- GET /v2/tests
- GET /v2/tests/{testIds}
- GET /v2/tests/records/waterfall/{testId}
- GET /v2/tests/errors/raw/{testIds}
---

# Create and monitor a Catchpoint synthetic test

Catchpoint REST API v2 base host: `https://io.catchpoint.com/api`. The spec's server value is the
relative path `/api`.

## Auth
Obtain a bearer (JWT) token from Catchpoint API credentials (REST API key/secret generated in the
Catchpoint Portal). Send it on every request:

```
Authorization: Bearer <token>
```

## Steps
1. **Create the test** — `POST /v2/tests` with the test definition (monitor type, URL, node group,
   frequency). Capture the returned test id.
2. **Confirm it exists** — `GET /v2/tests` (paged with `pageNumber`/`pageSize`) or
   `GET /v2/tests/{testIds}` for the specific id.
3. **Pull results** — once runs accrue, retrieve waterfall/performance records with
   `GET /v2/tests/records/waterfall/{testId}`.
4. **Inspect failures** — `GET /v2/tests/errors/raw/{testIds}` returns raw recent errors for the test.

## Conventions & errors
- Pagination: `pageNumber` + `pageSize` query params on list endpoints.
- Rate limits: 7/sec, 20/min, 500/hr, 2000/day per requestor IP — back off on `429`.
- Errors come back as ErrorCode objects (`id`, `name`, `message`, `isWarning`); `401` = bad/missing
  token, `403` = insufficient permission. See `errors/catchpoint-systems-problem-types.yml`.

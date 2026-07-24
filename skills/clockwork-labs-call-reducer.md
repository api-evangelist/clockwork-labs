---
name: Authenticate and invoke a reducer
description: Mint an identity, invoke a reducer to mutate state, and read the logs to confirm.
api: SpacetimeDB HTTP API
base_url: https://maincloud.spacetimedb.com
operations:
  - POST /v1/identity
  - POST /v1/database/:name_or_identity/call/:reducer
  - GET /v1/database/:name_or_identity/logs
docs: https://spacetimedb.com/docs/http/database/
---

# Authenticate and invoke a reducer

Reducers are the only way to mutate a SpacetimeDB database. Each reducer runs inside a single
database transaction (all-or-nothing atomicity). Use this to call one over HTTP.

## Steps

1. **Get an identity + token (if you don't already have one).**
   `POST /v1/identity` returns a new Spacetime identity and a JWT token. If you use an external OIDC
   provider (SpacetimeAuth, Auth0, Clerk), use that ID token instead. Send it as
   `Authorization: Bearer <token>`.

2. **Invoke the reducer.**
   `POST /v1/database/:name_or_identity/call/:reducer` with the reducer arguments as the request body
   (JSON or BSATN). A `200` means the transaction committed. There is no idempotency-key header —
   atomicity is transactional, so a call either fully applies or fully fails; do not assume retries
   are automatically de-duplicated.

3. **Confirm via logs.**
   `GET /v1/database/:name_or_identity/logs` (requires a Bearer token) returns module logs, including
   anything the reducer logged — useful to confirm the effect or diagnose a failure.

## Rules
- `401` from the call means the token is missing or invalid; re-mint via `POST /v1/identity` or refresh
  your OIDC token. See `errors/clockwork-labs-problem-types.yml`.
- When an `Authorization` header is sent, the response echoes `spacetime-identity` and
  `spacetime-identity-token` headers identifying the caller.
- To observe the resulting state change in real time, subscribe over the WebSocket
  (`GET /v1/database/:name_or_identity/subscribe`) rather than polling.

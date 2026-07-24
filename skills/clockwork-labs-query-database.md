---
name: Query a SpacetimeDB database over HTTP
description: Inspect a SpacetimeDB database's schema and read rows with SQL over the HTTP API.
api: SpacetimeDB HTTP API
base_url: https://maincloud.spacetimedb.com
operations:
  - GET /v1/database/:name_or_identity/schema
  - POST /v1/database/:name_or_identity/sql
  - GET /v1/database/:name_or_identity
docs: https://spacetimedb.com/docs/http/database/
---

# Query a SpacetimeDB database over HTTP

Use this to read data from a published SpacetimeDB database without an SDK. Base host is the hosted
SpacetimeDB Cloud (`https://maincloud.spacetimedb.com`) or any self-hosted `spacetime start` instance.

## Auth
A Bearer JWT is optional for public reads and required for private databases. Pass it as
`Authorization: Bearer <token>`. Mint an anonymous token with `POST /v1/identity` if you have no
external OIDC token. See `authentication/clockwork-labs-authentication.yml`.

## Steps

1. **Confirm the database exists.**
   `GET /v1/database/:name_or_identity` returns the database identity, owner, host type, and replica
   count. A 200 confirms the name (or identity) resolves.

2. **Fetch the schema.**
   `GET /v1/database/:name_or_identity/schema` returns the tables, types, and reducers. Use it to
   discover table names and columns before writing SQL.

3. **Run a read query.**
   `POST /v1/database/:name_or_identity/sql` with the SQL text as the body. Express paging inside the
   SQL (`LIMIT`/`OFFSET`/`WHERE`) — there is no generic pagination envelope. Results are returned as
   rows (JSON or BSATN).

## Rules
- Errors are HTTP status codes, not `problem+json`: `401` = missing/invalid token on a private
  database. See `errors/clockwork-labs-problem-types.yml`.
- SQL over HTTP is read-oriented; all state mutations must go through reducers (see the
  "Authenticate and invoke a reducer" skill).
- For continuous/real-time reads, open the subscription WebSocket
  (`GET /v1/database/:name_or_identity/subscribe`) instead of polling SQL.

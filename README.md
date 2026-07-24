# Clockwork Labs

Clockwork Labs is a San Francisco software company (founded 2019) that builds **SpacetimeDB**, a
relational database that is also an application server: developers upload their schema and
server-side logic as a WebAssembly module (Rust, C#, TypeScript, or C++) directly into the database,
and clients connect over WebSocket to invoke reducers and subscribe to real-time state updates with
no separate application server in between. It is the production backend of the MMO BitCraft.

SpacetimeDB exposes a versioned HTTP API (`/v1/database` and `/v1/identity`), a first-party
`spacetime` CLI, and client SDKs for TypeScript, C#/Unity, Rust, Python, and C++.

- Website: https://spacetimedb.com
- Docs: https://spacetimedb.com/docs
- HTTP API reference: https://spacetimedb.com/docs/http/database/
- GitHub: https://github.com/clockworklabs
- Status: https://status.spacetimedb.com

Backed by: a16z (plus Supercell, Firstminute Capital, Skycatcher, 1Up Ventures, Supernode).

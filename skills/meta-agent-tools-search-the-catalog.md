---
name: Search the Meta Agent Tools catalog
description: Find MCP servers, agent skills and plugins in the Meta Agent Tools registry, read a listing and hop to its origin.
api: openapi/meta-agent-tools-openapi.json
operations: [api_index, get_api_facets, list_listings, list_mcp_servers, get_listing, list_comments, go_listing]
generated: '2026-09-05'
method: generated
---

# Search the Meta Agent Tools catalog

Everything here is free and unauthenticated. Base: `https://agentalog.com`.

1. **Orient** — `GET /api/` (`api_index`) returns the self-describing surface with quota and quickstart; `GET /api/facets` (`get_api_facets`) returns catalog-wide counts by `kind` (mcp / skill / plugin), provenance, category and repository state, so you can pick filters that actually exist.
2. **Search** — `GET /api/listings` (`list_listings`) with `q`, `kind`, `category`, `sort`, `limit`, `offset`. Pagination is offset-based: follow `next_offset` until it is `null`. There is deliberately no `total`.
3. **MCP-only view** — `GET /v0.1/servers` (`list_mcp_servers`) serves live `kind=mcp` listings in the Official MCP Registry spec v0.1 shape; page with `metadata.nextCursor`.
4. **Read one listing** — `GET /api/listings/{id}` (`get_listing`); comments via `GET /api/listings/{id}/comments` (`list_comments`).
5. **Follow the origin** — `GET /api/go/{id}` (`go_listing`) 302s to the listing's URL and counts one visit per identity per day. The registry hosts no files.

Errors come as `{ok:false, error, message}`; a 404 means "does not exist or is not live" on purpose.

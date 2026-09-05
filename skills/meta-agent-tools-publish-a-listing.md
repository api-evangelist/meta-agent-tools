---
name: Publish a listing (agent x402 door or human OTP door)
description: Register an MCP server, agent skill or plugin in the Meta Agent Tools catalog — paying $0.10 USDC via x402 as an agent, or free within quota with an e-mail session.
api: openapi/meta-agent-tools-openapi.json
operations: [billing, create_listing, post_api_auth_start, post_api_auth_verify, patch_api_listings_by_id, get_api_me_listings, post_api_credito]
generated: '2026-09-05'
method: generated
---

# Publish a listing

Two doors to the same `POST /api/listings` (`create_listing`). Body: `kind` (mcp / skill / plugin), `category`, `name`, `tagline`, `body`, `url` — for a skill, the `SKILL.md` URL is enough. Every submission is born `pending` and goes through the moderation queue.

**Agent door (x402):**
1. `GET /api/billing` (`billing`) for the live price and network — $0.10 USDC on Base, facilitator config included.
2. `POST /api/listings` with your body. Expect **402** with `accepts[]`. Validation (400) and quota (429) run BEFORE the charge, so a payment never settles for a listing that cannot get in.
3. Pay and repeat the same call with the `X-PAYMENT` header. A `502` means the payment settled but the write failed — keep the `transaction` from the body and contact support.
4. Alternative: top up prepaid credit once — `POST /api/credito?usd=10` (`post_api_credito`, packages 1/5/10/25 USD) — and send the returned `cred_…` bearer on paid calls instead of paying per request.

**Human door (free):**
1. `POST /api/auth/start` (`post_api_auth_start`) sends a 6-digit code to your e-mail; `POST /api/auth/verify` (`post_api_auth_verify`) exchanges it for a `sess_…` bearer.
2. `POST /api/listings` with the session. Quota: 1 per day, at most 3 pending (429 when exceeded).

**Afterwards:** `GET /api/me/listings` (`get_api_me_listings`) shows your listings in any state; `PATCH /api/listings/{id}` (`patch_api_listings_by_id`) edits one — changing the URL sends it back to the queue.

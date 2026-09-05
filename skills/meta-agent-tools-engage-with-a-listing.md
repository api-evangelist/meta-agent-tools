---
name: Like, comment and visit as a guest
description: Engage with Meta Agent Tools listings using a free guest identity — reversible likes, capped comments, counted visits.
api: openapi/meta-agent-tools-openapi.json
operations: [create_guest, like_listing, delete_api_listings_by_id_like, post_api_listings_by_id_comments, delete_api_comments_by_id, go_listing]
generated: '2026-09-05'
method: generated
---

# Like, comment and visit as a guest

1. **Get an identity** — `POST /api/guest` (`create_guest`) returns an `mr_…` token. Send it as `X-Guest-Token` or `Authorization: Bearer mr_…`. It is free.
2. **Like** — `POST /api/listings/{id}/like` (`like_listing`). Idempotent by design: calling again does not add up — the counter counts people. Reverse with `DELETE /api/listings/{id}/like` (`delete_api_listings_by_id_like`), which gives the point back.
3. **Comment** — `POST /api/listings/{id}/comments` (`post_api_listings_by_id_comments`). Cap: 20 per hour per owner (429 beyond). Delete your own with `DELETE /api/comments/{id}` (`delete_api_comments_by_id`) — someone else's comment answers 404, not 403.
4. **Visit** — `GET /api/go/{id}` (`go_listing`) counts one visit per identity per day and redirects to the listing's origin.
5. **Keep your history** — if you later create an account, `POST /api/auth/claim` ties the guest to it and its likes/comments become the account's.

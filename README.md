# sweetrpg-data-api

API microservice composite for persistence layer

## Rate limiting

This service (legacy Swift 4.1, SQL-backed) has **no per-client/IP rate limiting**, and none
was added by the platform-wide rate-limiting standardization (`sweetrpg/platform` OpenSpec
change `fix-rate-limiting-per-client-ip`). It is a documented known gap.

When `kv-api` is migrated to the Go service baseline (or rewritten), it MUST adopt the shared
`api-core.go/ratelimit` middleware from day one: Redis-backed per-client counters keyed by
`X-API-Key` else client IP, the `cheap` (`/status/*`) and `standard` route tiers driven by
`RATE_LIMIT_CHEAP` / `RATE_LIMIT_CHEAP_WINDOW_SECONDS` / `RATE_LIMIT_STANDARD` /
`RATE_LIMIT_STANDARD_WINDOW_SECONDS`, fail-closed 503 when the store is unreachable, and a 429
with the standard error body on exceed. See `docs/go-service-conventions.md` "Rate limiting" in
the platform repo.

# Cloudflare Workers & D1 Stack Template

## Detected Technologies

- **Runtime:** Cloudflare Workers (V8 isolates edge runtime)
- **Database:** Cloudflare D1 (Serverless SQLite at the edge)
- **Configuration:** `wrangler.jsonc`, `wrangler.json`, or `wrangler.toml`
- **Tooling:** Wrangler CLI (`wrangler`), `@cloudflare/vite-plugin`
- **Security & Bot Protection:** Cloudflare Turnstile, Cloudflare WAF, Play Integrity
- **Storage/KV:** Workers KV, R2, D1

## Stack Detection Signals

```bash
rg --files -g 'wrangler.json*' -g 'wrangler.toml' -g 'workers/**' -g 'migrations/*.sql'
rg -n 'D1Database|compatibility_date|nodejs_compat|cf-typegen' package.json wrangler.* 2>/dev/null
```

## Common Stack Patterns & Anti-Patterns

### Cloudflare D1 & SQLite Optimization
- **Batching:** Always use `db.batch([stmt1, stmt2, ...])` for independent queries within a request to execute in a single round-trip.
- **Statement Limits:** D1 enforces a hard limit of 100 statements per `db.batch()` call. Slicing bulk operations into chunks of 100 statements is mandatory.
- **Index Coverage:** Frequently filtered columns (e.g. phone numbers, timestamps, status flags, user/voter hashes) must have compound or single indexes in `migrations/*.sql`.
- **Prepared Statements:** Always use parameter binding (`stmt.bind(...)`) rather than SQL string interpolation to prevent SQL injection and allow SQLite query plan caching.
- **Migration Application:** Every schema or index change in `migrations/*.sql` must be applied remotely (`wrangler d1 migrations apply DB --remote`) before deployment.

### Workers Edge Caching & CPU Limits
- **Cache-Control Headers:** Implement precise TTL policies in the worker entry (`workers/app.ts`). Distinguish live real-time pages (short cache, e.g. 120s) from static documentation or directory pages (longer cache).
- **No Stale-While-Revalidate on Critical Data:** Never advertise `stale-while-revalidate` on real-time reputation, abuse, or voting endpoints.
- **Isolate CPU Limits:** Workers have strict CPU time budgets per request. Offload heavy processing to client or batch cron workers. Avoid blocking crypto operations; use asynchronous Web Crypto API (`crypto.subtle`).

### Bot Defense & Privacy
- **Turnstile Verification:** Verify browser write tokens server-side using idempotent verification checks.
- **IP Privacy:** Never log or store raw IP addresses in D1. The trusted client IP chain must be `CF-Connecting-IP` → `X-Real-IP` → `0.0.0.0`. Never trust `X-Forwarded-For`. Always salt and hash client identifiers (`FINGERPRINT_SALT`).
- **Timing Attacks:** Use `crypto.subtle.timingSafeEqual` for all API key, token, or hash comparisons.

## Industry Standards (verified 2026-10 via Context7)

Sources: `developers.cloudflare.com/workers/`, `developers.cloudflare.com/d1/`, `developers.cloudflare.com/turnstile/`

- **D1 Database**: Cloudflare D1 executes SQLite queries globally with read replication. Batched statements execute in a single round trip via `db.batch()`. Max batch limit is 100 statements.
- **Wrangler Types**: Generate accurate TypeScript environment definitions using `wrangler types` (`cf-typegen`).
- **Web Crypto**: Use native `crypto.subtle.digest('SHA-256', ...)` and `crypto.subtle.timingSafeEqual` for constant-time comparisons.
- **Turnstile**: Verify site tokens against `https://challenges.cloudflare.com/turnstile/v0/siteverify` using `POST` with `secret` and `response`.

## Agent Mapping

| Owner | Focus |
|---|---|
| `bolt` | D1 query batching, index coverage, Worker edge caching TTLs, CPU execution time |
| `sentinel` | Turnstile verification, IP hashing, rate limiting, mobile API authentication, CSP nonces |
| `database` | D1 schema migrations, SQLite index optimization, transaction boundaries, batch limits |
| `custodian` | Dead route handlers, unused D1 queries, obsolete environment variables |
| `testing` | Vitest with mock D1 database bindings, security header tests, mobile API tests |
| `shtef` | Wrangler CLI and Cloudflare Vite plugin updates, dependency audits |

## Verification Commands

```bash
pnpm run typecheck    # (or npm run cf-typegen && tsc)
pnpm run build
pnpm exec vitest run app/lib/security-headers.test.ts
pnpm exec vitest run
```

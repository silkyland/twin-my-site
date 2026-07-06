# API Audit & Contract Guide

The app is only as good as the API feeding it. Audit what exists, design
what's missing, and remember: mobile clients update slowly — a breaking
API change costs a store-review cycle on every installed device.

## Part 1 — Audit the existing surface

- **Endpoint census:** existing JSON/REST/GraphQL endpoints with
  `file:line`. Server-rendered-only (HTML controllers, no API) is the
  common finding for classic web apps — then the gap is the whole contract,
  say so plainly.
- **Auth model:** session cookies + CSRF is the web default and does not
  transfer to mobile. Identify the current model (`file:line`) and what
  user store it sits on — the mobile flow builds on the same identities.
- **Render coupling:** data assembled inside templates (query in the view,
  HTML fragments over AJAX) has no reusable shape — those features need
  real endpoints, not scraping of their own site.
- **Reusable domain layer:** services/models the new endpoints can call
  directly (`file:line`) — the audit's good news section; API gaps are
  cheaper when the domain logic already exists.

## Part 2 — Contract rules

Design API-first: the app is client #1, not the only client ever.

- **Auth:** token-based — short-lived access token + refresh token
  (OAuth2/PKCE if third-party logins are in the census; otherwise a
  first-party token endpoint is fine). Plan token revocation (stolen
  device) and the account-deletion endpoint (store requirement).
- **Versioning:** `/api/v1/...` from day one; additive changes preferred;
  breaking changes get a new version and a deprecation window measured in
  months (installed clients lag).
- **Pagination:** cursor-based for feeds/lists (offset breaks when content
  shifts under an infinite scroll).
- **Media:** mobile-sized variants (widths, formats) via CDN params or
  variant fields — shipping desktop hero images to a phone on cellular is
  a product bug. For content apps this is the top bandwidth line item.
- **Offline-friendly:** `ETag`/`If-None-Match` or `updated_at` filtering
  so the app can sync cheaply; list endpoints include the fields the app
  caches for offline reading.
- **Push pipeline (server side):** device-token registry (register/rotate/
  unregister endpoints), topic model (e.g. `new-issue`, `saved-author`),
  and the trigger points in existing web code where events originate
  (`file:line` — e.g. the publish action). Verify current APNs/FCM
  requirements from official docs (cite URL + date).
- **Error shape:** one consistent JSON error envelope; mobile UIs render
  errors, they don't follow redirects to login pages.
- **Rate limits & abuse:** the API is now a public surface; state limits
  and where they're enforced.

## Part 3 — Contract output format

For each resource, a compact table the blueprint embeds:

```markdown
### Resource: articles
| Endpoint | Method | Auth | Notes | Status |
|----------|--------|------|-------|--------|
| /api/v1/articles?cursor= | GET | optional | cursor, `fields` for list cards | MISSING — build on ArticleRepository (src/.../ArticleRepository.php:40) |
| /api/v1/articles/{id} | GET | optional | full body + media variants | MISSING |
| /api/v1/articles/{id}/bookmark | POST | required | | MISSING |
```

Every MISSING row is server-side roadmap work with its reuse pointer
(`file:line` of the domain code it wraps). No invented "probably exists"
endpoints — the audit either found it or it's MISSING.

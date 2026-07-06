---
name: twin-my-site
description: >-
  Plans a native mobile companion app for an existing website — a twin, not
  a wrapper: same brand DNA, data, and features; native body. Censuses the
  web product's features and classifies each for the app, audits API
  readiness and designs the mobile API contract (token auth, pagination,
  media, push pipeline), extracts the web's design language into mobile
  design tokens translated to native idioms, commits to an app stack,
  prepares the store listing (app name, description, category, privacy
  policy and account-deletion links — recommending generators when missing —
  and a scaffolded store/ publish kit), and writes a phased blueprint
  starting with a walking skeleton. Use when the user has a website and
  wants a mobile app version or companion app, needs an API designed for an
  app, wants the app to match the web's design and tone, asks to prepare an
  App Store or Play Store listing, or mentions twin-my-site or
  /twin-my-site.
license: MIT
argument-hint: "[web-project-root] [driver] [platforms: ios/android/both]"
---

# Twin My Site

The website exists; the app doesn't. The wrong move is a WebView wrapper
(rejected by Apple as minimum-functionality, and it feels like a website).
The right move is a **twin**: an app that shares the web product's DNA —
its data, its features, its design language — but lives in a native body
with native idioms. This skill plans that twin, evidence-first.

Division of labor: this skill produces the blueprint (feature matrix, API
contract, design translation, stack decision, roadmap). Each roadmap phase
is then one **deep-plan** run for implementation detail.

## The Prime Directive (family rule)

> **The web product is the source of truth — and every claim about it needs
> `file:line`.** Features come from routes and code, not from memory of the
> homepage. Design tokens come from the actual CSS. Platform claims (APNs,
> FCM, store rules, framework capabilities) need a docs citation fetched
> now — store policies change quarterly and training-data memory of them is
> a rejection waiting to happen.

## Progress checklist

Copy this into your response and check items off:

```
Twin Progress:
- [ ] Step 0: Frame — driver, platforms, scope, team constraints
- [ ] Step 1: Feature census — every web feature classified for the app
- [ ] Step 2: API audit & contract — existing endpoints, gaps, mobile contract designed
- [ ] Step 3: Design language extraction — web tokens → mobile tokens, translated not transplanted
- [ ] Step 4: Stack decision — one committed choice with rationale
- [ ] Step 5: Mobile-native layer — offline, push, deep links, store requirements
- [ ] Step 6: Store listing & publish kit — name, description, category, legal links, store/ scaffolded
- [ ] Step 7: Blueprint document written; Phase 1 = walking skeleton
- [ ] Step 8: Self-check + report
```

## Step 0 — Frame

- **Driver:** why an app at all — push re-engagement, offline reading,
  store presence, home-screen habit? The feature scope and stack choice
  both hang on this.
- **Platforms:** iOS / Android / both. **Scope:** full parity or a subset
  (for content products the usual right answer is: reader app first,
  admin/publishing stays web — recommend it).
- **Team constraints:** who maintains the app? Their existing skills weigh
  heavily in Step 4 — an app the team can't maintain is a liability with
  an icon.

## Step 1 — Feature census

Inventory the web product's user-facing features from evidence: routes,
controllers, navigation menus, templates (`file:line` each — not from
memory of using the site). Classify every feature for the app:

| Verdict | Meaning |
|---------|---------|
| **include** | Goes to the app as-is (conceptually) |
| **adapt** | Goes, but mobile-reshaped (e.g. mega-menu → tab + search) |
| **exclude** | Stays web-only (admin, desktop workflows) — with the reason |
| **mobile-new** | Doesn't exist on web; the app's reason to exist (push, offline, share sheet, widgets) |

See [references/feature-census.md](references/feature-census.md) for the
method. Output: the feature matrix — it drives the API contract (Step 2)
and the roadmap (Step 6).

## Step 2 — API audit and contract

The make-or-break step. Audit what exists, then design what the app needs —
rules and template in [references/api-contract-guide.md](references/api-contract-guide.md):

1. **Audit:** existing endpoints (REST? GraphQL? none — server-rendered
   HTML only?), auth model (session cookies do not fly on mobile), data
   shapes coupled to HTML rendering. Cite `file:line` per finding.
2. **Gap list:** every `include`/`adapt` feature that has no consumable
   endpoint — this is server-side work the web team must build, and it goes
   on the roadmap explicitly, usually as the longest pole.
3. **Contract:** design API-first (the app is client #1, not the only
   client ever): token auth with refresh, versioned paths, cursor
   pagination for feeds, mobile-sized media variants, ETags for offline
   sync, a push pipeline (device token registry + topics).

## Step 3 — Design language extraction

The "same tone" requirement, done with evidence — method in
[references/design-translation.md](references/design-translation.md):

1. **Extract** from the actual web codebase: color tokens, typography,
   spacing scale, radii, iconography, component inventory, voice of copy
   (`file:line` — CSS variables, theme files, design tokens).
2. **Translate, don't transplant:** same brand tokens, native interaction
   idioms — navbar → tab bar, hover → press states, modals → sheets,
   breadcrumbs → navigation stack. A web layout cloned onto a phone reads
   as a website in a costume; the twin must feel native while unmistakably
   the same brand.
3. Output: mobile token set + a component mapping table (web component →
   app component, per-platform notes), plus the decisions the web can't
   answer (dark mode if the web has none, font licensing for app embedding).

## Step 4 — Stack decision

Commit to ONE app stack — native (Swift/Kotlin), Flutter, or React
Native/Expo — weighing: team skills (Step 0), web-stack affinity (a React
web team gets type/logic reuse from RN/Expo — verify the specific sharing
claim, don't assume), feature needs from the census (heavy native features
pull toward native), and hiring/maintenance reality. Cite docs for any
capability the decision leans on. One decision with rationale and rejected
alternatives in one line each — no menus.

## Step 5 — Mobile-native layer

Design the parts that make it an app rather than a port:

- **Offline:** what is readable without network (for content products:
  downloaded issues/articles + images), the cache/sync strategy (ETags,
  timestamps), and the honest storage budget.
- **Push:** the full pipeline — server event → device tokens → topic
  routing → deep link into the right screen. Server side included.
- **Deep links:** universal links/app links so web URLs open the app on
  the same content — the twin behaves as one product with the web.
- **Store requirements:** verify the CURRENT store rules that apply
  (fetch now, cite URL + date): Apple minimum functionality (4.2), IAP
  rules if any paid content, account deletion requirement, privacy
  labels/data-safety forms. Any rule that reshapes scope goes to the user
  immediately, not into a footnote.

## Step 6 — Store listing and publish kit

Prepare the launch surface now, from evidence — full method in
[references/store-listing-guide.md](references/store-listing-guide.md):

- **Listing text:** app name candidates (from the web brand, cited),
  subtitle/short description (from the `mobile-new` value), long
  description (hook from the site's own copy; bullets from the census),
  keywords, localized to the site's language(s). Character limits verified
  from current store docs — never from memory.
- **Category:** primary + secondary per store, chosen from the CURRENT
  category lists (fetched, cited) with one-line rationale.
- **Privacy policy link:** audit the site for an existing policy
  (`file:line`); if present, list the app-specific coverage gaps (push
  tokens, device IDs, SDKs); if absent, recommend generators (iubenda,
  Termly, GetTerms, …) and host the result at the canonical domain
  (`https://<site>/privacy`).
- **Account deletion link:** fetch current Apple/Google requirements, then
  recommend one canonical web route (`https://<site>/account/delete`)
  reusing web auth, paired with the contract's deletion endpoint.
- **Scaffold `store/`** in the repo — listing/, legal/ (privacy,
  account-deletion, data-safety worksheet mapped from the API contract),
  assets/ (icon/screenshot/feature-graphic specs with citations), and a
  verifiable pre-submission checklist. Every file drafted with real
  content from this run; no empty placeholders.

## Step 7 — Blueprint document

Write `docs/TWIN_BLUEPRINT.md` per
[references/blueprint-template.md](references/blueprint-template.md).
The roadmap's **Phase 1 is a walking skeleton**: real auth + one content
list + one detail screen, end-to-end through the real API on a real device
— it validates the API contract, the design tokens, and the stack choice
at once, before scale-out. Every phase sized for one deep-plan run.

## Step 8 — Self-check and report

- Every feature verdict traces to a route/controller citation; every API
  gap names the server-side work; every design token has a CSS source;
  every store-rule claim, character limit, and category has a current
  citation fetched this run.
- `store/` exists with drafted content in every file — zero empty
  placeholders — and the privacy/deletion links are either live URLs or
  explicit roadmap items.
- The user could hand Phase 1 to deep-plan today.
- Report: scope (include/adapt/exclude/mobile-new counts), the API gap
  list (the real cost center), stack decision in one line, the recommended
  app name + category, legal-link status, and the top 3 risks.

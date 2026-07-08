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

**No web access in this session?** Degrade honestly: extract everything
local evidence allows (routes, CSS, code), tag every store rule, character
limit, and platform claim `UNVERIFIED — needs web check`, and mark the
store-compliance and listing sections provisional. Never fill them from
memory.

**No codebase, only a live site?** The census and design extraction can
run against the deployed product instead: crawl the navigation, sitemap,
and rendered pages; extract computed styles for tokens. Evidence citations
become URLs instead of `file:line`, the API audit is limited to what the
browser's network traffic reveals, and every gap inherits `UNVERIFIED —
needs source access`. State this ceiling in the report — a blueprint from
the outside is a weaker blueprint.

## Progress checklist

Copy this into your response and check items off:

```
Twin Progress:
- [ ] Step 0: Frame — driver, platforms, scope, team constraints, Twin Questions
- [ ] Step 1: Feature census — every web feature classified for the app
- [ ] Step 2: API audit & contract — existing endpoints, gaps, mobile contract designed
- [ ] Step 3: Design language extraction — web tokens → mobile tokens, translated not transplanted
- [ ] Step 4: Stack decision — one committed choice with rationale, reversibility-tagged
- [ ] Step 5: Mobile-native layer — offline, push, deep links, store requirements
- [ ] Twin Brief Gate — 10–20 line brief confirmed; every ONE-WAY decision resolved
- [ ] Step 6: Store listing & publish kit — name, description, category, legal links, store/ scaffolded
- [ ] Step 7: Blueprint document written; Phase 1 = walking skeleton, spikes placed first
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
- **Monetization:** free / ads / subscriptions / paid content. This decides
  whether IAP rules apply (Step 5) and shapes the listing (Step 6) — pin it
  now, not at submission time.
- **Twin Questions:** write a numbered list of the questions the blueprint
  needs answered (does a consumable API exist? what auth model? do the
  fonts license for app embedding? where in the code do push-worthy events
  originate? which current store rules touch this product's monetization?).
  Steps 1–5 exist to answer this list — each finding cites the question it
  answers, questions discovered mid-run are appended, and research ends
  when every question is answered with evidence or tagged `UNVERIFIED` —
  **not** when every file has been read.
- If a required input is genuinely ambiguous, ask **one question at a
  time** with your recommended answer and a one-line reason attached —
  never a wall of questions.

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
and the roadmap (Step 7).

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

Tag every committed decision **REVERSIBLE** (cheap to undo later) or
**ONE-WAY** (undo cost rivals do cost). Three decisions in this skill are
always ONE-WAY: the **app stack** (a swap after the skeleton exists is a
rewrite), the **API contract v1** (installed clients can't be forced to
update), and the **app name + bundle id** (permanent after first store
submission). ONE-WAY decisions are confirmed at the Twin Brief Gate below —
never silently committed.

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

## Twin Brief Gate — before the expensive artifacts

Before scaffolding `store/` (Step 6) or writing the blueprint (Step 7),
present a compact brief in chat — 10–20 lines total, this is the brief,
not the document:

- Scope verdict + census counts (include / adapt / exclude / mobile-new).
- The stack decision, one-line rationale.
- The top 3 API gaps (the real cost center) and who builds them.
- Any store rule that reshapes scope.
- The recommended app name + bundle id (deriving candidates is cheap;
  writing the kit around the wrong name is not).
- Every **ONE-WAY** decision flagged as such.

Ask for confirmation **once**. Changing a decision here costs one message;
changing it after Step 7 costs a rewrite. If the user cannot respond
(headless run), proceed on REVERSIBLE decisions and carry every ONE-WAY
decision into the blueprint explicitly marked `UNCONFIRMED`.

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

Two more roadmap non-negotiables:

- Every `UNVERIFIED` that survives into the blueprint becomes a named,
  timeboxed **spike task** placed FIRST in the roadmap phase that depends
  on it — "prove token auth works from the native client", "push reaches
  a test device end-to-end", "font license covers app embedding". No
  unknown sits silently under an implementation task.
- The risk register comes from a **pre-mortem**: assume the app shipped
  and failed three months later — rejected in review, uninstalled within
  a week, broken on installed devices by an API change — and write the
  top three causes. Those are the risks, each with a mitigation. Generic
  risks that fit any app ("store review might fail") are a self-check
  failure.

## Step 8 — Self-check and report

- Every feature verdict traces to a route/controller citation; every API
  gap names the server-side work; every design token has a CSS source;
  every store-rule claim, character limit, and category has a current
  citation fetched this run.
- Every Twin Question from Step 0 is answered with evidence or tagged
  `UNVERIFIED` — and every surviving `UNVERIFIED` has a named, timeboxed
  spike task placed first in the roadmap phase that depends on it.
- Every ONE-WAY decision (stack, API contract v1, app name + bundle id)
  was confirmed at the Twin Brief Gate or is marked `UNCONFIRMED`.
- Roadmap Phase 1 is the walking skeleton — real auth + one list + one
  detail screen, end-to-end on a device. Any other Phase 1 fails this check.
- Every risk in the register names something specific to THIS product;
  risks that could appear in any app blueprint fail.
- `store/` exists with drafted content in every file — zero empty
  placeholders — and the privacy/deletion links are either live URLs or
  explicit roadmap items.
- The user could hand Phase 1 to deep-plan today.
- Report: scope (include/adapt/exclude/mobile-new counts), the API gap
  list (the real cost center), stack decision in one line, the recommended
  app name + category, legal-link status, and the top 3 risks.

## When things go wrong

| Situation | Response |
|-----------|----------|
| No API at all (fully server-rendered) | Not a blocker — the contract IS the gap list; say plainly that server-side work is the longest pole and phase it first |
| The census yields zero `mobile-new` rows | Flag it: a twin with no mobile-only value is a website in a costume — challenge the driver with the user before writing the blueprint |
| Web design tokens don't exist (ad-hoc CSS) | Extract the de-facto values (most-used colors/sizes via grep), propose them AS the token set, and note the web debt |
| Font license doesn't cover app embedding | Name it as a Phase 1 blocker with alternatives (licensed tier, similar open font) — never assume the web license transfers |
| The site is behind auth and can't be crawled | Ask for credentials or fall back to codebase-only evidence; never invent features from the marketing page |
| User wants a WebView wrapper anyway | Explain the minimum-functionality rejection risk with a current citation, then respect the decision — document it as an accepted risk |

# Store Listing & Publish Kit Guide

Derive the store listing from evidence (the web product's own brand and
copy), fill the legal requirements, and scaffold a `store/` folder so
nothing is invented the night before submission.

> **Currency rule:** store policies, character limits, category lists, and
> asset specs change. Numbers in this guide are illustrative — during the
> run, fetch the CURRENT official docs (App Store Connect / Play Console
> help) and cite URL + date next to every limit and requirement you state.

## Part 1 — Derive the listing text

**App name** — from the web brand, with evidence: `og:site_name`,
`<title>`, logo lockup (`file:line`). Rules:

- Both stores cap the name around 30 chars — verify current limits.
- If the plain brand name is likely taken, propose `<Brand> — <qualifier>`
  variants (e.g. "Horizon — Design Magazine").
- Name availability can only be confirmed inside the store consoles — mark
  it `UNVERIFIED (manual console check)` and say so.
- Deliver: 2–3 candidates, recommended one first, with reasoning.
- Propose the **bundle id / application id** alongside the name (e.g.
  `com.<brand>.app`, reverse-DNS from a domain the user controls). This
  is **ONE-WAY**: the Android applicationId can never change after the
  first Play submission, and the iOS bundle id is fixed once the App
  Store record exists — verify the current rules and cite. It goes
  through the Twin Brief Gate with the name.

**Subtitle (iOS, ~30 chars) / Short description (Android, ~80 chars)** —
the value proposition in one line, derived from the census's `mobile-new`
rows (that's what makes the app worth installing over the website).

**Long description** — structure:

1. First 1–3 lines carry everything (users see them before "more") — the
   hook, from the site's own og:description/landing copy (`file:line`).
2. Feature bullets = the census's `include` + `mobile-new` rows, phrased as
   user benefits, not features ("Read offline on the train", not "offline
   cache").
3. Close with trust: who makes it, link to the web product.
4. Weave search keywords naturally; iOS also has a separate keywords field
   (~100 chars, no spaces after commas) — draft it.
5. **Localize:** if the site serves a non-English audience, draft the
   listing in the site's language(s) first, English second.

**Category** — fetch the current category lists for both stores, then
recommend primary + secondary with a one-line reason each (e.g. a magazine
platform → Apple "Magazines & Newspapers" or "News"; Google Play "News &
Magazines"). Wrong category = invisible to the right audience and a
rejection vector — cite the list you chose from.

## Part 2 — Legal links

**Privacy policy URL (required by both stores):**

1. **Audit first:** does the website already have one? Check routes,
   footer links, `/privacy*` pages (`file:line`). If yes — use the
   canonical URL, but review coverage: the APP adds data the web policy
   likely doesn't mention (device identifiers, push tokens, analytics/
   crash SDKs, advertising IDs if any). List the gaps; the policy must be
   updated before submission.
2. **If none exists:** recommend generators — [iubenda](https://www.iubenda.com)
   (paid, auto-updating, app-aware), [Termly](https://termly.io),
   [GetTerms](https://getterms.io), [PrivacyPolicies.com](https://www.privacypolicies.com),
   [FreePrivacyPolicy](https://www.freeprivacypolicy.com) (free tiers) —
   pick per budget; flag that generated policies still need review against
   the actual data flows (and regional law: GDPR / Thailand PDPA / etc.).
3. **Host it on the canonical domain** (`https://<site>/privacy`) — the
   store link, the in-app link, and the web footer should all point to ONE
   canonical policy. Never a Google Doc or a generator-hosted orphan page.

**Account deletion:**

- Fetch and cite the current requirements: Apple requires in-app account
  deletion for apps with account creation; Google Play requires a web
  deletion URL declared in Data safety.
- **Recommended implementation:** a web route on the existing site
  (`https://<site>/account/delete`) reusing web auth — one flow serves the
  Play declaration, the Apple in-app webview/link, and web users. Pair
  with the API endpoint from the contract (the account-deletion endpoint
  is already a contract requirement). Design: confirm → optional grace
  period → hard delete + token revocation. What "delete" actually removes
  (posts? comments? subscriptions?) is a product decision — surface it to
  the user with a recommendation.

**Data safety / privacy labels worksheet** — both stores make you declare
collected data. Derive it from the API contract, not from guesswork:

| Data | Collected by | Purpose | Linked to identity? | Shared? |
|------|--------------|---------|--------------------:|---------|
| Email | auth endpoints | account | yes | no |
| Push token | device registry | notifications | yes | no |
| Reading analytics | analytics SDK | product | <decide> | <SDK vendor?> |

Every third-party SDK in the stack decision adds rows — check each SDK's
current data disclosure documentation and cite it.

## Part 3 — The publish kit (`store/` folder)

Scaffold this structure in the repo and **draft every file with real
content from this run** — no empty placeholder files:

```
store/
├── listing/
│   ├── listing.md            # name candidates, subtitle/short desc, long description(s), keywords — per locale
│   └── categories.md         # primary/secondary per store + rationale + list citation
├── legal/
│   ├── privacy.md            # canonical URL + coverage-gap list, or generator recommendation + draft outline
│   ├── account-deletion.md   # the deletion URL, flow design, what gets deleted
│   └── data-safety.md        # the worksheet above, mapped from the API contract
├── assets/
│   ├── icon/README.md        # required sizes/formats (cited from current specs) + source-logo pointer
│   ├── screenshots/README.md # required device classes & dimensions (cited) + shot list matching the walking skeleton screens
│   └── feature-graphic/README.md  # Play feature graphic spec (cited)
└── checklist.md              # pre-submission checklist per store: developer accounts enrolled, listing
                              # fields, legal links live, assets present, age rating questionnaire,
                              # build uploaded, data safety filed
```

Notes:

- **Developer accounts are a prerequisite with lead time:** Apple
  Developer Program and Google Play Console enrollment carry fees and
  identity-verification delays (organization enrollment may require a
  D-U-N-S number). Verify current fees and requirements (cite URL + date)
  and put enrollment on the roadmap before the first on-device/TestFlight
  build — not the night before submission.

- `assets/*/README.md` files carry the specs and the shot list; the actual
  images are produced later (Phase includes them) — but the *requirements*
  are pinned now, with citations, so design can produce them in one pass.
- `checklist.md` items are verifiable ("privacy URL returns 200 and
  mentions push tokens"), not vibes ("legal looks good").
- The blueprint's roadmap references this folder: listing/legal work is
  roadmap work with owners, not an afterthought.

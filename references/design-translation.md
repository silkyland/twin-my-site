# Design Language Translation Guide

"Same tone as the web" means shared brand DNA expressed in native idioms —
not the web layout cloned onto a phone. Extract with evidence, then
translate.

## Part 1 — Extract from the web (with citations)

Pull from the actual codebase, `file:line` each:

- **Color tokens:** CSS variables / theme / Tailwind config — brand,
  surface, text, semantic (success/warn/error). Note contrast ratios while
  extracting; fix-on-translate if the web fails them.
- **Typography:** families, weights in use, the real size scale (h1…caption),
  line heights. Check the font's license covers **app embedding** — web
  licenses often don't; this is a classic late surprise.
- **Spacing & shape:** the spacing scale actually used (grep margins/
  paddings for the rhythm), border radii, elevation/shadow style.
- **Components:** inventory the recurring ones (article card, author chip,
  issue cover grid, paywall banner…) with their anatomy.
- **Voice:** microcopy tone (formal? playful?), empty states, error wording
  — the twin speaks the same language, literally.
- **Imagery:** aspect ratios, crop conventions, illustration style.

## Part 2 — Translate, don't transplant

Same tokens, native behavior. The standard mappings:

| Web pattern | App idiom |
|-------------|-----------|
| Top navbar + mega-menu | Bottom tab bar (3–5 destinations) + in-tab navigation |
| Breadcrumbs | Navigation stack + back gesture |
| Hover states / tooltips | Press states, long-press; haptics where meaningful |
| Modal dialogs | Sheets (bottom sheets on both platforms) |
| Toasts | Snackbars (Android) / unobtrusive banners (iOS) |
| Multi-column layouts | Single column with hierarchy; grids only for covers/galleries |
| Pagination controls | Infinite scroll + pull-to-refresh |
| Footer links (about/terms) | Settings/About screen |
| px/rem scale | pt (iOS) / dp (Android) — map the scale, don't copy values blindly |

Platform-respect rules:

- Brand color and type carry the identity; **navigation patterns, gestures,
  and system components follow the platform.** A twin that fights platform
  conventions feels broken to natives of that platform.
- Support platform text scaling (Dynamic Type / font scale) from day one —
  content apps live and die by readability settings.
- Accessibility transfers as intent, not markup: web ARIA labels become
  platform accessibility labels on the mapped components.

## Part 3 — Decisions the web can't answer

Surface these to the user explicitly (recommended answer attached):

- **Dark mode** — stores and users expect it; if the web has no dark
  tokens, derive a proposal from the brand palette.
- **App icon & splash** — from the logo system; note if the logo won't
  survive small sizes.
- **Motion** — the web's transitions may be none; propose a restrained
  native motion set (navigation transitions, pull-to-refresh) in brand
  character.

## Output format

1. **Mobile token set** — colors/type/spacing/radius as a token table
   (name, value, source `file:line`), ready to become theme code.
2. **Component map** — web component → app component, per-platform notes,
   one row per census component.
3. **Open design decisions** — the Part 3 list with recommendations.

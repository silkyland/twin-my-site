# Feature Census Guide

Turn "read the features and functions of my web product" into an
evidence-backed matrix. Features come from code, not from memory of using
the site.

## Where features live (walk all of these)

- **Routes** — the route table/file is the closest thing to a feature list
  the code has. Every user-facing route is a feature candidate.
- **Navigation** — menus, footers, dashboards: what the product tells users
  it can do.
- **Controllers/handlers** — actions not linked from any menu (search,
  filters, exports, AJAX endpoints) hide here.
- **Templates/pages** — interactive elements per page: forms, toggles,
  comment boxes, bookmark buttons.
- **Roles** — features per audience (anonymous / registered / subscriber /
  admin). For a magazine: reader features vs editorial features are
  different products.

## The matrix

One row per feature:

| Feature | Audience | Evidence | App verdict | Reason / mobile shape | API needs |
|---------|----------|----------|-------------|----------------------|-----------|
| Article reading | reader | `routes/web.php:24`, `ArticleController.php:31` | include | core value | `GET /articles/{id}` |
| Issue archive browse | reader | `routes/web.php:31` | adapt | mega-menu → tab + filter sheet | `GET /issues?cursor=` |
| Comment posting | registered | `CommentController.php:18` | include | needs token auth | `POST /articles/{id}/comments` |
| Editorial CMS | admin | `routes/admin.php` | exclude | desktop workflow; stays web | — |
| New-issue alert | reader | — (doesn't exist) | mobile-new | the app's reason to exist | push pipeline |

## Verdict rules

- **include** — core value, works as a native screen.
- **adapt** — the capability transfers, the interaction doesn't; name the
  mobile shape (long-form nav → tabs, hover-revealed actions → swipe/long
  press, multi-column → single column with hierarchy).
- **exclude** — needs a reason a stakeholder accepts ("admin is a desktop
  workflow used by 3 staff") — not "hard to build".
- **mobile-new** — push, offline reading, share sheet, home widgets, app
  shortcuts. These justify the app's existence in the worth-it sense;
  a twin with zero mobile-new rows is a website in a costume — flag that
  to the user honestly.

## Deriving API needs

Each `include`/`adapt` row names the endpoints it consumes (they feed the
Step 2 contract). Group rows by data resource (articles, issues, users,
comments) — the resource list IS the API's skeleton.

## Scope recommendation

Default recommendation for content products: **reader app first** —
`include`+`adapt` the reading/consuming features, `exclude` the
authoring/admin side, ship, learn, then revisit. Present it as the
recommendation with the row counts as evidence.

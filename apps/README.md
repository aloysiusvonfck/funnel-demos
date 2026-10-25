# Embeddable apps

Self-contained drop-in widgets a client site can host: no build step, no
dependencies, original code under the MIT license. Built on Casey's direction
Oct 25, 2026. Mature open-source widgets are welcome where they beat a fresh
build; the Roadmap lists the imports. apps/INDUSTRIES.md maps every lane to
the apps that fit it.

## Catalog

| App | What it does |
|---|---|
| quote-calculator | Instant ballpark estimate from service, size and urgency, with a call CTA |
| service-request | Service request form; posts to the fulfillment webhook when configured, mailto fallback otherwise |
| booking-request | Day + time picker over the next two weeks, posts a booking request or falls back to mailto |
| reservation | Restaurant table requests: day, time, party size; posts to the webhook or falls back to mailto |
| gift-card | Gift card purchase request: amount (preset or custom), recipient, message; hands off to the client's checkout when configured |
| review-wall | Review cards with stars, fed from a JSON config |
| testimonial-carousel | Auto-rotating quotes with arrows and dots |
| before-after | Draggable before/after image comparison for work photos |
| photo-gallery | Photo grid with a keyboard-friendly lightbox |
| menu | Restaurant menu with categories, descriptions and dietary tags |
| hours-status | Live Open now / Closed badge from the business's hours and timezone, week list, call button |
| location-card | Embedded map, address, directions and call buttons |
| payment-estimator | Monthly payment estimator (price, down payment, term, APR) for financing offers |
| class-schedule | Weekly schedule grid for classes and sessions |
| team-grid | Staff cards with photos or initials avatars |
| faq | Accordion FAQ from a JSON config |
| promo-banner | Special-offer bar with an optional live countdown |

## Embedding

iframe (recommended, keeps host styles clean):

    <iframe src="/apps/quote-calculator/index.html" style="width:100%;max-width:620px;height:620px;border:0" title="Instant estimate" loading="lazy"></iframe>

## Configuring

Each app reads a JSON config from its inline `app-config` script tag, so a
client's data is edited in one place. Per-client overrides without forking the
file: add `?config=<url>` to the iframe src, where the URL returns the same
shape as the config object (hours-status and payment-estimator read their
config inline only; set per-client values by generating the file at build
time).

Theming: every app honors CSS variables `--app-accent`, `--app-ink`,
`--app-muted`, `--app-line`, `--app-bg`, `--app-soft`, so a client's palette
applies without touching the app. `?accent=%23RRGGBB` overrides the accent.

## Pipeline

Launch builds stay five pages with no apps. Growth and Custom tiers pass
`--apps quote-calculator,booking-request` (and so on) to `pipeline/build.py`,
which copies `apps/<name>/` into the output and strips every template section
marked `data-app` whose app was not bundled, so no build ever carries a
dangling embed.

## Roadmap (imports and builds)

- Date and time picker on booking-request and reservation: vendor flatpickr
  (MIT license).
- Map tiles beyond the Google embed: vendor Leaflet (BSD-2-Clause license).
- Live availability on booking-request and reservation: build against the
  client's calendar provider (Growth add-on per lane); today the apps show
  the business's standard weekly slots and every request is confirmed by a
  person, which is the honest version until the calendar hook exists.
- Photo and document upload for insurance and invoices: build on the intake
  webhook (needs a storage target; phase 4).
- Inventory/price-list app for retail lanes; before/after needs job photos
  from the client's onboarding materials.
- Gift card payment: the app hands off to the client's own checkout link
  (their Stripe payment link set as `checkoutUrl`); we never hold card data.

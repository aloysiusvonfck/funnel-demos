# Embeddable apps

Self-contained drop-in widgets a client site can host: no build step, no
dependencies, original code under the MIT license. Built on Casey's direction
Oct 25, 2026. Mature open-source widgets are welcome where they beat a fresh
build; the Roadmap lists the imports.

## Apps

| App | What it does |
|---|---|
| quote-calculator | Instant ballpark estimate from service, home size and urgency, with a call CTA |
| service-request | Service request form; posts to the fulfillment webhook when configured, mailto fallback otherwise |
| review-wall | Review cards with stars, fed from a JSON config |

## Embedding

iframe (recommended, keeps host styles clean):

    <iframe src="/apps/quote-calculator/index.html" style="width:100%;max-width:620px;height:620px;border:0" title="Instant estimate" loading="lazy"></iframe>

## Configuring

Each app reads a JSON config from its inline `app-config` script tag, so a
client's data is edited in one place. Per-client overrides without forking the
file: add `?config=<url>` to the iframe src, where the URL returns the same
shape as the config object.

- quote-calculator: `phone`, `ctaText`, `note`, `services[{name,low,high,sizeFactor}]`, `urgency[{label,mult}]`
- service-request: query params `endpoint` (POST target) and `email` (mailto fallback)
- review-wall: `average`, `reviews[{stars,quote,who}]`

Theming: every app honors CSS variables `--app-accent`, `--app-ink`,
`--app-muted`, `--app-line`, `--app-bg`, `--app-soft`, so a client's palette
applies without touching the app. `?accent=%23RRGGBB` overrides the accent.

## Pipeline

Launch builds stay five pages. Growth and Custom tiers inject chosen apps
during the build step: `pipeline/build.py` copies `apps/<name>/` into the
output and the template's embed block references them.

## Roadmap (imports and builds)

- Date and time picker on service-request: vendor flatpickr (MIT license).
- Map of the service area: vendor Leaflet (BSD-2-Clause license).
- Photo and document upload for insurance and invoices: build on the intake
  webhook (needs a storage target; phase 4).
- Booking flow with real availability: build against the client's calendar
  provider (Growth add-on per lane).

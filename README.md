# Shipping

The common shipping vocabulary -- shipments and the parcels in them, the rates quoted to send them, the labels that send them, the tracking that follows them, the pickups that collect them and the carriers that move them -- distilled from the APIs that already exist. Part of [Schema Commons](https://github.com/api-commons/schema-commons),
published by the [API Commons](https://apicommons.org).

Built from the providers tagged Shipping, Logistics, Last Mile Delivery, Fulfillment, Delivery, Parcel, Courier, 3PL, Freight in [APIs.io](https://apis.io) that
publish their own OpenAPI. Only harvested contracts count -- nothing API Evangelist
generated. Every number below is a count of providers.

## What the market already agrees on

<!-- commons:summary -->
| Concept | Expose it | Readable schema | Core properties (share) | Collection named |
| --- | ---: | ---: | --- | --- |
| shipment | 81 | 75 | id 75%, status 64% | shipments 60, deliveries 15, shipment 12, delivery 5 |
| rate | 12 | 11 |  | rates 10, quotes 1, rate 1 |
| label | 22 | 19 |  | labels 16, label 7, shipping-labels 2 |
| tracking | 43 | 38 | status 63%, tracking_number 58% | tracking 31, track 8, trackings 5, trackers 4 |
| pickup | 11 | 9 | id 67%, address 56%, pickup_date 56%, status 56% | pickups 9, pickup 3 |
| carrier | 17 | 17 | name 76%, id 53% | carriers 12, couriers 1, carrieraccounts 1, carrier 1 |

Cohort: 262 providers with harvested OpenAPI, 103 crosswalks. Of the providers exposing a shipment: 72% create, 60% list, 74% fetch, 35% update, 26% delete. Generated 2026-11-12 by build.py.
<!-- /commons:summary -->

Shares are of the providers with a readable resource schema for that concept. The
extended tier and the long tail of every concept are in [profile.yml](profile.yml).

## What the measurement shows

- A shipment agrees only on id and status. From and to addresses sit at 44%: carriers, multi-carrier platforms and last-mile delivery networks each nest the addresses differently.
- Tracking agrees on the tracking number and a status; the event history is modelled too differently to share.
- Labels agree on nothing outright: a label is a file at some providers, a purchase record at others, and part of the shipment at the rest.
- The carriers themselves are absent: FedEx and USPS have harvested specs without recorded provenance, DHL publishes none the catalog holds, and UPS's versioned action paths do not match. The context is built from multi-carrier platforms and delivery networks.

## Artifacts

- [openapi.yml](openapi.yml) -- OpenAPI 3.1 of the shared operations. No one serves it; it
  is the shape to map to. Lints clean against the
  [naming ruleset](https://github.com/api-commons/schema-commons/blob/main/ruleset/schema-commons-naming.yaml).
- [arazzo.yml](arazzo.yml) -- reference workflows composed only of the shared operations (compositions, not measurements).
- [schema/](schema) -- JSON Schema 2020-12 per concept; each property carries
  `x-commons-tier`, `x-commons-share`, `x-commons-providers` and the spellings seen.
- [crosswalk/](crosswalk) -- one per provider, common name -> their name, with the harvested
  spec and operations each mapping was read from.
- [adopters.yml](adopters.yml) -- who exposes which concept, linked to their APIs.io page.
- [concepts.yml](concepts.yml) -- the only hand-written input: concepts, path segments and
  their qualifiers, aliases, reviewed exclusions, merged duplicates, and the decisions below.
- [census/](census) -- the cohort and the raw census the profile is built from.

## Decisions

These are calls, not measurements, recorded so they can be revisited as the method matures.

- Shipping owns the shipment. Commerce's `/orders/{order_id}/shipments` is the order's view of it.
- A last-mile delivery (DoorDash Drive, Grab Express, Metrobi) is a shipment: a parcel moved from one address to another.
- A rate is `/shipping-rates`, though providers say `rates`: a bare rate is an FX or tax rate in other contexts (the same call as travel's `/rate-plans`). Bare `quotes` and `estimates` were sales quotes and ride estimates and do not count.
- A courier is a carrier only when it is a company: Nash's and Circuit's couriers are drivers.
- Tracking is `/tracking` (28 providers), covering trackers, tracks and trackings.
- Duplicates merged: Kargo.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).

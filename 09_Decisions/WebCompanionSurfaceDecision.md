# Web Companion Surface Decision

## 1. Status

Locked for JoinFolk v1 release hardening.

This decision defines the browser-facing user/public surface that complements, but does not replace, the JoinFolk mobile app and host dashboard.

## 2. Product Boundary

JoinFolk has three distinct surfaces:

| Surface | Primary audience | Purpose |
| --- | --- | --- |
| Mobile app | attendees, buyers, participants, hosts on the go | primary event experience |
| Dashboard | hosts, businesses, staff, ops | event, venue, commerce, and operational management |
| Web Companion | public visitors, personal users, buyers before app install | lightweight discovery, profile, ticket, and app handoff surface |

Web Companion is not the dashboard.

Web Companion is not a full mobile app clone.

Web Companion is a lightweight browser surface for public discovery, personal account access, ticket visibility, organizer/event pages, and mobile app handoff.

## 3. Problem

The current browser surface is dashboard-heavy.

Authenticated non-host users can land on `/personal`, but that surface is currently too limited and does not represent JoinFolk as a public/user product.

A user should be able to open JoinFolk in a browser and understand:

- what event is being shared
- who the organizer is
- what events the organizer has
- whether tickets/reservations exist
- whether they should sign in
- whether they should install/open the mobile app
- how to access their personal tickets or claims

## 4. Core Rule

Browser users must not be forced into the host dashboard unless they are using host/business/ops surfaces.

Normal users need a Web Companion surface.

Hosts may use dashboard and may also appear publicly through organizer profiles.

## 5. Route Families

Required route families:

| Route family | Purpose |
| --- | --- |
| `/e/:id` | public event detail/share page |
| `/organizers/:id` or `/u/:handle` | public organizer profile and organizer events |
| `/personal` | authenticated personal mini home |
| `/personal/tickets` | authenticated tickets, claims, and reservations overview |
| `/download` | mobile app download/open handoff |
| `/login` | shared auth entry with return intent |
| `/dashboard/*` or existing dashboard routes | host/business/ops management |

Existing dashboard routes may remain as-is, but public/user routes must not rely on dashboard mental model.

## 6. Public Event Page Contract

A public event page should show:

- event title
- event poster/media
- date/time
- location or location summary
- organizer identity
- event status
- ticket/reservation availability summary
- primary CTA
- app handoff CTA

Primary CTA rules:

| Viewer state | CTA |
| --- | --- |
| guest | sign in or continue in app |
| authenticated non-participant | buy/request/join if available |
| ticket holder | view ticket in app or personal tickets |
| participant | open in app |
| host owner | manage in dashboard |
| staff | open scanner/mobile staff flow if assigned |

If browser checkout is not supported for a flow, the CTA should honestly redirect to the mobile app or email ticket/claim flow.

## 7. Organizer Profile Contract

Public organizer profile should show:

- organizer display name
- organizer avatar
- organizer bio/summary
- verified or host context when available
- upcoming public events
- past public events if product allows
- follow/contact/share affordances when available
- app handoff CTA

Organizer profile must not expose private dashboard data.

## 8. Personal Web Contract

Authenticated personal users should be able to access a minimal personal web area.

Minimum content:

- personal identity summary
- tickets
- reservations
- claims
- upcoming joined events
- app download/open CTA
- sign out

This surface is not host dashboard.

If a normal user attempts a dashboard-only route, they should be redirected to personal web or denied, not shown host tooling.

## 9. Ticket And Claim Contract

Web Companion may support lightweight ticket and claim visibility.

Allowed:

- show ticket/reservation summary
- show claim status
- email-based ticket claim handoff
- app handoff for QR/check-in/live event
- download/open app CTA

Not required for v1 unless explicitly implemented:

- full browser checkout
- scanner
- live media upload
- realtime event interactions
- full wallet replacement

## 10. Mobile App Handoff

The mobile app remains the primary experience.

Web Companion must consistently provide:

- download/open app CTA
- explanation that richer live event features require the app
- return-intent preservation where possible

The app handoff must not hide missing browser capability behind broken buttons.

## 11. Auth Intent Preservation

When a user opens a public event or organizer page and signs in, the intended return route should be preserved.

Examples:

- guest opens `/e/:id`
- guest clicks ticket/request/join
- login required
- after login, return to `/e/:id` or the intended action state

Losing intent after login is a product bug.

## 12. Security Boundary

Web Companion must not weaken dashboard security.

Rules:

- public event pages may only show public/share-safe data
- organizer pages may only show public organizer profile data
- personal pages require authenticated user
- dashboard pages require host/business/ops authorization
- staff scanner routes require event-specific host/staff authorization
- non-owner users must not access host event detail routes

Cloudflare project separation is not a security boundary.

App authorization, Supabase RLS/RPCs, and route guards remain the security boundary.

## 13. Dashboard Boundary

Dashboard remains for:

- event creation/editing
- venue editing
- product/ticket management
- reservation management
- staff management
- ops tools
- host analytics
- media engine/admin tools

Web Companion must not expose these unless the user is authorized and intentionally enters dashboard.

## 14. Non-Goals

Web Companion v1 does not require:

- full mobile app clone
- full browser-native live event experience
- browser camera/media capture parity
- scanner replacement
- complete checkout replacement
- new native app features
- broad dashboard redesign

## 15. Implementation Order

Required implementation order:

1. document Web Companion route and access model
2. verify existing `/personal` and `/e/:id` behavior
3. implement public event page improvements
4. implement organizer profile
5. improve personal tickets/claims surface
6. add consistent app handoff CTAs
7. preserve login return intent
8. only then consider browser checkout expansion

## 16. Release Rule

JoinFolk v1 must not present the browser product as dashboard-only.

A public/user Web Companion surface is required for a complete market-facing product, even if the mobile app remains the primary experience.

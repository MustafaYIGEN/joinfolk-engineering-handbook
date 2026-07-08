# Dashboard Authorization Matrix Report

## 1. Metadata

- Status: Draft
- Owner: Mustafa / JoinFolk
- Source confidence: code-inspection required
- Audit type: Dashboard authorization / route guard / page data access / role matrix
- Related inventory:
  - `07_Audits/WebAccessIAInventoryReport.md`
- Related decisions:
  - `09_Decisions/WebAccessPersonaRoutingDecision.md`
  - `09_Decisions/CloudflareProductionSurfaceRoutingDecision.md`
- Inspection date: 2026-07-08

## 2. Scope

This report inventories dashboard authorization evidence before any dashboard authorization patch.

It focuses on:

| Gap | Question |
| --- | --- |
| WA-03 | Are web staff routes sufficiently protected by route, page, assignment, and data-access checks? |
| WA-04 | Are dashboard event nested tabs sufficiently protected by route, page, ownership, and RPC/data-access checks? |
| WA-06 | Are semi-pro/pro route semantics correct and intentional? |

## 3. Repository Evidence

| Path | Exists | Git branch | HEAD commit | Remote | Notes |
| --- | --- | --- | --- | --- | --- |
| `C:\dev\joinfolk-web` | Yes | `refactor/joinfolk-stabilization-p0` | `86dba59b4155efaeba13d0be369409367174bb68` | `MustafaYIGEN/joinfolk-web` | Contains dashboard and public web source inspected for dashboard route/guard evidence. |
| `C:\dev\joinfolk-web\dashboard` | Yes | `refactor/joinfolk-stabilization-p0` | `86dba59b4155efaeba13d0be369409367174bb68` | `MustafaYIGEN/joinfolk-web` | Dashboard React Router app inspected for route guards, pages, hooks, and API references. |
| `C:\dev\hostos` | Yes | `refactor/joinfolk-stabilization-p0` | `72d3a9f3c795e8b9e06060ab7b78fe88b690353c` | `MustafaYIGEN/joinfolk-platform` | Platform source root; used only for read-only reference where needed. |
| `C:\dev\hostos\apps\mobile` | Yes | `release/ios-v17-media-performance` | `6a1224bac1ed4c2a451b50611dc1967fc1e99d07` | `MustafaYIGEN/joinfolk-web` | Separate nested Expo/mobile git repository. Not inherited from `C:\dev\hostos`. |

`C:\dev\hostos\apps\mobile` is a separate nested git repository even though it is physically located under `C:\dev\hostos\apps`. It must not be treated as inherited from the platform repo for release evidence.

## 4. Guard Inventory

| Guard / helper | File | What it checks | Redirect/block behavior | Notes |
| --- | --- | --- | --- | --- |
| `AuthGuard` | `C:\dev\joinfolk-web\dashboard\src\components\AuthGuard.tsx` | Supabase session/loading state from `useAuth`. | Redirects unauthenticated users to `/login` with `state.from`. | Route-level authentication guard. |
| `HostGuard` | `C:\dev\joinfolk-web\dashboard\src\components\HostGuard.tsx` | `tier` from `useAuth`; blocks `tier === "user"`. | Redirects standard users to `/personal`; permits non-user tiers. | Dashboard host-tier guard for main dashboard route group. |
| `SemiProGuard` | `C:\dev\joinfolk-web\dashboard\src\components\SemiProGuard.tsx` | `tier === "semi_pro"`. | Redirects all other tiers, including `pro`, to `/`. | Comments state business/venue surfaces are exclusively semi-pro and pro organizers use event/ticket surfaces only. |
| `ProGuard` | `C:\dev\joinfolk-web\dashboard\src\components\ProGuard.tsx` | Blocks `tier === "user"`; permits semi-pro and pro. | Redirects user tier to `/`. | Component exists but route inventory did not show it applied in `src\App.tsx`; comments say event surfaces must be accessible to both tiers. |
| `OpsGuard` | `C:\dev\joinfolk-web\dashboard\src\components\OpsGuard.tsx` | `isOps` from `useAuth`. | Redirects non-ops users to `/`. | Route-level guard for `/ops/*`. |
| `EventOwnerGuard` | `C:\dev\joinfolk-web\dashboard\src\components\EventOwnerGuard.tsx` | Current user id, event id, `host_id`, and `created_under_persona = "host"`. | Redirects to `/events` when owner check fails. | Applied to `/event-reservations/:id`; not applied to all `/events/:id/*` nested tabs. |
| `StaffLayout` | `C:\dev\joinfolk-web\dashboard\src\components\StaffLayout.tsx` | Auth session display only. | No independent staff assignment block found in layout. | Wrapped by `AuthGuard`; staff assignment filtering is in pages, not the layout guard. |
| Staff assignment query | `C:\dev\joinfolk-web\dashboard\src\pages\staff\StaffEventsPage.tsx` | Queries `event_staff_assignments` for `staff_user_id = session.user.id`. | Redirects to `/staff/login` if no session; lists assigned events. | Page-level assignment filtering found for staff event list. |
| Mobile staff scan helper | `C:\dev\hostos\apps\mobile\app\(tabs)\event\[id]\scan.tsx` and `lib\event-staff.v1.ts` | Mobile reference checks event host or staff assignment before scanner use. | Blocks scanner when viewer role is not host or staff. | Read-only mobile reference only; does not prove web route-level staff authorization. |
| Venue owner helpers | Dashboard API hooks/RPC references | Venue routes use venue RPCs such as `list_my_venues_v1`, `get_venue_v1`, and related venue operations. | Server/RPC enforcement not proven from dashboard source alone. | No separate route-level venue owner guard component found beyond `SemiProGuard`. |

## 5. Dashboard Route Authorization Matrix

| Route | Component/page | Route guard chain | Page-level owner/staff check found? | Data/RPC owner filter found? | Status |
| --- | --- | --- | --- | --- | --- |
| `/login` | `LoginPage` | None | Not applicable | Not applicable | Public auth route. |
| `/` | `TierLanding` | `AuthGuard` + `HostGuard` | Tier landing branches by tier. | Not applicable | Supported for host-tier routing. |
| `/personal` | `PersonalHomePage` | `AuthGuard` | Not host-gated by design. | Unknown | Supported for authenticated personal surface. |
| `/events` | `EventListPage` | `AuthGuard` + `HostGuard` | Event list uses host event API path. | `fetchHostEvents()` filters `host_id` and `created_under_persona`. | Supported with source-level host filtering. |
| `/events/new` | `EventCreatePage` | `AuthGuard` + `HostGuard` | Uses current session/user context for host event creation. | `createEventDraft()` sets `host_id` and `created_under_persona`. | Supported with gaps; backend enforcement not proven here. |
| `/events/:id` | `EventDetailPage` / `EventSummary` | `AuthGuard` + `HostGuard` | `useEventDetail()` returns the event only from `fetchHostEvents()`. | Shared event detail source filters host events before matching id. | Supported with gaps; no route-level `EventOwnerGuard`. |
| `/events/:id/content` | `ContentPage` | `AuthGuard` + `HostGuard` | Uses `useEventDetail(id)`. | Content updates use event id; server/RPC owner enforcement not proven from route file. | Supported with gaps. |
| `/events/:id/modules` | `ModulesPage` | `AuthGuard` + `HostGuard` | Uses `useEventDetail(id)` and tier-limited module visibility. | Module RPCs called by event id; server/RPC owner enforcement not proven from dashboard source alone. | Supported with gaps. |
| `/events/:id/attendees` | `AttendeesPage` | `AuthGuard` + `HostGuard` | Uses `useEventDetail(eid)`. | Ticket queue RPC called by event id; server/RPC owner enforcement not proven from dashboard source alone. | Supported with gaps. |
| `/events/:id/invites` | `InvitesPage` | `AuthGuard` + `HostGuard` | Uses `useEventDetail(id)`. | Invite data boundary not fully proven by route inspection. | Supported with gaps. |
| `/events/:id/checklist` | `ChecklistPage` | `AuthGuard` + `HostGuard` | Uses event id; no route-level owner guard. | Checklist RPCs called by event id; server/RPC owner enforcement not proven from dashboard source alone. | Supported with gaps. |
| `/events/:id/poll` | `PollPage` | `AuthGuard` + `HostGuard` | Uses event id; no route-level owner guard. | Poll RPCs called by event id; server/RPC owner enforcement not proven from dashboard source alone. | Supported with gaps. |
| `/events/:id/gallery` | `GalleryPage` | `AuthGuard` + `HostGuard` | Uses event id; no route-level owner guard. | Gallery/moderation RPCs called by event id; server/RPC owner enforcement not proven from dashboard source alone. | Supported with gaps. |
| `/events/:id/tickets` | `TicketsPage` | `AuthGuard` + `HostGuard` | Uses `useEventDetail(id)`. | Product/ticket RPCs called by event id; server/RPC owner enforcement not proven from dashboard source alone. | Supported with gaps. |
| `/events/:id/products` | `ProductsPage` | `AuthGuard` + `HostGuard`; page blocks non-pro tier | Page requires `tier === "pro"`. | Product RPCs and some direct event updates by event id; owner enforcement not proven from dashboard source alone. | Supported with gaps. |
| `/events/:id/venue` | `VenuePage` | `AuthGuard` + `HostGuard` | Uses `useEventDetail(eventId)` and user-scoped template query. | Venue/layout RPCs and event updates by event id; owner enforcement not proven from dashboard source alone. | Supported with gaps. |
| `/events/:id/reservations` | `ReservationsPage` | `AuthGuard` + `HostGuard` | Uses `useEventDetail(eid)`. | Reservation RPCs called by event id; server/RPC owner enforcement not proven from dashboard source alone. | Supported with gaps. |
| `/events/:id/sessions` | `SessionsPage` | `AuthGuard` + `HostGuard`; page blocks non-pro tier | Page requires `tier === "pro"`. | Session API calls by event id; owner enforcement not proven from dashboard source alone. | Supported with gaps. |
| `/events/:id/staff` | `StaffPage` | `AuthGuard` + `HostGuard` | Uses event id; no route-level owner guard. | Staff assignment API calls by event id; owner enforcement not proven from dashboard source alone. | Supported with gaps. |
| `/events/:id/analytics` | `AnalyticsPage` | `AuthGuard` + `HostGuard` | Uses event id; no route-level owner guard. | Stats RPC called by event id; server/RPC owner enforcement not proven from dashboard source alone. | Supported with gaps. |
| `/event-reservations/:id` | `ReservationsPage` | `AuthGuard` + `HostGuard` + `EventOwnerGuard` | Route-level event owner check found. | Reservation RPCs still require server/RPC review. | Supported with gaps. |
| `/venues` | `VenueListPage` | `AuthGuard` + `HostGuard` + `SemiProGuard` | Tier-gated to semi-pro only. | `list_my_venues_v1` source reference found; server/RPC owner enforcement not proven here. | Supported with gaps. |
| `/venues/new` | `VenueCreatePage` | `AuthGuard` + `HostGuard` + `SemiProGuard` | Tier-gated to semi-pro only. | Venue create RPC reference found. | Supported with gaps. |
| `/venues/:venueId` | `VenueDetailPage` | `AuthGuard` + `HostGuard` + `SemiProGuard` | Tier-gated to semi-pro only. | `get_venue_v1` / update venue RPC references found. | Supported with gaps. |
| `/venues/:venueId/media` | `VenueMediaPage` | `AuthGuard` + `HostGuard` + `SemiProGuard` | Tier-gated to semi-pro only. | Venue media RPC/storage references found; owner enforcement not proven here. | Supported with gaps. |
| `/venues/:venueId/offerings` | `VenueOfferingsPage` | `AuthGuard` + `HostGuard` + `SemiProGuard` | Tier-gated to semi-pro only. | Venue offerings RPC references found. | Supported with gaps. |
| `/reservations` | `SemiProReservationsPage` | `AuthGuard` + `HostGuard` + `SemiProGuard` | Tier-gated to semi-pro only. | Host reservation summary uses host/venue data path; server/RPC owner enforcement not proven here. | Supported with gaps. |
| `/staff` | `StaffEventsPage` | `AuthGuard` + `StaffLayout` | Assignment filtering by current `staff_user_id` found. | Query filters `event_staff_assignments.staff_user_id = session.user.id`. | Supported with gaps; route-level staff guard not found. |
| `/staff/scan/:eventId` | `StaffScannerPage` | `AuthGuard` + `StaffLayout` | Event-specific staff assignment check not found in web scanner page. | Page fetches event title by event id; no assignment filter found in web page. | Gap. |
| `/ops/*` | Ops pages | `AuthGuard` + `HostGuard` + `OpsGuard` | `isOps` guard found. | Ops API calls require separate ops review. | Supported with gaps. |

## 6. Staff Authorization Evidence

| Staff route/page | Route guard | Assignment check | Event-specific check | Can normal authenticated user access meaningful staff data? | Status |
| --- | --- | --- | --- | --- | --- |
| `/staff/login` / `StaffLoginPage` | None | No | No | Login form only. | Supported |
| `/staff` / `StaffEventsPage` | `AuthGuard` + `StaffLayout` | Yes, filters `event_staff_assignments` by `staff_user_id = session.user.id`. | Assignment rows determine listed events. | A normal authenticated user without assignment should not see assigned staff event list from source evidence, but backend/RLS enforcement is not proven here. | Supported with gaps |
| `/staff/scan/:eventId` / `StaffScannerPage` | `AuthGuard` + `StaffLayout` | No page-level assignment check found. | No event-specific assignment filter found; page fetches event title by event id and instructs mobile scanner use. | Yes, an authenticated non-staff user may be able to load scanner guidance/title if data access permits. | Gap |
| Mobile scan reference | Mobile event scan page and `event-staff.v1.ts` | Yes | Yes, host/staff assignment checks found before scan flow. | Mobile reference supports intended model but does not secure web route by itself. | Supported with gaps |

## 7. Event Owner Authorization Evidence

| Event area | Route-level owner guard | Page-level event owner check | Query/RPC owner filtering | Cross-host access risk classification | Status |
| --- | --- | --- | --- | --- | --- |
| content | No | `useEventDetail(id)` host-filter evidence found. | Mutations/use calls by event id; server authorization not proven from dashboard source. | P0 candidate until backend/RPC owner enforcement is proven. | Supported with gaps |
| modules | No | `useEventDetail(id)` host-filter evidence found. | Module RPCs by event id; server authorization not proven from dashboard source. | P0 candidate until backend/RPC owner enforcement is proven. | Supported with gaps |
| attendees | No | `useEventDetail(eid)` host-filter evidence found. | Ticket queue RPC by event id; server authorization not proven from dashboard source. | P0 candidate until backend/RPC owner enforcement is proven. | Supported with gaps |
| invites | No | `useEventDetail(id)` host-filter evidence found. | Invite boundary not fully proven from source inspection. | P0 candidate until backend/RPC owner enforcement is proven. | Supported with gaps |
| checklist | No | Event id page context found. | Checklist RPCs by event id; server authorization not proven from dashboard source. | P0 candidate until backend/RPC owner enforcement is proven. | Supported with gaps |
| poll | No | Event id page context found. | Poll RPCs by event id; server authorization not proven from dashboard source. | P0 candidate until backend/RPC owner enforcement is proven. | Supported with gaps |
| gallery | No | Event id page context found. | Gallery/moderation RPCs by event id; server authorization not proven from dashboard source. | P0 candidate until backend/RPC owner enforcement is proven. | Supported with gaps |
| tickets | No | `useEventDetail(id)` host-filter evidence found. | Ticket/product RPCs by event id; server authorization not proven from dashboard source. | P0 candidate until backend/RPC owner enforcement is proven. | Supported with gaps |
| products | No | `tier === "pro"` page block plus event id context. | Product RPCs and direct event updates by event id; server authorization not proven from dashboard source. | P0 candidate until backend/RPC owner enforcement is proven. | Supported with gaps |
| venue | No | `useEventDetail(eventId)` evidence found. | Venue/layout RPCs and direct event updates by event id; server authorization not proven from dashboard source. | P0 candidate until backend/RPC owner enforcement is proven. | Supported with gaps |
| reservations | Only for `/event-reservations/:id`; not for `/events/:id/reservations` | `useEventDetail(eid)` evidence found. | Reservation RPCs by event id; server authorization not proven from dashboard source. | P0 candidate until backend/RPC owner enforcement is proven. | Supported with gaps |
| sessions | No | `tier === "pro"` page block plus event id context. | Session API calls by event id; server authorization not proven from dashboard source. | P0 candidate until backend/RPC owner enforcement is proven. | Supported with gaps |
| staff | No | Event id page context found. | Staff assignment API calls by event id; server authorization not proven from dashboard source. | P0 candidate until backend/RPC owner enforcement is proven. | Supported with gaps |
| analytics | No | Event id page context found. | Stats RPC by event id; server authorization not proven from dashboard source. | P0 candidate until backend/RPC owner enforcement is proven. | Supported with gaps |

## 8. Semi-Pro / Pro Authorization Evidence

| Route/feature | Guard | Expected tier access | Observed tier access | Status | Notes |
| --- | --- | --- | --- | --- | --- |
| Dashboard root `/` | `HostGuard` + `TierLanding` | User blocked; semi-pro/pro allowed. | User redirects to `/personal`; semi-pro gets `SemiProOverviewPage`; pro gets `OverviewPage`. | Supported | Code and comments align. |
| Event routes `/events*` | `HostGuard` | Semi-pro and pro host access. | `HostGuard` allows non-user tiers; comments in `ProGuard` also state event surfaces must be accessible to both tiers. | Supported | `ProGuard` exists but route group uses `HostGuard`. |
| Venue routes `/venues*` | `SemiProGuard` | Semi-pro only, if current product direction is business/venue surfaces for semi-pro. | `SemiProGuard` requires `tier === "semi_pro"` and redirects pro to `/`. | Supported with owner-confirmed semantics needed | Code comments explicitly say pro organizers use event/ticket surfaces only. |
| `/reservations` | `SemiProGuard` | Semi-pro only. | Pro is blocked by `SemiProGuard`. | Supported with owner-confirmed semantics needed | Same guard as venue/business surfaces. |
| Product/ticket pages | Page-level `tier !== "pro"` block in `ProductsPage` | Pro only for product/ticket management. | Semi-pro blocked by page logic. | Supported | Business/venue and event ticket capabilities are split by tier. |
| Sessions page | Page-level `tier !== "pro"` block in `SessionsPage` | Pro only. | Semi-pro blocked by page logic. | Supported | Code comment says pro-tier only. |
| Sidebar venue links | Sidebar tier condition | Semi-pro only. | Links render only when `tier === "semi_pro"`. | Supported | Navigation matches `SemiProGuard`. |

Answers:

- `SemiProGuard` allows only `semi_pro`.
- `SemiProGuard` does not allow `pro`.
- Pro is blocked from semi-pro/venue routes and `/reservations`.
- Source comments indicate this is intentional for Phase 3.7 business/venue surfaces.
- Owner review is still required because the product decision must confirm whether pro users should remain blocked from venue/business routes.

## 9. Data/RPC Boundary Notes

| Page/feature | Table/RPC referenced | Client-side filter | Server/RPC authorization evidence from source | Status |
| --- | --- | --- | --- | --- |
| Event list/detail | `events` via `fetchHostEvents()` | Filters `host_id = uid` and `created_under_persona = "host"` before event detail matching. | Dashboard source proves client query filtering, not production RLS/RPC enforcement. | Supported with gaps |
| Event stats/analytics | `get_event_stats_v1` | Event id only. | RPC implementation not inspected/executed; server owner check not proven. | Unknown |
| Attendees | `get_event_ticket_queue_v2` | Event id only. | RPC implementation not inspected/executed; server owner check not proven. | Unknown |
| Modules | `get_event_modules_v1`, `set_event_modules_v1`, `clear_event_module_v1` | Event id only. | RPC implementation not inspected/executed; server owner check not proven. | Unknown |
| Checklist | `get_event_checklist_v2`, `upsert_event_checklist_v2`, claim/approve/reject RPCs | Event id only for event-scoped calls. | RPC implementation not inspected/executed; server owner check not proven. | Unknown |
| Poll | `get_event_poll_v1`, `upsert_event_poll_v1`, `close_event_poll_v1` | Event id only. | RPC implementation not inspected/executed; server owner check not proven. | Unknown |
| Gallery | `get_event_gallery_v1`, `host_moderate_media_v1` | Event id or media id parameters. | RPC/storage authorization not proven from dashboard source alone. | Unknown |
| Reservations | `get_event_reservations_v1`, `update_reservation_status_v1` | Event id/reservation id parameters. | RPC implementation not inspected/executed; server owner check not proven. | Unknown |
| Staff management | `event_staff_assignments` table operations | Event id and staff assignment ids. | Backend/RLS owner enforcement not proven from dashboard source alone. | Unknown |
| Products/tickets | `get_event_ticket_products_v1`, `upsert_event_ticket_product_v2`, direct `events` updates | Event id, product id; pro-tier page block. | Server owner enforcement not proven from dashboard source alone. | Unknown |
| Venue/business | `list_my_venues_v1`, `get_venue_v1`, venue offering/media/reservation RPCs | Venue id and semi-pro route guard. | Server/RPC venue owner enforcement not proven from dashboard source alone. | Unknown |
| Staff web scanner | `events` title query by event id | No staff assignment filter found in web scanner page. | RLS may restrict, but dashboard source does not prove it. | Gap |
| Mobile scanner reference | `event_staff_assignments`, `staffCheckinTicketV1` | Host/staff assignment checks in mobile source. | Supports intended mobile flow; does not secure web dashboard route. | Supported with gaps |

## 10. Gap Classification

| Gap ID | Original gap | Evidence result | Final severity | Required action |
| --- | --- | --- | --- | --- |
| WA-03 | Web staff route-level authorization unclear | `/staff` list is assignment-filtered by current user, but `/staff/scan/:eventId` lacks a proven web page-level event staff check and only fetches event title by event id. | P0 | Add owner-reviewed staff authorization patch plan or prove RLS/page guard coverage; at minimum address `/staff/scan/:eventId`. |
| WA-04 | Event dashboard nested tab owner authorization unclear | Event list/detail uses host-filtered `fetchHostEvents()` and `useEventDetail()`, but nested tabs are not route-level owner guarded and many data/RPC calls rely on server authorization not proven from dashboard source alone. | P0 | Owner review plus targeted route/page/RPC authorization plan; prove backend/RPC owner enforcement or add consistent route/page guard coverage. |
| WA-06 | Semi-pro/pro semantics need owner review | Source comments and code intentionally make venue/business routes semi-pro-only and pro event/ticket routes pro-only for products/sessions. Product owner confirmation is still required. | P1 | Owner decision confirming tier semantics or patch plan if pro should access venue/business surfaces. |

## 11. Blocked Implementation Claims

- This report does not authorize dashboard authorization implementation.
- This report does not authorize route guard changes.
- This report does not authorize DB/RLS/RPC changes.
- This report does not authorize Cloudflare changes.
- This report does not claim production launch readiness.
- This report does not claim dashboard authorization is secure unless the matrix proves it.

## 12. Required Next Gates

| Gate | Required before | Status |
| --- | --- | --- |
| Owner review of WA-03 classification | Any staff authorization patch or closure | Required |
| Owner review of WA-04 classification | Any event owner authorization patch or closure | Required |
| Owner review of WA-06 classification | Any semi-pro/pro guard patch or closure | Required |
| Targeted dashboard authorization patch plan | Any dashboard authorization code change | Required if P0/P1 remains |
| Manual negative authorization QA | Any release-ready claim | Required if dashboard auth touched |

## 13. No-Modification Confirmation

- No application code was modified by this handbook task.
- No dashboard/mobile/web code was modified by this handbook task.
- No Supabase tree was modified by this handbook task.
- No SQL was executed by this handbook task.
- No production mutation was executed by this handbook task.
- No Cloudflare setting was changed by this handbook task.
- No Supabase CLI was run by this handbook task.
- No files were staged or committed by this handbook task.
- Only `07_Audits/DashboardAuthorizationMatrixReport.md` was created.

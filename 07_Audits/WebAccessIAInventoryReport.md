# Web Access IA Inventory Report

## 1. Metadata

- Status: Draft
- Owner: Mustafa / JoinFolk
- Source confidence: code-inspection required
- Inventory type: Web access IA / route surface / persona entry audit
- Related decisions:
  - `09_Decisions/CloudflareProductionSurfaceRoutingDecision.md`
  - `09_Decisions/WebAccessPersonaRoutingDecision.md`
- Inspection date: 2026-07-08

## 2. Scope

This report inventories the current web-facing access architecture before any routing patch.

| Surface | Local source |
| --- | --- |
| Marketing/root web | `C:\dev\joinfolk-web` and `C:\dev\hostos\apps\web` |
| Dashboard web | `C:\dev\joinfolk-web\dashboard` |
| Platform web/live candidate | `C:\dev\hostos\apps\web` |
| Mobile app routing reference | `C:\dev\hostos\apps\mobile` read-only reference only |

## 3. Repository Evidence

| Path | Exists | Git branch | HEAD commit | Remote | Notes |
| --- | --- | --- | --- | --- | --- |
| `C:\dev\joinfolk-web` | Yes | `refactor/joinfolk-stabilization-p0` | `86dba59b4155efaeba13d0be369409367174bb68` | `MustafaYIGEN/joinfolk-web` | Contains Vite `web` public app and `dashboard` SPA dashboard surfaces. |
| `C:\dev\joinfolk-web\dashboard` | Yes | `refactor/joinfolk-stabilization-p0` | `86dba59b4155efaeba13d0be369409367174bb68` | `MustafaYIGEN/joinfolk-web` | Dashboard React Router app; git metadata inherited from `C:\dev\joinfolk-web`. |
| `C:\dev\hostos` | Yes | `refactor/joinfolk-stabilization-p0` | `72d3a9f3c795e8b9e06060ab7b78fe88b690353c` | `MustafaYIGEN/joinfolk-platform` | Platform source root with `apps\web` and `apps\mobile` subpaths. |
| `C:\dev\hostos\apps\web` | Yes | `refactor/joinfolk-stabilization-p0` | `72d3a9f3c795e8b9e06060ab7b78fe88b690353c` | `MustafaYIGEN/joinfolk-platform` | Next.js marketing/root web candidate; git metadata inherited from `C:\dev\hostos`. |
| `C:\dev\hostos\apps\mobile` | Yes | `release/ios-v17-media-performance` | `6a1224bac1ed4c2a451b50611dc1967fc1e99d07` | `MustafaYIGEN/joinfolk-web` | Separate nested Expo/mobile git repository. Release tag `joinfolk-v1-rc2.1-mobile` points at HEAD. Not inherited from `C:\dev\hostos`. |

`C:\dev\hostos\apps\mobile` is a separate nested git repository even though it is physically located under `C:\dev\hostos\apps`. It must not be treated as inherited from the platform repo for release evidence.

## 4. Cloudflare Surface Context

| Cloudflare project | Domain | Intended role | Current release relevance |
| --- | --- | --- | --- |
| `joinfolk-web` | `join-folk.com`, `www.join-folk.com` | Marketing/root web | Marketing surface |
| `joinfolk-dashboard-live` | `app.join-folk.com` | Production application/dashboard surface | RC-2.1 deploy target |
| `joinfolk-dashboard` | no custom production domain | Non-production dashboard/dev/staging | Not production |
| `joinfolk-web-live` | no custom production domain | Platform web/live candidate | Not selected for RC-2.1 dashboard deploy |

## 5. Route Inventory

### 5.1 Marketing/root web routes

| Route/path | File/component | Public/auth required | Intended user | Notes |
| --- | --- | --- | --- | --- |
| `/` | `C:\dev\hostos\apps\web\app\page.tsx` | Public | Guest/public | Redirects to `/tr`. |
| `/:lang` | `C:\dev\hostos\apps\web\app\[lang]\page.tsx` | Public | Guest/public | Marketing home; includes CTAs to app and host pages. |
| `/:lang/for-hosts` | `C:\dev\hostos\apps\web\app\[lang]\for-hosts\page.tsx` | Public | Host prospect | Links to application login surface. |
| `/:lang/for-venues` | `C:\dev\hostos\apps\web\app\[lang]\for-venues\page.tsx` | Public | Venue prospect | Links to application login surface. |
| `/:lang/privacy` | `C:\dev\hostos\apps\web\app\[lang]\privacy\page.tsx` | Public | Guest/public | Policy page. |
| `/:lang/terms` | `C:\dev\hostos\apps\web\app\[lang]\terms\page.tsx` | Public | Guest/public | Terms page. |
| `/:lang/support` | `C:\dev\hostos\apps\web\app\[lang]\support\page.tsx` | Public | Guest/public | Support page. |
| `/auth/verified` | `C:\dev\hostos\apps\web\app\auth\verified\page.tsx` | Public utility route | Auth flow user | Email verification landing page with app/dashboard continuation links. |
| `/auth/reset-password` | `C:\dev\hostos\apps\web\app\auth\reset-password\page.tsx` | Public utility route | Auth flow user | Password recovery landing page. |
| `/` | `C:\dev\joinfolk-web\web\src\App.tsx` / `Home` | Public | Guest/public | Vite public web app home route. |
| `/e/:id` | `C:\dev\joinfolk-web\web\src\App.tsx` / `EventSharePage` | Public | Event viewer | Public event share route found. |
| `/claim/:token` | `C:\dev\joinfolk-web\web\src\App.tsx` / `ClaimPage` | Public | Claim flow user | Claim route found. |
| `/v/:token` | `C:\dev\joinfolk-web\web\src\App.tsx` / `Verify` | Public | Verification user | Verification route found. |
| `*` | `C:\dev\joinfolk-web\web\src\App.tsx` / `Home` | Public | Guest/public | Catch-all routes to home. |

### 5.2 Dashboard routes

| Route/path | File/component | Public/auth required | Host/tier/staff guard found? | Notes |
| --- | --- | --- | --- | --- |
| `/login` | `C:\dev\joinfolk-web\dashboard\src\pages\LoginPage.tsx` | Public | No dashboard guard | Login page signs in and navigates to saved return path when present. |
| `/` | `C:\dev\joinfolk-web\dashboard\src\App.tsx` / `TierLanding` | Auth required | `AuthGuard` + `HostGuard` | Dashboard root requires authenticated semi-pro/pro host tier; standard user is redirected to `/personal`. |
| `/personal` | `C:\dev\joinfolk-web\dashboard\src\App.tsx` / `PersonalHomePage` | Auth required | `AuthGuard` only | Personal account surface exists inside dashboard app. |
| `/events` | `C:\dev\joinfolk-web\dashboard\src\App.tsx` / `EventListPage` | Auth required | `AuthGuard` + `HostGuard` | Host dashboard event list. |
| `/events/new` | `C:\dev\joinfolk-web\dashboard\src\App.tsx` / `EventCreatePage` | Auth required | `AuthGuard` + `HostGuard` | Host event creation route. |
| `/events/:id/*` | `C:\dev\joinfolk-web\dashboard\src\App.tsx` / event detail tabs | Auth required | `AuthGuard` + `HostGuard`; route-level event-owner guard not found for all nested tabs | Event detail tabs include content, modules, attendees, invites, checklist, poll, gallery, tickets, products, venue, reservations, sessions, staff, analytics. |
| `/event-reservations/:id` | `C:\dev\joinfolk-web\dashboard\src\App.tsx` / `EventReservationsPage` | Auth required | `AuthGuard` + `HostGuard` + `EventOwnerGuard` | Route-level event owner guard found for this reservation detail path. |
| `/venues`, `/venues/new`, `/venues/:venueId/*` | `C:\dev\joinfolk-web\dashboard\src\App.tsx` / venue pages | Auth required | `AuthGuard` + `HostGuard` + `SemiProGuard` | Venue/reservation/offerings/media routes require semi-pro guard in route file. |
| `/reservations` | `C:\dev\joinfolk-web\dashboard\src\App.tsx` / `VenueReservationInboxPage` | Auth required | `AuthGuard` + `HostGuard` + `SemiProGuard` | Reservation inbox route. |
| `/ops/*` | `C:\dev\joinfolk-web\dashboard\src\App.tsx` / ops pages | Auth required | `AuthGuard` + `HostGuard` + `OpsGuard` | Ops routes require `isOps` flag. |

### 5.3 Public event/provider routes

| Route/path | File/component | Guest accessible? | Buy/reserve CTA path | Notes |
| --- | --- | --- | --- | --- |
| `/e/:id` | `C:\dev\joinfolk-web\web\src\App.tsx` / `EventSharePage` | Found | Unknown from route inventory | Public event share route exists in Vite public web app. |
| `/:lang` | `C:\dev\hostos\apps\web\app\[lang]\page.tsx` | Found | Application CTAs found | Marketing home includes app and host links; specific event/provider detail route not found in route inventory. |
| `/:lang/for-hosts` | `C:\dev\hostos\apps\web\app\[lang]\for-hosts\page.tsx` | Found | Login CTA found | Host acquisition page; not an event/provider detail page. |
| `/:lang/for-venues` | `C:\dev\hostos\apps\web\app\[lang]\for-venues\page.tsx` | Found | Login CTA found | Venue acquisition page; not an event/provider detail page. |
| Public provider detail route | Not found in inspected route files | Unknown | Unknown | No explicit provider detail route was found in inspected web route definitions. |
| Public reservation/ticket checkout route | Not found in inspected route files | Unknown | Unknown | No standalone public checkout/reservation route was proven from route definitions. |

### 5.4 Login/auth routes

| Route/path | File/component | Redirect behavior found? | Persona intent preserved? | Notes |
| --- | --- | --- | --- | --- |
| `/login` | `C:\dev\joinfolk-web\dashboard\src\pages\LoginPage.tsx` | Found | Partially supported | `AuthGuard` passes `state.from`; `LoginPage` reads `returnTo` and navigates there after sign-in. Explicit persona intent beyond route return was not found. |
| `/personal` | `C:\dev\joinfolk-web\dashboard\src\App.tsx` / `PersonalHomePage` | Found | Personal persona route found | Authenticated route does not require host tier. |
| Dashboard protected routes | `C:\dev\joinfolk-web\dashboard\src\components\AuthGuard.tsx` | Found | Dashboard intent via route return | Unauthenticated users are redirected to `/login` with `state.from`. |
| `/auth/verified` | `C:\dev\hostos\apps\web\app\auth\verified\page.tsx` | Found | Unknown | Auth utility route provides continuation links after verification. |
| `/auth/reset-password` | `C:\dev\hostos\apps\web\app\auth\reset-password\page.tsx` | Found | Unknown | Password recovery utility route. |
| Mobile sign-in | `C:\dev\hostos\apps\mobile\app\(auth)\sign-in.tsx` | Found | Mobile reference only | Mobile sign-in redirects to tab root after login; web dashboard intent preservation does not apply. |

### 5.5 Staff routes

| Route/path | File/component | Guard found? | Role required | Notes |
| --- | --- | --- | --- | --- |
| `/staff/login` | `C:\dev\joinfolk-web\dashboard\src\pages\staff\StaffLoginPage.tsx` | Public staff login | Staff account expected | Staff login signs in and navigates to `/staff`. |
| `/staff` | `C:\dev\joinfolk-web\dashboard\src\App.tsx` / `StaffLayout` + `StaffEventsPage` | `AuthGuard`; assignment query found | Event staff assignment evidence | `StaffEventsPage` queries `event_staff_assignments` for current user and lists assigned events. Route-level staff role guard remains less explicit than host/dashboard guard. |
| `/staff/scan/:eventId` | `C:\dev\joinfolk-web\dashboard\src\App.tsx` / `StaffScannerPage` | `AuthGuard`; page guidance found | Unknown at route level | Web scanner page guides staff to mobile scanner; route-level role enforcement was not proven by this inventory. |
| Mobile event scan route | `C:\dev\hostos\apps\mobile\app\(tabs)\event\[id]\scan.tsx` | Found in mobile reference | Host or assigned staff | Mobile code checks event host/staff assignment before scan flow; read-only reference only. |

## 6. Access Behavior Matrix

| Actor | Marketing web | Public event/provider | Personal app/profile | Host dashboard | Staff tools |
| --- | --- | --- | --- | --- | --- |
| Guest | Found: public marketing routes | Found for `/e/:id`; provider detail unknown | Unknown for app profile; auth utility routes found | Found blocked by `AuthGuard` redirect to login | Found public `/staff/login`; protected `/staff` blocked by `AuthGuard` |
| Normal authenticated user | Found | Unknown/Found depending route | Found: `/personal` requires auth only | Found blocked: `HostGuard` redirects `user` tier to `/personal` | Unknown: assignment-based staff access may allow assigned users, but route-level role guard is unclear |
| Semi-pro host | Found | Unknown/Found depending route | Found: `/personal` remains available | Found: `HostGuard` allows non-user tier; `SemiProGuard` allows semi-pro routes | Unknown unless assigned staff |
| Pro host | Found | Unknown/Found depending route | Found: `/personal` remains available | Found: `HostGuard` allows non-user tier; `SemiProGuard` appears to restrict some venue routes to semi-pro only | Unknown unless assigned staff |
| Staff scanner | Found | Unknown/Found depending route | Unknown | Unknown unless also host tier | Found: staff routes exist; assignment filtering found; mobile scanner has host/staff assignment checks |

## 7. Persona/Login Routing Findings

| Requirement | Current evidence | Status |
| --- | --- | --- |
| Same email can access personal and host persona | Dashboard uses one Supabase session/account; `HostGuard` gates dashboard by tier; `/personal` is auth-only. Mobile `persona.ts` models personal and host personas from account tier. | Supported with gaps |
| Login preserves dashboard intent | `AuthGuard` passes `state.from` to `/login`; `LoginPage` navigates to `returnTo` after successful sign-in. | Supported for route return |
| Login preserves public event/provider intent | Public route login CTA intent is not proven to preserve event/provider context across login. | Unknown/Gap |
| Normal users are blocked from dashboard | `HostGuard` redirects `tier === "user"` to `/personal`. | Supported |
| Host users can still use personal/user flow | `/personal` is protected only by `AuthGuard`; mobile persona model supports personal + host for semi-pro/pro tiers. | Supported |
| Staff route requires staff role | `StaffEventsPage` filters by `event_staff_assignments`; mobile scanner checks host/staff assignment. Web route-level staff role guard was not proven. | Supported with gaps |
| Cloudflare separation is not used as security boundary | Dashboard source contains app-level auth/tier/ops guards. Cloudflare project separation is documented as deployment ownership, not authorization. | Supported as architecture direction |

## 8. Gaps Identified

| Gap ID | Gap | Evidence | Severity | Required next decision/patch |
| --- | --- | --- | --- | --- |
| WA-01 | Public provider detail route and public reservation/ticket route are not proven in inspected web route definitions. | Marketing and public event share routes found; provider/checkout route not found. | P1: required before public launch | Public event/provider route decision |
| WA-02 | Public event/provider login intent preservation is unclear. | Dashboard login preserves protected-route return path, but public CTA/event-provider return intent was not proven. | P1: required before public launch | Login redirect/persona intent design |
| WA-03 | Web staff route-level authorization is less explicit than host/dashboard authorization. | `/staff` uses `AuthGuard` and assignment filtering; no standalone route-level staff guard was found. | P0 candidate: release blocker until dashboard authorization matrix proves route/page/backend coverage. | Dashboard authorization matrix |
| WA-04 | Event dashboard nested tab owner/authorization coverage is unclear at route level. | `EventOwnerGuard` found for `/event-reservations/:id`; broad `/events/:id/*` tabs are under host guard but not all under route-level event-owner guard. | P0 candidate: release blocker until event owner authorization coverage is proven across route, page, and RPC/data access layers. | Dashboard authorization matrix |
| WA-05 | Persona switch / persona-intent UX is split across web and mobile evidence. | Mobile has explicit persona model and switcher; dashboard has `/personal` and host tier guard, but explicit web persona switch IA was not proven. | P1: required before public launch | Persona login routing design |
| WA-06 | Semi-pro/pro route semantics need owner review. | `HostGuard` allows non-user tiers; `SemiProGuard` appears to require exact semi-pro for selected venue/reservation routes. | Unknown: needs more evidence | Dashboard authorization matrix |
| WA-07 | Cloudflare production deployment evidence remains a release gate. | Existing decision requires `joinfolk-dashboard-live` and production branch alignment for `app.join-folk.com`. | P1: required before public launch | Cloudflare production deploy evidence |

## 9. Blocked Implementation Claims

- This report does not authorize routing implementation.
- This report does not authorize dashboard auth changes.
- This report does not authorize DB/RLS/RPC changes.
- This report does not authorize Cloudflare changes.
- This report does not claim production launch readiness.
- This report does not claim dashboard authorization is secure unless code evidence proves it.

## 10. Required Next Gates

| Gate | Required before | Status |
| --- | --- | --- |
| Web Access IA decision update | Any route implementation | Required if gaps exist |
| Dashboard authorization matrix | Any dashboard access patch | Required |
| Public event/provider route decision | Any public SEO/web patch | Required |
| Login redirect/persona intent design | Any login routing patch | Required |
| Cloudflare production deploy evidence | Any production deploy | Required |

## 11. No-Modification Confirmation

- No application code was modified by this handbook task.
- No dashboard/mobile/web code was modified by this handbook task.
- No Supabase tree was modified by this handbook task.
- No SQL was executed by this handbook task.
- No production mutation was executed by this handbook task.
- No Cloudflare setting was changed by this handbook task.
- No Supabase CLI was run by this handbook task.
- No files were staged or committed by this handbook task.
- Only `07_Audits/WebAccessIAInventoryReport.md` was created.

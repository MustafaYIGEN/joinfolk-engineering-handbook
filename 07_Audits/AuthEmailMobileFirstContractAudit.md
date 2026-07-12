# Auth Email Mobile-First Contract Audit

## 1. Metadata

- Status: Draft
- Owner: Mustafa / JoinFolk
- Scope: AUTH-EMAIL-01-G1 canonical auth host and production configuration evidence
- Evidence date: 2026-07-12
- Source confidence: mixed live-host snapshot, repository inspection, handbook deployment evidence, and missing Dashboard-only evidence
- Canonical: false

## 2. Scope

This audit is bounded to the password-reset and email-confirmation contract.

Surfaces inspected:

- `C:\dev\hostos`
- `C:\dev\hostos\apps\mobile`
- `C:\dev\joinfolk-web`
- `C:\dev\joinfolk-engineering-handbook`
- Public host snapshots for:
  - `join-folk.com`
  - `www.join-folk.com`
  - `app.join-folk.com`

This audit does not authorize implementation, deployment, Supabase mutation, Cloudflare mutation, or AASA changes.

## 3. Approved Product Contract

The approved target contract requires:

- password reset MUST open native mobile reset flow when the iOS app is installed
- password entry and update MUST happen on native mobile
- app-absent flows MUST have safe web fallback
- email confirmation MUST open a native mobile result surface when the app is installed
- unrelated `/login` and `/download` routes MUST remain browser-only
- auth links MUST NOT expose tokens in user-visible errors, logs, analytics, or crash surfaces
- one canonical auth-link host and one canonical route contract per action MUST exist

## 4. Repository Evidence

### 4.1 Platform web auth ownership

PROVEN:

- `C:\dev\hostos\apps\web\app\auth\reset-password\page.tsx` owns `/auth/reset-password`
- `C:\dev\hostos\apps\web\app\auth\verified\page.tsx` owns `/auth/verified`
- `C:\dev\hostos\apps\web\app\page.tsx` and `components/RootAuthBridge.tsx` route recovery/signup indicators into those paths

### 4.2 Mobile auth ownership

PROVEN:

- Native reset route exists at `C:\dev\hostos\apps\mobile\app\reset-password.tsx`
- Native confirmation route was not found
- Mobile auth stack includes `sign-in`, `sign-up`, `forgot-password`, `terms`, and `privacy`, but not a confirmation result route

### 4.3 Secondary duplicate auth surface

PROVEN:

- `C:\dev\joinfolk-web\app\(auth)\forgot-password.tsx`
- `C:\dev\joinfolk-web\app\(auth)\sign-up.tsx`
- `C:\dev\joinfolk-web\app\(auth)\reset-password.tsx`
- `C:\dev\joinfolk-web\app\index.tsx`

These duplicate the auth-email surface and therefore weaken route-contract determinism.

## 5. Live Host Snapshot

### 5.1 Auth route snapshot

| URL | Live evidence | Classification |
| --- | --- | --- |
| `https://join-folk.com/auth/reset-password` | HTML page opens and is titled `JoinFolk — Email Verified`; live HTTP chain not fully observable from current tool boundary | PROVEN_PRESENT / LIVE_CHAIN_BLOCKED |
| `https://join-folk.com/auth/verified` | HTML page opens and is titled `JoinFolk — Email Verified` | PROVEN_PRESENT |
| `https://www.join-folk.com/auth/verified` | HTML page opens and visibly renders an email-verified surface with app-open and dashboard continuation links | PROVEN_PRESENT |
| `https://www.join-folk.com/auth/reset-password` | direct live fetch chain not fully recovered from current environment | NOT_PROVEN |
| `https://app.join-folk.com/auth/reset-password` | returns `200 OK` HTML and renders the organizer dashboard shell (`JoinFolk — Organizer Dashboard`) | PROVEN_BROKEN |
| `https://app.join-folk.com/auth/verified` | returns `200 OK` HTML and renders the organizer dashboard shell (`JoinFolk — Organizer Dashboard`) | PROVEN_BROKEN |

### 5.2 AASA snapshot

| URL | Live evidence | Classification |
| --- | --- | --- |
| `https://join-folk.com/.well-known/apple-app-site-association` | public fetch returned `404 Not Found` | PROVEN_BROKEN |
| `https://www.join-folk.com/.well-known/apple-app-site-association` | public fetch returned `404 Not Found` | PROVEN_BROKEN |
| `https://app.join-folk.com/.well-known/apple-app-site-association` | returns `200 OK` HTML organizer dashboard shell instead of valid AASA JSON | PROVEN_BROKEN |

The exact live redirect chain, status code history, and response headers for all nine URLs remain partially blocked by the current read-only network/tool boundary.

## 6. Cloudflare Ownership Evidence

### 6.1 Handbook production evidence

PROVEN from handbook records:

- `joinfolk-web` Cloudflare Pages project owns `join-folk.com` and `www.join-folk.com`
- `joinfolk-dashboard-live` Cloudflare Pages project owns `app.join-folk.com`
- `joinfolk-dashboard-live` production source is `MustafaYIGEN/joinfolk-web`, branch `refactor/joinfolk-stabilization-p0`, root `dashboard`, build output `dist`

Source documents:

- `00_Status/CloudflareGitBackedProductionDeployEvidenceReport.md`
- `09_Decisions/CloudflareProductionSurfaceRoutingDecision.md`
- `09_Decisions/WebAccessPersonaRoutingDecision.md`

### 6.2 Deployment conflict

CONFLICT:

- `join-folk.com` and `www.join-folk.com` are governed as marketing/root web domains
- `app.join-folk.com` is governed as the production application/dashboard surface
- the current auth routes live in `hostos/apps/web`, while the current production `app.join-folk.com` evidence points to `joinfolk-web/dashboard`
- therefore live route ownership for `/auth/reset-password` and `/auth/verified` is not frozen to one production project from source evidence alone

## 7. Mobile Associated-Domain Evidence

Committed mobile configuration:

- bundle identifier: `com.joinfolk.app`
- custom scheme: `joinfolk`
- associated domains:
  - `applinks:app.join-folk.com`

Classification:

| Item | State |
| --- | --- |
| Current entitlement compatible with `app.join-folk.com` | PROVEN |
| Current entitlement compatible with `join-folk.com` | MISSING |
| Current entitlement compatible with `www.join-folk.com` | MISSING |
| Canonical host frozen for auth email links | BLOCKED |
| Build/TestFlight required after any associated-domain change | PROVEN |

## 8. Source Conflict Matrix

| Conflict | Evidence | State |
| --- | --- | --- |
| Committed mobile reset redirect was custom-scheme | mobile git baseline for `forgot-password.tsx` at commit `2f3d1fe` | PROVEN |
| Unstaged mobile reset redirect now points to `https://join-folk.com/auth/reset-password` | pre-existing worktree diff in `apps/mobile/app/(auth)/forgot-password.tsx` | PROVEN |
| Signup confirmation redirect points to `https://join-folk.com/auth/verified` | `apps/mobile/app/(auth)/sign-up.tsx` | PROVEN |
| Native reset route exists | `apps/mobile/app/reset-password.tsx` | PROVEN |
| Native confirmation route missing | no mobile `confirm-email` or `verified` route found | PROVEN_BROKEN |
| Web reset page still performs browser token exchange and browser password update | `apps/web/app/auth/reset-password/page.tsx` | PROVEN_BROKEN |
| Web verification page is a success-style page, not a stateful confirmation resolver | `apps/web/app/auth/verified/page.tsx` | PROVEN_BROKEN |
| Duplicate auth surfaces exist in `joinfolk-web` | `joinfolk-web/app/(auth)` and `joinfolk-web/app/index.tsx` | PROVEN_BROKEN |
| Source-controlled AASA excludes `/auth/*` | both AASA candidates | PROVEN_BROKEN |
| Associated-domain host does not match current auth-link host used in mobile code | `app.join-folk.com` vs `join-folk.com` | PROVEN_BROKEN |

## 9. Supabase Dashboard Evidence Requirement

DASHBOARD_EVIDENCE_REQUIRED:

- `Auth > URL Configuration > Site URL`
- `Auth > URL Configuration > Additional Redirect URLs`
- presence/absence of:
  - `joinfolk://reset-password`
  - `https://join-folk.com/auth/reset-password`
  - `https://www.join-folk.com/auth/reset-password`
  - `https://app.join-folk.com/auth/reset-password`
  - `https://join-folk.com/auth/verified`
  - `https://www.join-folk.com/auth/verified`
  - `https://app.join-folk.com/auth/verified`
- `Auth > Email Templates > Confirm signup` subject, CTA label, route pattern, branding shape
- `Auth > Email Templates > Reset password` subject, CTA label, route pattern, branding shape
- `Auth > Email Templates > Magic Link` enabled/disabled and route behavior
- `Auth > Hooks` whether any auth hook changes confirmation or recovery routing

Until this evidence is captured, redirect allowlist and template alignment remain unproven.

## 10. Host Decision Result

Result: `BLOCKED_DEPLOYMENT_CONFLICT`

Reason:

- `join-folk.com` has live auth routes, but live AASA is `404` and mobile does not currently declare it
- `www.join-folk.com` has at least one live auth page, but live AASA is `404` and mobile does not currently declare it
- `app.join-folk.com` is the only currently entitled mobile host, but its live auth routes and AASA path currently render organizer dashboard HTML instead of an end-user auth recovery surface and valid AASA JSON
- Supabase redirect allowlist and email template routing are Dashboard-only and currently unverified
- Cloudflare project ownership proves a production split between marketing/root web and application/dashboard domains, but not a frozen canonical auth-link host

## 11. Required Next Gate

AUTH-EMAIL implementation is BLOCKED UNTIL:

1. Dashboard URL configuration is recorded
2. Dashboard email template routing is recorded
3. live `app.join-folk.com` auth-route/AASA deployment conflict is resolved or ruled out as the canonical host
4. one canonical auth-link host is frozen against both Cloudflare ownership and mobile entitlements
5. route-specific AASA policy is designed so `/login` and `/download` remain browser-only

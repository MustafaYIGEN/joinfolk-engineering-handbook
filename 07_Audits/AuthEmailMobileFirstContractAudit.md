# Auth Email Mobile-First Contract Audit

## 1. Metadata

- Status: Draft
- Owner: Mustafa / JoinFolk
- Scope: AUTH-EMAIL-01 browser-first auth-email contract reconciliation and rollout-state evidence
- Evidence date: 2026-07-12
- Source confidence: mixed live-host snapshot, repository inspection, handbook deployment evidence, Dashboard production evidence, and operator UAT
- Canonical: false

## 2. Scope

This audit is bounded to password-reset and email-confirmation email-link behavior.

Historical note:

- the filename remains `MobileFirst` for continuity
- the binding architecture recorded by this audit is browser-first

Surfaces inspected:

- `C:\dev\hostos`
- `C:\dev\hostos\apps\mobile`
- `C:\dev\joinfolk-web`
- `C:\dev\joinfolk-engineering-handbook`
- live public auth hosts:
  - `join-folk.com`
  - `www.join-folk.com`
  - `app.join-folk.com`
- Supabase Dashboard production configuration evidence
- operator email-template UAT evidence

This audit does not authorize deployment, Supabase mutation, Cloudflare mutation, or application-code edits.

## 3. Accepted Product Contract

The accepted JoinFolk auth-email contract is now browser-first.

Binding rules:

- password-reset email links MUST complete recovery in the browser
- password entry and password update MUST occur on the public web reset page
- the reset flow MUST NOT automatically request opening the mobile app
- after success, the page MAY offer explicit `Open JoinFolk` or `Sign in` CTAs
- the native reset route MAY remain temporarily for legacy emails and installed-client compatibility
- email confirmation MUST complete on the public web verified page
- the verified page MUST visibly state that the email is confirmed
- a native confirmation result route is `NOT_REQUIRED_BY_DECISION`
- `/auth/*`, `/login*`, and `/download*` MUST remain browser-only
- AASA MUST NOT capture password-reset or confirmation routes

## 4. Repository Evidence

### 4.1 Platform web auth ownership

PROVEN:

- `C:\dev\hostos\apps\web\app\auth\reset-password\page.tsx` owns `/auth/reset-password`
- `C:\dev\hostos\apps\web\app\auth\verified\page.tsx` owns `/auth/verified`
- `C:\dev\hostos\apps\web\app\page.tsx` and `components/RootAuthBridge.tsx` route recovery/signup indicators into those paths

### 4.2 Mobile auth ownership

PROVEN:

- native reset route exists at `C:\dev\hostos\apps\mobile\app\reset-password.tsx`
- native confirmation route was not found
- under the accepted browser-first contract, native confirmation is no longer required
- native reset route is now `LEGACY_COMPATIBILITY_ONLY`
- canonical reset request redirect was implemented in mobile commit `43d909a`
- recovery/onboarding isolation was implemented in mobile commit `5912d60`
- a successful iOS build containing both mobile auth commits exists:
  - version: `1.0.0 (24)`
  - build commit: `4b7fb0e`
- token-bearing browser reset UAT remains `OPEN`
- legacy native recovery regression UAT remains `OPEN`
- normal onboarding regression UAT remains `OPEN`

### 4.3 Secondary duplicate auth surface

PROVEN:

- `C:\dev\joinfolk-web\app\(auth)\forgot-password.tsx`
- `C:\dev\joinfolk-web\app\(auth)\sign-up.tsx`
- `C:\dev\joinfolk-web\app\(auth)\reset-password.tsx`
- `C:\dev\joinfolk-web\app\index.tsx`

These remain rollout-dependent cleanup candidates.

## 5. Historical Live Host Snapshot

### 5.1 Auth route snapshot

This snapshot records the pre-browser-first production defect state that justified the browser-first closure wave. It is historical evidence, not the current binding rollout state.

| URL | Live evidence | Classification |
| --- | --- | --- |
| `https://join-folk.com/auth/reset-password` | `200 OK`, no redirect, final host `join-folk.com`, renders `JoinFolk — Email Verified`; wrong surface for password reset | PROVEN_BROKEN |
| `https://www.join-folk.com/auth/reset-password` | `200 OK`, no redirect, final host `www.join-folk.com`, renders `JoinFolk — Email Verified`; wrong surface for password reset | PROVEN_BROKEN |
| `https://app.join-folk.com/auth/reset-password` | `200 OK`, no redirect, renders organizer dashboard shell; not a suitable end-user auth fallback | PROVEN_DEPLOYMENT_CONFLICT |
| `https://join-folk.com/auth/verified` | `200 OK`, no redirect, final host `join-folk.com`; web verification fallback surface exists | PARTIAL |
| `https://www.join-folk.com/auth/verified` | `200 OK`, no redirect, final host `www.join-folk.com`; web verification fallback surface exists | PARTIAL |
| `https://app.join-folk.com/auth/verified` | `200 OK`, no redirect, renders organizer dashboard shell; not a suitable end-user auth fallback | PROVEN_DEPLOYMENT_CONFLICT |

### 5.2 AASA snapshot

| URL | Live evidence | Classification |
| --- | --- | --- |
| `https://join-folk.com/.well-known/apple-app-site-association` | `404 Not Found`; missing | PROVEN_BROKEN |
| `https://www.join-folk.com/.well-known/apple-app-site-association` | `404 Not Found`; missing | PROVEN_BROKEN |
| `https://app.join-folk.com/.well-known/apple-app-site-association` | `200 OK`, `text/html`, organizer dashboard shell instead of valid AASA JSON | PROVEN_DEPLOYMENT_CONFLICT |

Under the accepted browser-first contract, missing AASA remains a separate deep-link deployment gap. It does not block browser-only auth-email completion.

### 5.3 Canonical email-logo asset

PROVEN:

- local source: `C:\dev\hostos\apps\web\public\joinfolk-email-symbol-512-transparent.png`
- file type: PNG
- dimensions: `512 x 512`
- byte length: `61114`
- canonical live URL: `https://join-folk.com/joinfolk-email-symbol-512-transparent.png`
- live verification:
  - `200 OK`
  - `Content-Type: image/png`
  - `Content-Length: 61114`
  - no redirect

The same asset also resolves on `www`, but email templates MUST use the canonical non-`www` URL.

## 6. Production Configuration Evidence

### 6.1 Supabase URL configuration

PROVEN:

- Site URL: `https://join-folk.com`
- exact canonical auth-email routes are allowlisted:
  - `https://join-folk.com/auth/verified`
  - `https://join-folk.com/auth/reset-password`
  - `https://www.join-folk.com/auth/verified`
  - `https://www.join-folk.com/auth/reset-password`
- legacy/mobile compatibility entries remain present:
  - `joinfolk:///*`
  - `joinfolk://reset-password`

PROVEN / CLEANUP_REQUIRED:

- broad wildcard redirect coverage exists for:
  - `https://join-folk.com/**`
  - `https://www.join-folk.com/**`
  - `https://app.join-folk.com/*`
  - `https://app.join-folk.com/**`

### 6.2 Auth hooks

PROVEN:

- no auth hooks are configured
- no hook currently changes confirmation or recovery routing

### 6.3 Email templates

PROVEN:

- Confirm signup template saved
- Reset password template saved
- Magic Link template saved
- all use `{{ .ConfirmationURL }}`
- all use the verified canonical logo asset URL:
  - `https://join-folk.com/joinfolk-email-symbol-512-transparent.png`

Recorded subjects:

- Confirm signup: `Confirm your JoinFolk email`
- Reset password: `Reset your JoinFolk password`
- Magic Link: `Your JoinFolk magic link`

Magic Link product usage remains `NOT_PROVEN`.

## 7. UAT Evidence

### 7.1 Confirm signup email

- email received: `PASS`
- verified JoinFolk logo visible: `PASS`
- Verify email address CTA visible: `PASS`
- CTA clickable: `PASS`
- fallback URL visible: `PASS`
- Gmail rendering: `PASS`
- iPhone Mail functional layout: `PASS`
- iPhone Mail heading contrast: `VISUAL_FIX_REQUIRED`
- confirmation flow behavior: `PASS`

### 7.2 Browser-first rollout state

PROVEN:

- public auth host: `https://join-folk.com`
- password reset route: `/auth/reset-password`
- confirmation route: `/auth/verified`
- browser-first production web implementation commit: `0df2562a`
- new password-reset requests target the canonical HTTPS reset route
- web-only password reset is the binding contract

OPEN:

- token-bearing browser reset production UAT
- legacy native recovery regression UAT
- normal onboarding regression UAT

### 7.3 Email delivery and sender domain evidence

PROVEN:

- `CUSTOM_SMTP: PASS`
- `SPF: PASS`
- `DKIM: PASS`
- `DMARC: PASS_MONITORING_POLICY`
- `DMARC_POLICY: p=none`
- `AUTH_EMAIL_TEMPLATES: PASS`
- `AUTH_CONFIRMATION_EMAIL_UAT: PASS`

OPEN / NON-BLOCKING:

- `SOURCE_CONTROLLED_EMAIL_TEMPLATES: OPEN_REPRODUCIBILITY_GAP`
- `SENDER_AVATAR_BIMI_APPLE: DEFERRED_NON_BLOCKING`

## 8. Host and Surface Classification

- `AUTH_EMAIL_FLOW_MODEL: BROWSER_FIRST`
- `AUTH_EMAIL_TARGET_HOST: DECIDED_JOIN_FOLK_COM`
- `AUTH_EMAIL_HOST_READINESS: PARTIAL`
- `PASSWORD_RESET_TARGET_SURFACE: WEB_ONLY`
- `PASSWORD_RESET_WEB_IMPLEMENTATION: DEPLOYED`
- `PASSWORD_RESET_WEB_PRODUCTION_UAT: OPEN`
- `PASSWORD_RESET_MOBILE_REQUEST_REDIRECT: IMPLEMENTED_PENDING_NEW_BINARY_UAT`
- `PASSWORD_RESET_NATIVE_ROUTE: LEGACY_COMPATIBILITY_ONLY`
- `RECOVERY_ONBOARDING_ISOLATION: IMPLEMENTED_PENDING_NEW_BINARY_UAT`
- `AUTH_CONFIRMATION_SURFACE: WEB_FIRST_PASS`
- `EMAIL_CONFIRMATION_NATIVE_ROUTE: NOT_REQUIRED_BY_DECISION`
- `AUTH_AASA_POLICY: AUTH_ROUTES_BROWSER_ONLY`
- `CUSTOM_SMTP: PASS`
- `SPF: PASS`
- `DKIM: PASS`
- `DMARC: PASS_MONITORING_POLICY`
- `DMARC_POLICY: p=none`
- `AUTH_EMAIL_TEMPLATES: PASS`
- `AUTH_EMAIL_LOGO_ASSET: PASS`
- `AUTH_CONFIRMATION_EMAIL_UAT: PASS`
- `AUTH_APP_DOMAIN_FALLBACK: REJECTED_AS_CURRENT_AUTH_EMAIL_HOST`
- `SOURCE_CONTROLLED_EMAIL_TEMPLATES: OPEN_REPRODUCIBILITY_GAP`
- `SENDER_AVATAR_BIMI_APPLE: DEFERRED_NON_BLOCKING`

## 9. Remaining Open Gates

OPEN:

- token-bearing browser reset production UAT on `https://join-folk.com/auth/reset-password`
- legacy native recovery regression UAT
- normal onboarding regression UAT

RISK:

- source-controlled email templates remain an open reproducibility gap

DEFERRED:

- sender avatar / BIMI / Apple Branded Mail is non-blocking and deferred

## 10. Rollout-Dependent Cleanup

Cleanup is blocked until reset and confirmation UAT pass and installed clients are verified.

Later cleanup candidates:

- `joinfolk://reset-password`
- broad `joinfolk:///*`
- duplicate exact and wildcard redirect coverage
- `app.join-folk.com` auth redirect entries
- duplicate `joinfolk-web` auth surfaces
- unused Magic Link product surface
- legacy mobile reset route

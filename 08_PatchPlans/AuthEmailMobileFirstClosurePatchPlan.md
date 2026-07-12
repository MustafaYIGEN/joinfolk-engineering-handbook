# Auth Email Mobile-First Closure Patch Plan

## 1. Metadata

- Status: Active draft
- Owner: Mustafa / JoinFolk
- Scope: password-reset and email-confirmation browser-first contract closure
- Depends on: `09_Decisions/AuthEmailCanonicalLinkHostDecision.md`
- Canonical: false

## 2. Current Gate

Implementation is AUTHORIZED_UNDER_ACCEPTED_PATCH_PLAN, but rollout remains open until browser reset UAT and legacy/mobile regression proof are complete.

## 3. Preconditions

Before closure:

1. canonical auth-link host remains `join-folk.com`
2. Supabase redirect allowlist remains verified
3. browser-first reset implementation remains deployed on the canonical web route
4. token-bearing browser reset UAT completes
5. legacy native recovery regression UAT completes
6. normal onboarding regression UAT completes
7. duplicate auth surface strategy remains tracked for `joinfolk-web`
8. `app.join-folk.com` remains excluded as the current auth email host unless a separate production-surface decision reopens it

## 4. Accepted Closure Scope

The accepted closure scope is:

1. preserve the accepted `join-folk.com` browser-first target-host contract
2. preserve `www.join-folk.com` as browser fallback during rollout
3. keep password reset web-only on `/auth/reset-password`
4. keep email confirmation web-first on `/auth/verified`
5. preserve native reset as `LEGACY_COMPATIBILITY_ONLY`
6. preserve recovery/onboarding isolation for legacy native recovery
7. verify token-bearing browser reset behavior and browser-side password update
8. verify legacy native recovery and normal onboarding regressions on the new mobile binary
9. correct email-template reproducibility when a source-controlled template authority is accepted
10. perform redirect cleanup only after UAT and installed-client verification

## 5. Verification Categories

Do not record secrets.

Verification must cover:

- browser reset: valid token-bearing recovery link
- browser reset: expired recovery link
- browser reset: invalid recovery link
- browser reset: already-used recovery link where distinguishable
- browser reset: offline/network failure
- browser reset: password update succeeds without automatic app opening
- browser reset: success page exposes only explicit user-controlled continuation CTAs
- installed iOS app: legacy native reset route does not redirect into onboarding during recovery
- installed iOS app: normal onboarding still opens after intentional sign-in when appropriate
- app absent: canonical web fallback works
- valid confirmation
- already-confirmed confirmation
- expired confirmation
- invalid confirmation
- `/auth/*` remains browser-only
- `/login*` remains browser-only
- `/download*` remains browser-only
- tokens are not exposed in logs or user-facing raw errors

## 6. Rollback Boundary

Rollback may restore:

- prior browser reset implementation if a production regression is proven
- prior mobile reset request behavior only under an explicit legacy rollback decision
- prior browser fallback ownership
- prior email template redirects

Rollback MUST NOT silently leave a mixed host contract in place.

## 7. Cleanup Register

Cleanup is BLOCKED until reset and confirmation UAT pass and installed clients are verified.

Later cleanup candidates:

- `joinfolk://reset-password`
- broad `join-folk.com` wildcard redirect entries
- broad `www.join-folk.com` wildcard redirect entries
- `app.join-folk.com` auth redirect entries
- duplicate exact and wildcard redirect coverage
- Magic Link template if product usage remains absent
- duplicate auth surfaces in `joinfolk-web`

## 8. Prohibited Actions While Blocked

- do not modify auth behavior by assumption
- do not treat AASA for `/claim/*` or `/e/*` as part of the auth-email blocker set
- do not remove broad redirect coverage before UAT and installed-client verification
- do not ship a native confirmation result route under this browser-first decision
- do not route auth email links to `app.join-folk.com` under the current architecture

# Auth Email Mobile-First Closure Patch Plan

## 1. Metadata

- Status: Active draft
- Owner: Mustafa / JoinFolk
- Scope: password-reset and email-confirmation mobile-first contract closure
- Depends on: `09_Decisions/AuthEmailCanonicalLinkHostDecision.md`
- Canonical: false

## 2. Current Gate

Implementation is AUTHORIZED_UNDER_ACCEPTED_PATCH_PLAN, but readiness remains blocked until the target host contract is implemented.

## 3. Preconditions

Before rollout completion:

1. canonical auth-link host decided as `join-folk.com`
2. Supabase redirect allowlist verified
3. email template routes verified
4. live AASA verified on the chosen public host
5. mobile associated-domain delta identified
6. exact web fallback owner identified
7. duplicate auth surface strategy decided for `joinfolk-web`
8. `app.join-folk.com` remains excluded as the current auth email host unless a separate production-surface decision reopens it

## 4. Accepted Closure Scope

The accepted closure scope is:

1. apply the accepted `join-folk.com` target-host contract
2. preserve `www.join-folk.com` as browser fallback during rollout
3. implement route ownership and fallback contract on the selected web surface
4. update mobile associated domains if required
5. implement native confirmation result route
6. normalize mobile reset result states and success surface
7. revise AASA to route-specific auth includes while keeping `/login` and `/download` browser-only
8. update email templates and redirect allowlist to match the selected host
9. verify installed-app, not-installed, expired, invalid, already-used, and offline states
10. perform redirect cleanup only after UAT and installed-client verification

## 5. Verification Categories

Do not record secrets.

Verification must cover:

- installed iOS app: reset opens native route
- installed iOS app: confirmation opens native route
- app absent: web fallback works
- expired reset
- already-used reset
- invalid reset
- valid confirmation
- already-confirmed confirmation
- expired confirmation
- invalid confirmation
- offline/network failure
- `/login` remains browser-only
- `/download` remains browser-only
- tokens are not exposed in logs or user-facing raw errors

## 6. Rollback Boundary

Rollback may restore:

- prior AASA exclusions
- prior associated-domain host set
- prior browser fallback ownership
- prior email template redirects

Rollback MUST NOT silently leave a mixed host contract in place.

## 7. Cleanup Register

Cleanup is BLOCKED until reset and confirmation UAT pass and installed clients are verified.

Later cleanup candidates:

- broad `join-folk.com` wildcard redirect entries
- broad `www.join-folk.com` wildcard redirect entries
- `app.join-folk.com` auth redirect entries
- duplicate exact and wildcard redirect coverage
- Magic Link template if product usage remains absent
- duplicate auth surfaces in `joinfolk-web`

## 8. Prohibited Actions While Blocked

- do not modify auth behavior by assumption
- do not change AASA before host freeze
- do not change associated domains before host freeze
- do not remove broad redirect coverage before UAT and installed-client verification
- do not ship a native confirmation result route that is not connected to the final host contract
- do not route auth email links to `app.join-folk.com` under the current architecture

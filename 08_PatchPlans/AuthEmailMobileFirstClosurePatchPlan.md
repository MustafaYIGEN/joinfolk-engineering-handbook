# Auth Email Mobile-First Closure Patch Plan

## 1. Metadata

- Status: Blocked draft
- Owner: Mustafa / JoinFolk
- Scope: password-reset and email-confirmation mobile-first contract closure
- Depends on: `09_Decisions/AuthEmailCanonicalLinkHostDecision.md`
- Canonical: false

## 2. Current Gate

Implementation is BLOCKED UNTIL the public canonical host, fallback routes, AASA, redirects, and templates are frozen.

## 3. Preconditions

Before any implementation:

1. canonical auth-link host decided
2. Supabase redirect allowlist verified
3. email template routes verified
4. live AASA verified on the chosen public host
5. mobile associated-domain delta identified
6. exact web fallback owner identified
7. duplicate auth surface strategy decided for `joinfolk-web`
8. `app.join-folk.com` remains excluded as the current auth email host unless a separate production-surface decision reopens it

## 4. Intended Closure Scope

When unblocked, the smallest safe closure scope is:

1. freeze one canonical host for reset and confirmation
2. add route-specific AASA support for:
   - `/auth/reset-password`
   - `/auth/verified` or final confirmation route
3. preserve browser-only handling for:
   - `/login`
   - `/download`
4. preserve safe web fallback when app is absent
5. keep password entry/update on native mobile when app is installed
6. add native mobile confirmation-result route and state handling
7. remove raw provider error strings from user-visible auth-email flows
8. align email templates with the final route contract
9. retire or isolate duplicate auth surfaces that conflict with the canonical contract

## 5. Ordered Execution Plan

1. capture missing Dashboard URL and email-template evidence
2. accept canonical public host decision between `join-folk.com` and `www.join-folk.com`
3. implement route ownership and fallback contract on the selected web surface
4. update mobile associated domains if required
5. implement native confirmation result route
6. normalize mobile reset result states and success surface
7. revise AASA to route-specific auth includes while keeping `/login` and `/download` browser-only
8. update email templates and redirect allowlist to match the selected host
9. verify installed-app, not-installed, expired, invalid, already-used, and offline states

## 6. Verification Categories

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

## 7. Rollback Boundary

Rollback may restore:

- prior AASA exclusions
- prior associated-domain host set
- prior browser fallback ownership
- prior email template redirects

Rollback MUST NOT silently leave a mixed host contract in place.

## 8. Prohibited Actions While Blocked

- do not modify auth behavior by assumption
- do not change AASA before host freeze
- do not change associated domains before host freeze
- do not change Supabase redirects before Dashboard evidence is captured
- do not ship a native confirmation result route that is not connected to the final host contract
- do not route auth email links to `app.join-folk.com` under the current architecture

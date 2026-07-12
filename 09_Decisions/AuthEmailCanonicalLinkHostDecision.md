# Auth Email Canonical Link Host Decision

## 1. Metadata

- Status: Active
- Owner: Mustafa / JoinFolk
- Decision type: auth-link host / browser-first auth-email contract
- Scope: password-reset and email-confirmation links only
- Evidence date: 2026-07-12
- Canonical: true for target-host selection and flow model

## 2. Decision Outcome

- `AUTH_EMAIL_FLOW_MODEL: BROWSER_FIRST`
- `AUTH_EMAIL_TARGET_HOST: DECIDED_JOIN_FOLK_COM`
- `AUTH_EMAIL_HOST_READINESS: PARTIAL`

## 3. Accepted Contract

Canonical public auth host:

- `https://join-folk.com`

Canonical password-reset route:

- `https://join-folk.com/auth/reset-password`

Canonical email-confirmation route:

- `https://join-folk.com/auth/verified`

Binding rules:

- password-reset email links MUST complete recovery in the browser
- password entry and password update MUST occur on the public web reset page
- the reset flow MUST NOT automatically request opening the mobile app
- after success, the page MAY offer explicit `Open JoinFolk` or `Sign in` CTAs
- email confirmation MUST complete on the public web verified page
- the page MUST visibly state that the email is confirmed
- native email confirmation is `NOT_REQUIRED_BY_DECISION`

## 4. Host Classification

### `join-folk.com`

- Supabase Site URL: `https://join-folk.com`
- exact canonical auth-email routes are allowlisted
- canonical email asset host: yes
- result: accepted canonical auth-email host

### `www.join-folk.com`

- exact auth-email routes are allowlisted
- accepted as browser fallback / compatibility surface only
- result: not canonical, but allowed during rollout

### `app.join-folk.com`

- organizer/dashboard production surface
- live auth routes render organizer dashboard HTML
- live AASA response is invalid HTML instead of valid JSON
- result: `REJECTED_AS_CURRENT_AUTH_EMAIL_HOST`

## 5. Readiness Gaps

Host selection is decided. Readiness is not complete.

Current readiness gaps:

- `join-folk.com/auth/reset-password` still renders the wrong surface
- browser reset flow still needs implementation and verification
- reset flow currently attempts mobile-app opening and this MUST be removed
- live `join-folk.com` AASA is still missing, but this is not a blocker for browser-only auth-email completion
- iPhone Mail heading contrast requires visual correction

Native confirmation route is no longer a blocker.

## 6. Binding Browser-Only Route Policy

- `/auth/*` MUST remain browser-only
- `/login*` MUST remain browser-only
- `/download*` MUST remain browser-only
- AASA MUST NOT capture password-reset or confirmation routes
- `/claim/*` and `/e/*` remain part of a separate universal-link contract
- `applinks:join-folk.com` is NOT an AUTH-EMAIL requirement under this decision

## 7. Cleanup Register

Cleanup is BLOCKED until reset and confirmation UAT pass and installed clients are verified.

Later cleanup candidates:

- `joinfolk://reset-password`
- broad `joinfolk:///*`
- broad `join-folk.com` wildcard redirect entries
- broad `www.join-folk.com` wildcard redirect entries
- `app.join-folk.com` auth redirect entries
- duplicate exact and wildcard redirect coverage
- duplicate auth surfaces in `joinfolk-web`
- unused Magic Link product surface
- legacy mobile reset route
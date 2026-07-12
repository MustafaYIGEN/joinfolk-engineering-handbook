# Auth Email Status Gates

## 1. Metadata

- Status: Draft
- Owner: Mustafa / JoinFolk
- Scope: AUTH-EMAIL-01 canonical host and mobile-first closure
- Canonical: false

## 2. Gate Record

| Gate | State | Notes |
| --- | --- | --- |
| AUTH_EMAIL_CURRENT_CONTRACT | FAIL | Reset and confirmation flows are split across custom scheme, web utility routes, duplicate surfaces, and browser-first behavior. |
| AUTH_EMAIL_CANONICAL_HOST | BLOCKED_DEPLOYMENT_CONFLICT | No public host is accepted yet. `app.join-folk.com` is rejected as the current auth email host, and public-host evidence is still incomplete. |
| AUTH_PUBLIC_RESET_FALLBACK | PROVEN_BROKEN | `join-folk.com` and `www.join-folk.com` reset routes render the wrong surface. |
| AUTH_PUBLIC_CONFIRMATION_FALLBACK | PARTIAL | `join-folk.com` and `www.join-folk.com` expose a web confirmation fallback surface, but canonical host policy is not frozen. |
| AUTH_APP_DOMAIN_FALLBACK | REJECTED_AS_CURRENT_AUTH_EMAIL_HOST | `app.join-folk.com` is an organizer/dashboard surface and currently serves invalid AASA HTML. |
| AUTH_LIVE_AASA | FAIL | Public hosts are missing live AASA and `app.join-folk.com` serves HTML instead of valid JSON. |
| PASSWORD_RESET_NATIVE_ROUTE | PARTIAL | Native reset route exists, but route ownership and selected email-link host are not frozen. |
| EMAIL_CONFIRMATION_NATIVE_ROUTE | MISSING | No native mobile confirmation result route was found. |
| AUTH_AASA_ALIGNMENT | FAIL | Source-controlled AASA excludes `/auth/*` on active candidates while product requires mobile-first reset and confirmation. |
| AUTH_REDIRECT_ALLOWLIST | DASHBOARD_EVIDENCE_REQUIRED | URL configuration evidence is not yet captured. |
| AUTH_EMAIL_TEMPLATES | DASHBOARD_EVIDENCE_REQUIRED | Template subject/CTA/route evidence is not yet captured. |
| AUTH_EMAIL_IMPLEMENTATION | BLOCKED UNTIL public canonical host, fallback routes, AASA, redirects and templates are frozen | No auth-email implementation work should proceed before host and redirect evidence is complete. |

## 3. Open Gates

Open evidence gates:

- public canonical-host decision between `join-folk.com` and `www.join-folk.com`
- public reset fallback correction
- public AASA delivery
- Supabase URL configuration
- Supabase email templates
- final Cloudflare ownership alignment for the chosen public auth fallback host

## 4. Closure Rule

This domain MUST NOT move to PASS or CONDITIONAL_PASS until:

- a canonical host is accepted
- reset and confirmation routes are frozen
- AASA and associated domains are aligned
- native confirmation route exists
- redirect allowlist is verified
- template routing is verified

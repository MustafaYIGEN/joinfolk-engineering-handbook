# Auth Email Status Gates

## 1. Metadata

- Status: Draft
- Owner: Mustafa / JoinFolk
- Scope: AUTH-EMAIL-01 browser-first auth-email closure
- Canonical: false

## 2. Gate Record

| Gate | State | Notes |
| --- | --- | --- |
| AUTH_EMAIL_CURRENT_CONTRACT | FAIL | Current reset and confirmation behavior remains mixed and not yet aligned to the accepted browser-first contract. |
| AUTH_EMAIL_FLOW_MODEL | BROWSER_FIRST | Password reset and confirmation are browser-first by decision. |
| AUTH_EMAIL_TARGET_HOST | DECIDED_JOIN_FOLK_COM | `join-folk.com` is the accepted canonical auth email host. |
| AUTH_EMAIL_HOST_READINESS | PARTIAL | Host and route contract are decided, but browser reset implementation is still incomplete. |
| AUTH_REDIRECT_ALLOWLIST | PASS_WITH_LEGACY_CLEANUP_REQUIRED | Exact canonical routes are allowlisted, but legacy and wildcard entries remain cleanup candidates. |
| AUTH_HOOKS | NONE | No auth hooks are configured. |
| AUTH_EMAIL_LOGO_ASSET | PASS | Verified canonical logo asset is live on `join-folk.com`. |
| AUTH_CONFIRMATION_EMAIL_UAT | PASS_WITH_VISUAL_FIX_REQUIRED | Confirmation email flow passed, but iPhone Mail heading contrast needs improvement. |
| AUTH_CONFIRMATION_SURFACE | WEB_FIRST_PASS | Web confirmation behavior passed operator UAT. |
| EMAIL_CONFIRMATION_NATIVE_ROUTE | NOT_REQUIRED_BY_DECISION | Native confirmation is no longer a launch blocker. |
| PASSWORD_RESET_TARGET_SURFACE | WEB_ONLY | New password-reset requests must complete on the public web reset page. |
| PASSWORD_RESET_WEB_FLOW | IMPLEMENTATION_REQUIRED | Browser reset behavior is not yet correctly implemented in production. |
| PASSWORD_RESET_NATIVE_ROUTE | LEGACY_COMPATIBILITY_ONLY | Native reset may remain temporarily for legacy email and installed-client compatibility. |
| AUTH_AASA_POLICY | AUTH_ROUTES_BROWSER_ONLY | Auth routes must remain browser-only and MUST NOT be captured by AASA. |
| AUTH_EMAIL_TEMPLATES | IMPLEMENTED_UAT_PARTIAL | Templates are saved and functional, but iPhone Mail heading contrast still needs correction. |
| AUTH_APP_DOMAIN_FALLBACK | REJECTED_AS_CURRENT_AUTH_EMAIL_HOST | `app.join-folk.com` remains an organizer/dashboard surface and is not an auth-email host. |
| AUTH_PUBLIC_RESET_FALLBACK | PROVEN_BROKEN | `join-folk.com` and `www.join-folk.com` reset routes still render the wrong surface today. |
| AUTH_PUBLIC_CONFIRMATION_FALLBACK | PARTIAL | Browser confirmation fallback exists and passed operator UAT, but visual polish remains open. |
| AUTH_LIVE_AASA | FAIL | Live AASA is still missing on `join-folk.com`, but this does not block browser-only auth-email completion. |
| AUTH_EMAIL_IMPLEMENTATION | AUTHORIZED_UNDER_ACCEPTED_BROWSER_FIRST_PATCH_PLAN | Implementation may proceed under the accepted browser-first patch plan. |
| AUTH_EMAIL_DOMAIN_OVERALL | ACTIVE_CLOSURE | The domain is not closed yet, but the final browser-first contract is accepted and implementation is active. |

## 3. Open Implementation Gates

- public web reset route correction
- browser token/session recovery completion on the reset page
- browser password form and success/error states
- removal of automatic app-open behavior from reset
- iPhone Mail heading contrast fix
- reset UAT on iPhone Safari and desktop browser
- rollout-safe cleanup after legacy proof

## 4. Closure Rule

This domain MUST NOT move to PASS or CONDITIONAL_PASS until:

- browser reset completion is correct on the canonical web route
- reset states are verified for valid, expired, invalid, already-used, and network-failure cases
- the reset success page uses only explicit user-controlled CTAs
- legacy cleanup is verified safe after rollout
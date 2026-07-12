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
| AUTH_EMAIL_HOST_READINESS | PARTIAL | Host and route contract are decided, but rollout evidence remains incomplete. |
| AUTH_REDIRECT_ALLOWLIST | PASS_WITH_LEGACY_CLEANUP_REQUIRED | Exact canonical routes are allowlisted, but legacy and wildcard entries remain cleanup candidates. |
| AUTH_HOOKS | NONE | No auth hooks are configured. |
| CUSTOM_SMTP | PASS | Custom SMTP is configured and operating under the verified production auth-email setup. |
| SPF | PASS | Sender SPF is verified. |
| DKIM | PASS | Sender DKIM is verified. |
| DMARC | PASS_MONITORING_POLICY | DMARC is configured in monitoring mode only. |
| DMARC_POLICY | p=none | DMARC policy remains `p=none`. |
| AUTH_EMAIL_LOGO_ASSET | PASS | Verified canonical logo asset is live on `join-folk.com`. |
| AUTH_CONFIRMATION_EMAIL_UAT | PASS | Confirmation email flow passed operator UAT. |
| AUTH_CONFIRMATION_SURFACE | WEB_FIRST_PASS | Web confirmation behavior passed operator UAT. |
| EMAIL_CONFIRMATION_NATIVE_ROUTE | NOT_REQUIRED_BY_DECISION | Native confirmation is no longer a launch blocker. |
| PASSWORD_RESET_TARGET_SURFACE | WEB_ONLY | New password-reset requests must complete on the public web reset page. |
| PASSWORD_RESET_WEB_IMPLEMENTATION | DEPLOYED | Browser-first production reset implementation is deployed on the canonical web route. |
| PASSWORD_RESET_WEB_PRODUCTION_UAT | OPEN | Token-bearing browser reset production UAT remains open. |
| PASSWORD_RESET_MOBILE_REQUEST_REDIRECT | IMPLEMENTED_PENDING_NEW_BINARY_UAT | Mobile reset requests were updated to the canonical web route and await new-binary UAT. |
| PASSWORD_RESET_NATIVE_ROUTE | LEGACY_COMPATIBILITY_ONLY | Native reset may remain temporarily for legacy email and installed-client compatibility. |
| RECOVERY_ONBOARDING_ISOLATION | IMPLEMENTED_PENDING_NEW_BINARY_UAT | Recovery intent isolation is implemented and awaits new-binary UAT. |
| AUTH_AASA_POLICY | AUTH_ROUTES_BROWSER_ONLY | Auth routes must remain browser-only and MUST NOT be captured by AASA. |
| AUTH_EMAIL_TEMPLATES | PASS | Production auth email templates are configured and verified. |
| AUTH_APP_DOMAIN_FALLBACK | REJECTED_AS_CURRENT_AUTH_EMAIL_HOST | `app.join-folk.com` remains an organizer/dashboard surface and is not an auth-email host. |
| AUTH_PUBLIC_RESET_FALLBACK | PARTIAL | Canonical browser reset is deployed, but token-bearing production UAT remains open. |
| AUTH_PUBLIC_CONFIRMATION_FALLBACK | PARTIAL | Browser confirmation fallback exists and passed operator UAT, but visual polish remains open. |
| AUTH_LIVE_AASA | FAIL | Live AASA is still missing on `join-folk.com`, but this does not block browser-only auth-email completion. |
| AUTH_EMAIL_IMPLEMENTATION | AUTHORIZED_UNDER_ACCEPTED_BROWSER_FIRST_PATCH_PLAN | Implementation may proceed under the accepted browser-first patch plan. |
| SOURCE_CONTROLLED_EMAIL_TEMPLATES | OPEN_REPRODUCIBILITY_GAP | Dashboard templates are live, but a source-controlled template authority remains open. |
| SENDER_AVATAR_BIMI_APPLE | DEFERRED_NON_BLOCKING | Sender avatar / BIMI / Apple Branded Mail remains deferred and is not a launch blocker. |
| AUTH_EMAIL_DOMAIN_OVERALL | ACTIVE_CLOSURE | The domain is not closed yet, but the final browser-first contract is accepted and implementation is active. |

## 3. Open Implementation Gates

- token-bearing browser reset production UAT
- legacy native recovery regression UAT
- normal onboarding regression UAT
- rollout-safe cleanup after legacy proof
- source-controlled email template reproducibility gap

## 4. Closure Rule

This domain MUST NOT move to PASS or CONDITIONAL_PASS until:

- reset states are verified for valid, expired, invalid, already-used, and network-failure cases on the deployed browser route
- legacy native recovery and normal onboarding regressions are verified safe on the new mobile binary
- the reset success page uses only explicit user-controlled CTAs
- legacy cleanup is verified safe after rollout

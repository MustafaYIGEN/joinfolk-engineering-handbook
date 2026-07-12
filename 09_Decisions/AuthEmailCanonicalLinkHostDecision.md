# Auth Email Canonical Link Host Decision

## 1. Metadata

- Status: Blocked
- Owner: Mustafa / JoinFolk
- Decision type: auth-link host / universal-link boundary / fallback routing
- Scope: password-reset and email-confirmation links only
- Evidence date: 2026-07-12
- Canonical: false until unblocked

## 2. Decision Question

Which exact production host MUST own auth email links for:

- password reset
- email confirmation

Candidates:

- `join-folk.com`
- `www.join-folk.com`
- `app.join-folk.com`

## 3. Decision Outcome

Current outcome: `BLOCKED_DEPLOYMENT_CONFLICT`

No canonical host is accepted yet.

## 4. Required Selection Criteria

A host may be accepted only when all are true:

1. production route is owned by the intended public auth web project
2. reset and verification fallback routes exist on that exact host
3. AASA is served directly on that exact host
4. mobile can declare that exact associated domain
5. Supabase redirect allowlist accepts the exact routes
6. `/login` and `/download` remain browser-only
7. organizer dashboard is not repurposed as the end-user password-recovery surface
8. no mandatory redirect silently changes the universal-link host
9. web fallback still works when the app is absent

## 5. Evidence Summary

### `join-folk.com`

- auth routes present: yes
- reset fallback currently broken: yes; reset route renders `JoinFolk — Email Verified`
- confirmation fallback currently present: yes
- live AASA: `404` in current live evidence
- mobile associated domain currently present: no
- Supabase allowlist evidence: missing
- result: blocked

### `www.join-folk.com`

- at least one auth route present: yes
- reset fallback currently broken: yes; reset route renders `JoinFolk — Email Verified`
- confirmation fallback currently present: yes
- live AASA: `404` in current live evidence
- mobile associated domain currently present: no
- Supabase allowlist evidence: missing
- result: blocked

### `app.join-folk.com`

- mobile associated domain currently present: yes
- Cloudflare production app/dashboard ownership: proven
- live auth routes: present, but currently render organizer dashboard HTML
- live AASA: present, but currently returns organizer dashboard HTML instead of valid AASA JSON
- Supabase allowlist evidence: missing
- result: `REJECTED_AS_CURRENT_AUTH_EMAIL_HOST`

Rejection reason:

- current production route ownership is organizer/dashboard-facing, not end-user auth-recovery-facing
- live AASA response is invalid for universal-link use
- this host MAY be reconsidered only after an explicit production-surface architecture decision, not merely after more evidence

## 6. Binding Rules While Blocked

- AUTH_EMAIL implementation MUST NOT freeze a host by assumption
- AASA changes MUST NOT be made before the host decision is unblocked
- mobile associated-domain changes MUST NOT be made before the host decision is unblocked
- email template route changes MUST NOT be made before the host decision is unblocked
- the existing mobile `forgot-password.tsx` unstaged change MUST remain treated as pre-existing work, not as a frozen product decision
- `app.join-folk.com` MUST NOT be used as the current auth email host under the present production architecture

## 7. Remaining Candidate Set

The only remaining current-candidate public auth hosts are:

- `join-folk.com`
- `www.join-folk.com`

Neither is accepted because:

- live AASA is missing
- reset route renders the wrong surface
- Supabase URL configuration is not yet captured
- email templates are not yet captured
- root vs `www` canonical-host policy is not frozen

## 8. Unblock Requirements

The decision becomes eligible for acceptance only after:

- Supabase Dashboard URL configuration is captured
- Supabase email-template routing is captured
- one public host between `join-folk.com` and `www.join-folk.com` is selected as canonical
- Cloudflare project ownership for the exact public auth fallback host is confirmed against live behavior
- reset fallback route is corrected on the selected host
- confirmation fallback route is confirmed on the selected host
- live AASA is served on the selected host
- safe browser-only handling for `/login` and `/download` is preserved in the selected host contract

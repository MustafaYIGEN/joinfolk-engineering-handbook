# Current Status

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is the platform-level current status draft for JoinFolk. It describes current known handbook status and verification needs.

This is not a code audit, not an accepted product readiness report, not a release readiness report, and not a confirmed implementation status report. No implementation area is complete, incomplete, blocked, released, production-ready, launch-ready, or accepted until verified.

## 3. Current Status Definition

Current status must distinguish:

- Handbook draft status.
- Verified accepted status.
- Implementation status.
- Release status.
- Unknown / Needs verification.

Existing handbook drafts are non-canonical until verified. Exact implementation status, release readiness, launch readiness, completion percentage, blocker list, risk list, backlog state, audit findings, patch plans, ADR decisions, and production state are Unknown / Needs verification.

## 4. Status Authority Model

### What this document may state

- That handbook layers and drafts exist as non-canonical draft material.
- That known implementation surfaces and product domains need verification.
- That next major work is expected to be read-only implementation audit before patching.
- That production SQL, migrations, functions, RLS, storage, and auth changes require explicit approval.

### What this document may not claim without verification

- Completion percentage.
- Release readiness or launch readiness.
- Confirmed blockers or accepted risks.
- Accepted backlog items.
- Audit findings.
- Patch plans.
- Accepted ADR decisions.
- Exact implementation status.
- Exact production state.
- That any module, surface, backend behavior, frontend behavior, database behavior, security behavior, release behavior, or operations behavior is complete or accepted.

### What requires explicit approval before status changes imply implementation

- Production SQL changes.
- Migrations.
- Functions.
- RLS changes.
- Storage policy changes.
- Auth changes.
- Any uncontrolled production change.
- Any implementation change derived from status interpretation.

## 5. Handbook Baseline Status Draft

JoinFolk has handbook layers for governance, product, architecture, database, modules, security, operations, audits, patch plans, decisions / ADRs, and status/backlog.

Recently drafted handbook layers include module drafts, security drafts, operations drafts, audit index draft, patch plan index draft, and ADR index draft. These drafts are non-canonical until verified.

## 6. Known Handbook Layers Draft

- Governance: Draft status is known; accepted status is Unknown / Needs verification.
- Product: Draft status is known; accepted status is Unknown / Needs verification.
- Architecture: Draft status is known; accepted status is Unknown / Needs verification.
- Database: Draft status is known; accepted status is Unknown / Needs verification.
- Modules: Recently drafted; accepted status is Unknown / Needs verification.
- Security: Recently drafted; accepted status is Unknown / Needs verification.
- Operations: Recently drafted; accepted status is Unknown / Needs verification.
- Audits: Audit index draft exists; accepted audit process is Unknown / Needs verification.
- Patch plans: Patch plan index draft exists; accepted patch plans are Unknown / Needs verification.
- Decisions / ADRs: ADR index draft exists; accepted decisions are Unknown / Needs verification.
- Status/backlog: Current status draft exists; accepted backlog state is Unknown / Needs verification.

## 7. Known Implementation Surfaces Draft

### Mobile

JoinFolk has a mobile app surface. React Native / Expo concepts may apply. Exact mobile implementation status and readiness are Unknown / Needs verification.

### Dashboard

JoinFolk has a dashboard surface. Vite / React concepts may apply. Exact dashboard implementation status and readiness are Unknown / Needs verification.

### Web/Public

JoinFolk may have Web/Public surfaces. Exact Web/Public implementation status and readiness are Unknown / Needs verification.

### Supabase Backend / Database / Storage / Auth

JoinFolk uses or may use Supabase or Supabase-like backend concepts, including RLS, RPC, SECURITY DEFINER, storage policies, auth, and migrations. Exact backend, database, storage, auth, and production status are Unknown / Needs verification.

## 8. Known Product Domains Draft

Known security-sensitive or product domains include:

- Event lifecycle.
- Viewer roles.
- Personas and tiers.
- Ticketing.
- Reservations.
- Wallet/ownership.
- Media/gallery.
- Feed/discovery.
- Messaging where applicable.
- Notifications.
- Staff scanner/check-in.
- Venue/business tools.
- Host identity transfer.
- Ops/admin.
- Public sharing.

Exact implementation status and accepted behavior for each domain are Unknown / Needs verification.

## 9. Non-Accepted Status Areas

- Exact current implementation status is not accepted yet.
- Exact release readiness is not accepted yet.
- Exact launch readiness is not accepted yet.
- Exact completion percentage is not accepted yet.
- Exact blocker list is not accepted yet.
- Exact accepted risk list is not accepted yet.
- Exact backlog state is not accepted yet.
- Exact audit findings are not accepted yet.
- Exact patch plans are not accepted yet.
- Exact ADR decisions are not accepted yet.
- Exact production state is not accepted yet.
- Exact mobile/dashboard/web/backend readiness is not accepted yet.
- Exact security/database/operations readiness is not accepted yet.
- Exact testing, monitoring, incident, release readiness is not accepted yet.

## 10. Draft Current Handbook Status

The handbook baseline includes draft layers and recently drafted module, security, operations, audit index, patch plan index, and ADR index documents.

This is handbook draft status only. It does not prove implementation status, release status, production state, or accepted behavior.

## 11. Draft Current Implementation Status

Implementation status is Unknown / Needs verification across Mobile, Dashboard, Web/Public where applicable, and Supabase Backend / Database / Storage / Auth.

No surface or domain is marked complete, incomplete, blocked, released, production-ready, launch-ready, or accepted in this document.

## 12. Draft Current Release Status

Release status is Unknown / Needs verification. This document does not claim release readiness, launch readiness, production readiness, release gates, rollout status, rollback status, or production state.

## 13. Draft Current Security Status

Security status is Unknown / Needs verification. Security-sensitive domains include event lifecycle, viewer roles, personas and tiers, ticketing, reservations, wallet/ownership, media/gallery, feed/discovery, messaging where applicable, notifications, staff scanner/check-in, venue/business tools, host identity transfer, ops/admin, and public sharing.

No security behavior, RLS policy, RPC contract, storage policy, auth behavior, permission behavior, or public exposure behavior is accepted by this document.

## 14. Draft Current Database Status

Database status is Unknown / Needs verification. Supabase or Supabase-like backend concepts, RLS, RPC, SECURITY DEFINER, storage policies, auth, and migrations are known concepts only.

Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

## 15. Draft Current Operations Status

Operations draft areas include build and release, testing strategy, monitoring and logging, and incident response.

Exact operational readiness, release gates, test strategy, monitoring/logging process, incident response process, and production behavior are Unknown / Needs verification.

## 16. Draft Current Audit Status

AuditIndex.md is a draft index for future audits. Exact audit findings, methodology, schedule, evidence requirements, severity model, priority model, owners, due dates, and remediation status are Unknown / Needs verification.

Future implementation audits should be read-only first.

## 17. Draft Current Patch Plan Status

PatchPlanIndex.md is a draft index for future patch plans. Exact patch plans, priorities, severities, owners, due dates, implementation steps, files, commands, release gates, rollback paths, and remediation status are Unknown / Needs verification.

Patch plans must not be created from unverified findings as accepted remediation.

## 18. Draft Current ADR Status

ADRIndex.md is a draft index for future ADRs. Exact ADR decisions, decision IDs, statuses, owners, dates, consequences, implementation requirements, supersession rules, approval history, and review cadence are Unknown / Needs verification.

ADRs must not be created from unverified assumptions as accepted decisions.

## 19. Draft Current Backlog Status

Backlog status is Unknown / Needs verification. This document does not accept backlog items, priorities, owners, due dates, blockers, risks, launch state, release state, or production state.

## 20. Next Verification Sequence Draft

The next major work after handbook baseline is expected to be read-only implementation audit before patching.

The expected sequence is draft only:

- Read-only implementation audit.
- Handbook vs implementation gap report.
- P0/P1 patch plan.
- Controlled patching only after approval.

Exact sequence details, scope, priorities, and acceptance criteria are Unknown / Needs verification.

## 21. Read-Only Audit Sequence Draft

The read-only implementation audit sequence is expected to cover:

- Supabase/backend.
- Dashboard.
- Mobile.
- Web/Public where applicable.
- Handbook vs implementation gap report.
- P0/P1 patch plan.
- Controlled patching only after approval.

This sequence is not an accepted audit process or remediation plan until verified.

## 22. Implementation Gap Report Draft

A future implementation gap report may compare handbook drafts against verified implementation facts. Exact gap report format, evidence requirements, finding status model, severity/priority model, owners, due dates, and remediation workflow are Unknown / Needs verification.

## 23. P0 / P1 Patch Planning Draft

P0/P1 patch planning is expected after read-only audit and gap reporting, but exact priorities, severity model, patch plans, owners, due dates, implementation steps, files, commands, release gates, rollback paths, and remediation status are Unknown / Needs verification.

## 24. Controlled Patching Draft

Controlled patching may happen only after approval. Future implementation changes must be scoped and reviewed before commit.

Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Exact patching workflow, branch strategy, test/re-test workflow, release relationship, rollback model, and production approval workflow are Unknown / Needs verification.

## 25. Cross-Surface Consistency Requirements

### Mobile

Mobile status must remain separate from handbook draft status and must not be claimed as complete, incomplete, blocked, ready, or accepted without verification.

### Dashboard

Dashboard status must remain separate from handbook draft status and must not be claimed as complete, incomplete, blocked, ready, or accepted without verification.

### Web/Public

Web/Public status, where applicable, must remain separate from handbook draft status and must not be claimed as complete, incomplete, blocked, ready, or accepted without verification.

### Supabase Backend

Supabase Backend / Database / Storage / Auth status must remain separate from handbook draft status and must not be claimed as complete, incomplete, blocked, ready, production-ready, or accepted without verification.

### Handbook

Handbook status must distinguish draft documents from verified accepted facts.

## 26. Security Risks

- Treating draft handbook status as accepted security or implementation status.
- Claiming security-sensitive readiness without verified backend, RLS, RPC, storage, auth, database, or production evidence.
- Allowing status interpretation to imply production SQL, migration, function, RLS, storage, or auth changes without explicit approval.

## 27. Privacy Risks

- Claiming public exposure, media visibility, messaging, ticketing, reservation, wallet/ownership, ops/admin, or host identity transfer status without verification.
- Recording sensitive status details without accepted evidence and reporting rules.

## 28. Determinism Risks

- Mixing handbook draft status with implementation status.
- Treating unknown status as complete, incomplete, blocked, ready, or accepted.
- Assigning priorities, risks, blockers, owners, due dates, launch state, release state, or production state without accepted models.

## 29. Operational Risks

- Treating this document as a release readiness report.
- Starting patching before read-only audit and approval.
- Creating uncontrolled production changes from unverified status.
- Treating build, testing, monitoring/logging, or incident draft status as operational readiness.

## 30. Maintainability Risks

- Duplicating state across status/backlog, audits, patch plans, ADRs, operations, and module documents without a verified relationship model.
- Allowing non-canonical drafts to be interpreted as accepted current state.
- Failing to update status after verified audits, accepted ADRs, approved patch plans, or controlled implementation work.

## 31. Current Known Implementation

Known implementation surfaces and concepts include Mobile app, Dashboard, Web/Public where applicable, Supabase backend/storage/database/auth concepts, React Native / Expo concepts, Vite / React concepts, RLS, RPC, SECURITY DEFINER, storage policies, auth, and migrations.

Exact implementation status, production state, release readiness, launch readiness, completion percentage, blocker list, risk list, backlog state, audit findings, patch plans, ADR decisions, and operational readiness are Unknown / Needs verification.

## 32. Unknowns / Needs Verification

- Exact current implementation status.
- Exact release readiness.
- Exact launch readiness.
- Exact completion percentage.
- Exact blocker list.
- Exact accepted risk list.
- Exact backlog state.
- Exact audit findings.
- Exact patch plans.
- Exact ADR decisions.
- Exact production state.
- Exact mobile/dashboard/web/backend readiness.
- Exact security/database/operations readiness.
- Exact testing, monitoring, incident, and release readiness.

## 33. Acceptance Criteria for v1.0

For this document to become v1.0, JoinFolk must verify and accept:

- Status model distinguishing draft, accepted, implementation, release, and unknown states.
- Handbook baseline status.
- Read-only implementation audit process.
- Implementation gap report format.
- Audit-to-patch planning workflow.
- P0/P1 priority model, if used.
- Controlled patching workflow.
- Status/backlog relationship.
- ADR relationship.
- Release readiness and launch readiness reporting boundaries.
- Production state reporting boundaries.
- Explicit production-change approval constraints for SQL, migrations, functions, RLS, storage, and auth.

## 34. Open Questions

- What status model should distinguish handbook draft status, accepted status, implementation status, release status, and Unknown / Needs verification?
- What evidence is required before any implementation area can be marked complete, incomplete, blocked, ready, or accepted?
- What read-only implementation audit process should be accepted?
- What should the handbook vs implementation gap report contain?
- What priority model should P0/P1 patch planning use?
- What approvals are required before controlled patching begins?
- How should status/backlog relate to audits, patch plans, and ADRs?
- What evidence is required before release readiness or launch readiness can be claimed?
- What evidence is required before production state can be reported?
- How should status preserve cross-surface consistency across Mobile, Dashboard, Web/Public, Supabase Backend, and Handbook documents?

## 35. Notification Delivery Status

Notification delivery has moved to a conditional pass state, not a final close.

| Gate | State | Notes |
|---|---|---|
| SERVER_NOTIFICATION_RPC_BOUNDARY | PASS | Self-targeted wrapper is the approved mobile-facing RPC. |
| SERVER_PUSH_OUTBOX_SECURITY | PASS | Outbox is RLS-enabled, service-role constrained, and uses fenced atomic claim/retry semantics. |
| SERVER_PUSH_SCHEDULER | PASS | Scheduler uses pg_cron + pg_net + Vault and invokes only the internal helper. |
| SERVER_PUSH_AUTHORIZATION | PASS | push-dispatch requires the dispatch secret before service-role access. |
| SERVER_PUSH_POLICY_ENFORCEMENT | PASS | Reminder notifications are excluded from server push. |
| SERVER_PUSH_PROVIDER_DISPATCH | PASS | Guarded Expo dispatch and dead-token cleanup are recorded. |
| SERVER_PUSH_DEVICE_VISIBILITY | DEVICE_UAT_REQUIRED | Closed-app visible receipt still needs explicit device evidence unless separately proven. |
| LOCAL_REMINDER_IMPLEMENTATION | IMPLEMENTED_NOT_RELEASED | Local reminder scheduling exists but is not yet accepted as released device evidence. |
| LOCAL_REMINDER_DEVICE_DELIVERY | DEVICE_UAT_REQUIRED | Closed-app reminder delivery must be confirmed on device. |
| LEGACY_NOTIFICATION_RPC_PHASE_B | ROLLOUT_DEPENDENT | Legacy authenticated RPC access remains temporary during installed-client rollout. |
| NOTIFICATION_DOMAIN_OVERALL | CONDITIONAL_PASS / NOT_FULLY_CLOSED | Server delivery is proven; device UAT and legacy RPC Phase B remain open. |

See [NotificationDeliveryStatusGates.md](NotificationDeliveryStatusGates.md) for the canonical status gate record.

## 36. Auth Email Status

AUTH-EMAIL-01 is in active closure under a browser-first contract.

| Gate | State | Notes |
|---|---|---|
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

See [AuthEmailStatusGates.md](AuthEmailStatusGates.md) for the canonical gate record.
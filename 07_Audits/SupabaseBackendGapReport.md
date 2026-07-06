# Supabase Backend Gap Report

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is a draft backend gap report based on a prior read-only Supabase/backend audit summary.

It is not a patch plan, not an accepted vulnerability list, not an accepted remediation list, not a production parity report, and not a release readiness report. Findings below separate file evidence from production unknowns. No issue should be treated as production-active unless production has been explicitly verified.

## 3. Gap Report Status

This report is draft and non-canonical. It records candidate backend gaps and verification needs from the supplied audit summary only.

- Accepted findings: None
- Accepted remediation list: None
- Final patch priority: None
- Production parity conclusion: Not available
- Release readiness conclusion: Not available

## 4. Source Audit Scope

Paths read during the prior audit:

- `C:\dev\hostos\supabase`
- `C:\dev\joinfolk-engineering-handbook`

Paths intentionally not read:

- `C:\dev\joinfolk-web`
- `C:\dev\joinfolk-web\dashboard`
- `C:\dev\hostos\apps\mobile`
- Web/Public
- Dashboard
- Mobile

Operational constraints from the prior audit:

- No Supabase CLI commands were run.
- No database connection was made.
- No production state was inspected.
- No files were modified, created, staged, committed, generated, migrated, or applied.

## 5. Backend Inventory Summary

- 186 SQL migration files were found under `supabase/migrations`.
- Observed migration range:
  - `20260116212500_command_center_tables.sql`
  - through `20260626_commerce_standing_tickets_v1.sql`
- Notable late/security migrations:
  - `20260606_exact_seat_purchase_guard.sql`
  - `20260606_publish_readiness_guard.sql`
  - `20260618_p0_cross_entitlement_guard_v1.sql`
  - `20260619123535_fix_venue_media_storage_upload_policy.sql`
  - `20260626_commerce_standing_tickets_v1.sql`
- 27 created table names were detected in visible migrations.
- RLS enable statements were detected for all tables created in visible migrations.
- RLS was also detected for pre-existing `events`.
- No visible RLS enable evidence was found for heavily used pre-existing sensitive tables: `tickets`, `reservations`, `commerce_orders`, `event_ticket_claims_v1`, `event_media`.
- The sensitive table RLS point is Unknown / Needs verification, not a confirmed missing-RLS finding.
- 109 unique policy surface entries and 115 policy definitions were detected.
- 229 unique function names and 438 function definitions/redefinitions were detected.
- 210 unique `SECURITY DEFINER` function names and 412 `SECURITY DEFINER` definitions were detected.
- 21 `SECURITY DEFINER` definitions lacked visible `SET search_path` in the function header.
- Buckets detected: `avatars`, `venue-posters`, `venue-media`.
- `venue-media` was detected as public with public read policy evidence in visible migrations.
- Indexes: 69 unique names, 78 definitions.
- Triggers: 15 unique names, 16 definitions.
- Types/enums: 1 detected.
- Grants/revokes: 440 statements detected.

Example table names detected in visible migrations include:

- `event_participants`
- `event_checklist`
- `event_costs`
- `event_activity`
- `event_staff_assignments`
- `host_members`
- `host_member_invites`
- `venue_media`
- `venues`
- `venue_reservations`
- `notifications_v2`
- `push_tokens_v1`
- `user_notification_settings_v1`
- `app_diagnostics`

Policy families detected include events visibility, event participants, staff assignments, venue / venue media, notifications, push tokens/settings, checklist, host members, blocks, likes, relics, and storage.

Major RPC/function domains include event lifecycle/feed, ticketing/check-in, reservations, venue tools, commerce, notifications, media/gallery, staff scanner, push eligibility, profile/relics.

## 6. Confidence Model

- Confirmed from file evidence: The supplied audit summary reported direct evidence in allowed local files.
- Likely but needs verification: File evidence suggests a candidate gap, but active live definitions, grants, deployment settings, or production parity are not confirmed.
- Unknown / Needs verification: Evidence is incomplete, absent from visible files, or dependent on production/schema/deployment state that was not inspected.

## 7. Findings Summary

### Confirmed from file evidence

- BE-AUD-001: Repeated core RPC/function definitions make the local source of truth hard to establish.
- BE-AUD-003: `venue-media` bucket and public read policy evidence exist in visible migrations.
- BE-AUD-004: Positive control evidence exists for unsafe check-in revoke and staff scanner role checks.

### Likely but needs verification

- BE-AUD-002: `push-dispatch` lacks visible caller auth or webhook signature validation in file evidence; exploitability depends on deployment exposure and function-level controls.
- BE-AUD-005: 21 `SECURITY DEFINER` definitions lack visible `SET search_path`; active live definitions need verification.
- BE-AUD-006: Cross-entitlement guard migrations indicate known hardening around tickets, reservations, and pending gift claims; coverage of legacy claim/transfer paths needs verification.

### Unknown / Needs verification

- BE-AUD-007: RLS status for sensitive pre-existing tables cannot be confirmed from visible files alone.
- BE-AUD-008: Host identity transfer/admin execution backend implementation was not confirmed in allowed files.

## 8. Detailed Findings

### BE-AUD-001

- Finding ID: BE-AUD-001
- Domain: Determinism / maintainability
- Surface: Supabase/backend
- Evidence paths: `supabase/migrations`
- Evidence summary: Extraction found 438 function definitions for 229 unique names. Repeated definitions were observed for core RPCs including `create_reservation_v2`, `request_ticket_v2`, `create_commerce_order_v1`, `purchase_event_ticket_v4`, `checkin_ticket_by_id_v2`, and `get_ticket_by_id_v2`.
- Risk / impact: Local source of truth is hard to establish without replaying migrations or checking live function definitions.
- Confidence: Confirmed from file evidence
- Candidate priority: Candidate P1 for maintainability/source-of-truth clarity
- Suggested next step: Backend gap report and source-of-truth verification.
- Patch status: Not a patch plan
- Production status: Unknown / Needs verification

### BE-AUD-002

- Finding ID: BE-AUD-002
- Domain: Edge functions / auth boundary
- Surface: Supabase/backend
- Evidence paths:
  - `supabase/functions/push-dispatch/index.ts:29`
  - `supabase/functions/push-dispatch/index.ts:34`
  - `supabase/functions/push-dispatch/index.ts:43`
- Evidence summary: `push-dispatch` accepts JSON payload, uses `SUPABASE_SERVICE_ROLE_KEY`, and sends push based on caller-provided `user_id`. No caller auth or webhook signature validation is visible in file evidence.
- Risk / impact: If externally invokable, service-role behavior could be abused to query tokens or send notifications.
- Confidence: Confirmed from file evidence for missing visible check; exploitability needs deployment verification.
- Candidate priority: Candidate P0 if deployed as externally callable without function-level auth/signature controls.
- Suggested next step: Manual verification of deployment exposure, function-level auth settings, and invocation path.
- Patch status: Not a patch plan
- Production status: Unknown / Needs verification

### BE-AUD-003

- Finding ID: BE-AUD-003
- Domain: Storage / public exposure
- Surface: Supabase/backend
- Evidence paths:
  - `supabase/migrations/20260505_venue_commerce_v1.sql:218`
  - `supabase/migrations/20260505_venue_commerce_v1.sql:244`
- Evidence summary: `venue-media` bucket is created public. Storage read policy allows public read for all objects in that bucket.
- Risk / impact: Privacy depends on intended-public semantics and upload path controls.
- Confidence: Confirmed from file evidence
- Candidate priority: Unknown or Candidate P1 depending on accepted public exposure decision.
- Suggested next step: Audit public exposure decision / ADR.
- Patch status: Not a patch plan
- Production status: Unknown / Needs verification

### BE-AUD-004

- Finding ID: BE-AUD-004
- Domain: Staff scanner / check-in
- Surface: Supabase/backend
- Evidence paths:
  - `supabase/migrations/20260209_phase10f_unsafe_lockdown.sql:11`
  - `supabase/migrations/20260312_staff_checkin_rpc.sql:47`
- Evidence summary: Unsafe check-in RPC execute is revoked from public/anon/authenticated. Staff scanner RPC checks host or staff role.
- Risk / impact: Positive control evidence exists, but production grants must be verified.
- Confidence: Confirmed from file evidence
- Candidate priority: Manual verification / positive control evidence. Not a patch candidate unless production parity fails.
- Suggested next step: Manual production parity verification.
- Patch status: Not a patch plan
- Production status: Unknown / Needs verification

### BE-AUD-005

- Finding ID: BE-AUD-005
- Domain: SECURITY DEFINER
- Surface: Supabase/backend
- Evidence paths:
  - `20260119184500_collaborative_checklist.sql`
  - `20260120190000_fix_checklist_logic.sql`
  - `20260126_lifecycle_constraints.sql`
  - `20260127_*`
  - `20260314_personal_reminders.sql`
  - `20260605_phase6d3_module_mutual_exclusion.sql`
- Evidence summary: 21 `SECURITY DEFINER` definitions lack visible `SET search_path`.
- Risk / impact: Search-path hijack risk if active definitions remain live.
- Confidence: Likely but needs verification
- Candidate priority: Candidate P1 / Needs verification
- Suggested next step: Live function definition verification through an approved read-only process.
- Patch status: Not a patch plan
- Production status: Unknown / Needs verification

### BE-AUD-006

- Finding ID: BE-AUD-006
- Domain: Ticketing / reservations / wallet
- Surface: Supabase/backend
- Evidence paths:
  - `supabase/migrations/20260618_p0_cross_entitlement_guard_v1.sql:40`
  - `supabase/migrations/20260618_p0_cross_entitlement_guard_v1.sql:788`
  - `supabase/migrations/20260626_commerce_standing_tickets_v1.sql:8`
- Evidence summary: Later migrations add cross-entitlement guards between tickets, reservations, and pending gift claims. Coverage of legacy claim/transfer functions is explicitly listed as not included in one migration comment.
- Risk / impact: Strong evidence of prior or known gap. Current active coverage and legacy path exposure need verification.
- Confidence: Likely but needs verification
- Candidate priority: Candidate P1 / Needs verification
- Suggested next step: Backend gap report focus on entitlement paths and manual verification of active RPC coverage.
- Patch status: Not a patch plan
- Production status: Unknown / Needs verification

### BE-AUD-007

- Finding ID: BE-AUD-007
- Domain: RLS / sensitive tables
- Surface: Supabase/backend
- Evidence paths:
  - No visible RLS enable statement found for `tickets`, `reservations`, `commerce_orders`, `event_ticket_claims_v1`, `event_media`.
  - Functions reference these tables in ticket/reservation/media migrations.
- Evidence summary: Local migration set references sensitive tables without showing their canonical creation/RLS definitions.
- Risk / impact: Cannot confirm table-level permission boundary from files alone.
- Confidence: Unknown / Needs verification
- Candidate priority: Candidate P1 / Unknown because production/schema parity is not confirmed.
- Suggested next step: Manual production parity verification or schema dump audit through an approved read-only process.
- Patch status: Not a patch plan
- Production status: Unknown / Needs verification

### BE-AUD-008

- Finding ID: BE-AUD-008
- Domain: Ops/admin / host identity transfer
- Surface: Supabase/backend
- Evidence paths:
  - Handbook mentions `admin_execute_host_identity_transfer_v1` as concept.
  - Backend search found no matching RPC in allowed files.
- Evidence summary: No backend implementation evidence for host identity transfer or admin execution RPC in allowed files.
- Risk / impact: Domain may be unimplemented, renamed, or out of scope.
- Confidence: Unknown / Needs verification
- Candidate priority: Unknown / ADR or manual verification. Not a confirmed bug.
- Suggested next step: ADR or manual verification.
- Patch status: Not a patch plan
- Production status: Unknown / Needs verification

## 9. Candidate P0 / P1 Areas

Candidate P0:

- BE-AUD-002 only if deployed as externally callable without function-level auth/signature controls.

Candidate P1:

- BE-AUD-001 for maintainability/source-of-truth clarity.
- BE-AUD-005 for `SECURITY DEFINER` search-path verification if active definitions remain live.
- BE-AUD-006 for entitlement path verification around tickets, reservations, wallet, and pending gift claims.
- BE-AUD-007 for sensitive table RLS unknowns, subject to production/schema parity verification.

Unknown:

- BE-AUD-003 priority depends on accepted public-sharing and storage exposure decision.
- BE-AUD-004 is positive control evidence requiring production parity verification.
- BE-AUD-008 depends on whether host identity transfer is implemented, renamed, out of scope, or only documented conceptually.

## 10. Domain-by-Domain Backend Notes

- Event lifecycle: RPCs and guards exist, including `publish_event_with_groups_and_snapshot_v2`, `transition_event_status_v2`, and publish readiness guard. Repeated replacements mean active behavior needs verification.
- Viewer roles: Policies and RPCs use `auth.uid()`, host ownership, participants, staff, host members, and public visibility.
- Personas and tiers: Tier/persona checks appear in feed, publish, notification, and media migrations. Consistency needs deeper read-only pass.
- Ticketing: Substantial RPC surface and hardening exist. Cross-entitlement guard is present.
- Reservations: Event and venue reservation RPCs are present. Capacity and cross-entitlement guards are present.
- Wallet/ownership: `get_my_tickets_v2`, gift claims, pending claims, and poster snapshots are present. Full ownership transfer is not confirmed.
- Media/gallery: Event media RPCs and venue media storage/table policies are present. Public media boundaries need exposure review.
- Feed/discovery: Multiple feed/discover RPCs and visibility RLS patches exist.
- Messaging: No clear backend messaging implementation was found in Supabase files read.
- Notifications: `notifications_v2`, settings, push tokens, and push dispatch are present.
- Staff scanner/check-in: Staff assignment table, staff check-in RPC, and unsafe lockdown are present.
- Venue/business tools: Venue tables, venue reservations, service offerings, venue media, and layout/blueprint functions are present.
- Host identity transfer: Not confirmed in backend files.
- Ops/admin: No canonical admin role model confirmed from backend files.
- Public sharing: `get_event_share`, public venue/profile/media/relic functions, and public storage policies are present.

## 11. Production Parity Unknowns

- Production parity cannot be confirmed from files.
- Active live function bodies and grants cannot be confirmed from files.
- Original schema/RLS for pre-existing sensitive tables is incomplete in local migrations.
- Edge Function deployment auth/JWT settings are not visible from files alone.
- Whether public storage buckets are intentionally public per accepted product/security decision is unverified.
- Handbook documents are draft/non-canonical, so domain expectations are guidance only.

## 12. Manual Verification Needs

- Verify whether `push-dispatch` is deployed as externally callable and what function-level auth/signature controls apply.
- Verify active live definitions for repeated core RPCs and `SECURITY DEFINER` functions through an approved read-only process.
- Verify grants and RLS state for sensitive pre-existing tables through an approved read-only process.
- Verify whether `venue-media` public exposure is accepted product/security behavior.
- Verify active entitlement coverage across tickets, reservations, wallet, pending gift claims, and legacy claim/transfer paths.
- Verify whether host identity transfer/admin execution is implemented, renamed, out of scope, or only documented conceptually.

## 13. ADR Candidates

These are candidate ADR topics only, not accepted ADRs:

- Public media exposure semantics for `venue-media`.
- Backend source-of-truth convention for repeatedly replaced RPCs.
- Host identity transfer/admin execution domain ownership and expected backend authority.
- Production parity verification process for RLS, grants, functions, storage, and Edge Function auth settings.

## 14. Patch Plan Candidates

These are candidate patch areas only, not accepted patch plans:

- `push-dispatch` auth boundary if deployment verification shows external invocation without function-level auth/signature controls.
- `SECURITY DEFINER` search-path posture if live definitions remain active without visible safe search-path configuration.
- Entitlement path hardening if manual verification shows uncovered active ticket/reservation/wallet paths.
- Sensitive table RLS/grant posture if production/schema parity verification shows missing or unintended table-level boundaries.

## 15. Non-Accepted Items

- No finding in this report is accepted as production-active.
- No remediation is accepted.
- No priority is final.
- No ADR is accepted.
- No patch plan is accepted.
- No release readiness conclusion is provided.

## 16. Security Risks

- Conditional service-role Edge Function abuse risk for `push-dispatch` if externally callable without function-level auth/signature controls.
- Potential search-path hijack risk if `SECURITY DEFINER` functions lacking visible `SET search_path` remain active.
- Unknown table-level permission boundary for sensitive pre-existing tables due to incomplete local evidence.
- Unknown active coverage for legacy entitlement paths.

## 17. Privacy Risks

- Public `venue-media` exposure may be intended or unintended; accepted semantics are not verified.
- Public sharing functions and storage policies require clear product/security decision records.
- Production storage object state and access exposure were not inspected.

## 18. Determinism / Maintainability Risks

- Repeated core RPC definitions make active local source of truth difficult to establish from files alone.
- Live behavior cannot be inferred confidently without replayed migration state or live read-only verification.
- Draft/non-canonical handbook expectations should not be treated as implementation authority.

## 19. Operations Risks

- Production parity is unknown.
- Live grants, function bodies, storage policy behavior, and Edge Function deployment settings are unknown.
- Admin role model and host identity transfer backend authority are not confirmed.

## 20. Recommended Next Step

The recommended next step is a bounded manual verification pass through an approved read-only process, focused on production parity unknowns, live function definitions/grants, Edge Function deployment auth settings, sensitive table RLS/grants, and accepted public media exposure semantics.

This recommended next step is not a patch plan.

## 21. No-Modification / No-Production Confirmation

For this draft report:

- No JoinFolk application code repositories were inspected or modified.
- Dashboard, Mobile, and Web/Public code were not inspected or modified.
- Supabase production was not inspected.
- No database connection was made.
- Supabase CLI was not run.
- No migrations were run or generated.
- No SQL fixes were written.
- No patch plan was created.
- No files were staged or committed.

## 22. Open Questions

- Is `push-dispatch` deployed with function-level auth and/or signature controls?
- Which repeated RPC definitions are active in the current production database?
- Do live `SECURITY DEFINER` functions include safe search-path posture?
- Are `tickets`, `reservations`, `commerce_orders`, `event_ticket_claims_v1`, and `event_media` protected by intended live RLS/grants?
- Is `venue-media` intentionally public under an accepted product/security decision?
- Are legacy claim/transfer entitlement paths still active and covered?
- Is host identity transfer implemented elsewhere, renamed, out of scope, or only a documented concept?

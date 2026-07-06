# Supabase Production Parity Verification Report

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Manual production verification summary
- canonical: false

## 2. Purpose

This document is a draft production parity verification report for JoinFolk Supabase/backend.

It is based on manual production SQL outputs and dashboard checks supplied by the operator, plus the draft `07_Audits/SupabaseBackendGapReport.md`. It is not a patch plan, not an accepted vulnerability list, not an accepted remediation list, and not a release readiness report.

Production impact and patch priority are not final unless directly supported by supplied production evidence. Backend/RPC/RLS/storage/auth remain the security-sensitive authority. Frontend UX is not security authority.

## 3. Verification Scope

Verification target findings:

- BE-AUD-001: repeated core RPC definitions / unclear active source of truth
- BE-AUD-002: `push-dispatch` Edge Function auth boundary
- BE-AUD-003: public `venue-media` exposure
- BE-AUD-004: staff scanner/check-in positive control parity
- BE-AUD-005: `SECURITY DEFINER` functions missing visible `search_path`
- BE-AUD-006: ticket/reservation/wallet cross-entitlement guard parity
- BE-AUD-007: sensitive table RLS parity
- BE-AUD-008: host identity transfer / admin RPC existence

The operator manually ran read-only production verification queries in Supabase SQL Editor and performed dashboard checks. Some outputs were partial or truncated.

## 4. Safety Constraints

- No JoinFolk application code repositories were inspected for this report.
- Dashboard, Mobile, and Web/Public application code were not inspected.
- Codex did not connect to production.
- Supabase CLI was not run.
- No migrations were run or generated.
- No SQL fixes are included.
- No implementation instructions are included.
- No patch plan is created.
- No files were staged or committed.

## 5. Evidence Summary

Production evidence supplied by the operator:

- Migration metadata relation discovery returned no rows.
- Querying `supabase_migrations.schema_migrations` failed with relation-not-found error, so production migration version parity remains Unknown / Needs verification.
- Target sensitive tables exist in `public` and have RLS enabled: `commerce_orders`, `event_media`, `event_staff_assignments`, `event_ticket_claims_v1`, `events`, `notifications_v2`, `push_tokens_v1`, `reservations`, `tickets`, `user_notification_settings_v1`, `venue_media`, `venues`.
- `relforcerowsecurity = false` and owner `postgres` were reported for all listed target tables.
- Production policies were returned for target tables, including commerce order deny-all, event media select/update, event staff assignment, and event visibility/host policies.
- Storage buckets `avatars`, `venue-media`, and `venue-posters` are public in production.
- Storage object policies confirm public read for those buckets and authenticated owner/host-constrained write patterns in supplied output.
- `admin_execute_host_identity_transfer_v1` exists in production, is `SECURITY DEFINER`, has `search_path=public`, includes an `auth_is_ops()` gate, and has broad execute privileges reported.
- `checkin_ticket_by_id_v2` denies anon/public execute and allows authenticated/postgres/service_role execute.
- Some production `SECURITY DEFINER` functions have null `proconfig` and no search_path configuration.
- `push-dispatch` was not visible in the production Supabase Edge Functions dashboard.

## 6. Production Parity Results by Finding

| Finding | Production parity status | Candidate priority after verification | Production impact |
| --- | --- | --- | --- |
| BE-AUD-001 | Partially confirmed | Candidate P1 | Unknown / Needs verification |
| BE-AUD-002 | Not confirmed as production-active | Unknown; Candidate P0 only if future deployment/exposure is confirmed | Not confirmed |
| BE-AUD-003 | Confirmed public exposure | Unknown or Candidate P1 depending on accepted decision | Public read exposure confirmed; acceptability unconfirmed |
| BE-AUD-004 | Positive control partially confirmed | Legacy function review candidate | Unknown for legacy functions |
| BE-AUD-005 | Confirmed | Candidate P1 | Needs security hardening review |
| BE-AUD-006 | Needs verification | Candidate P1 / Unknown | Unknown |
| BE-AUD-007 | RLS enabled confirmed | RLS-enable unknown downgraded; policy review remains | Missing-RLS concern not supported by supplied evidence |
| BE-AUD-008 | Existence confirmed | Unknown / possible grant hardening candidate | Final exploitability not confirmed |

## 7. BE-AUD-001 Production Status

Production evidence:

- Multiple RPC overloads/repeated surfaces were observed, especially `create_reservation_v2`.
- Original migration-level repeated definitions remain maintainability evidence.

Interpretation:

- Production parity status: Partially confirmed.
- Candidate priority: Candidate P1 for determinism/source-of-truth clarity.
- Production impact: Unknown / Needs verification.
- Suggested next step: Backend source-of-truth inventory / RPC overload review.

## 8. BE-AUD-002 Production Status

Production evidence:

- `push-dispatch` was not visible/deployed in the production Edge Functions dashboard.
- Local code evidence supplied by the operator indicates caller-provided `user_id`, service role usage, and no visible inbound caller auth or webhook signature check.
- Dashboard JWT verification status is Not applicable / Unknown because the function was not deployed/visible.

Interpretation:

- Production parity status: Not confirmed as production-active.
- Candidate priority: Unknown / dormant local-code risk. Candidate P0 only if deployed as externally callable without JWT/signature/caller-auth controls.
- Production impact: Not confirmed.
- Suggested next step: Record as deployment guard / future hardening candidate.

## 9. BE-AUD-003 Production Status

Production evidence:

- Production buckets `avatars`, `venue-media`, and `venue-posters` have `public = true`.
- Public read policies exist for all three buckets.
- Storage write policies appear constrained by authenticated ownership or venue host ownership in supplied policy output.

Interpretation:

- Production parity status: Confirmed public exposure.
- Candidate priority: Unknown or Candidate P1 depending on accepted public-sharing decision.
- Production impact: Public read exposure is real; whether it is acceptable depends on product/security decision.
- Suggested next step: ADR/security decision for public media semantics.

## 10. BE-AUD-004 Production Status

Production evidence:

- `checkin_ticket_by_id_v2` grants are constrained for anon/public.
- `checkin_ticket_v2_unsafe` appears revoked from anon/authenticated/public.
- Legacy or related functions show broader grants, including `assert_checkin_open`, `check_in_ticket`, `control_open_checkin`, `ensure_ticket_checkin_proof_v1`, and `get_event_checkin_summary`.

Interpretation:

- Production parity status: Positive control partially confirmed.
- Candidate priority: Not a direct patch candidate for `checkin_ticket_by_id_v2`; legacy function review candidate.
- Production impact: Unknown for legacy functions until active use and internal guards are reviewed.
- Suggested next step: Legacy check-in RPC review.

## 11. BE-AUD-005 Production Status

Production evidence:

- Some live `SECURITY DEFINER` functions lack `search_path` proconfig.
- Examples supplied include `control_cancel_event`, `control_end_event`, `control_open_checkin`, `delete_personal_reminder`, `list_active_reminders`, `list_personal_reminders`, `publish_event`, `publish_event_with_groups`, and `upsert_personal_reminder`.
- Many other `SECURITY DEFINER` functions include `search_path=public`.

Interpretation:

- Production parity status: Confirmed.
- Candidate priority: Candidate P1.
- Production impact: Needs security hardening review.
- Suggested next step: `SECURITY DEFINER` hardening patch plan candidate.

## 12. BE-AUD-006 Production Status

Production evidence:

- Some target commerce/reservation RPCs exist and broad grants are visible.
- Full entitlement guard body review is incomplete from supplied partial outputs.

Interpretation:

- Production parity status: Needs verification.
- Candidate priority: Candidate P1 / Unknown.
- Production impact: Unknown.
- Suggested next step: Focused entitlement RPC body review.

## 13. BE-AUD-007 Production Status

Production evidence:

- All target sensitive tables exist and have `relrowsecurity = true`.
- Production policies were returned for target sensitive tables.
- Full policy correctness and redundancy/deduplication were not established from the supplied summary.

Interpretation:

- Production parity status: RLS enabled confirmed.
- Candidate priority: Downgrade RLS-enable unknown; policy correctness review remains.
- Production impact: Missing-RLS production concern is not supported by supplied evidence.
- Suggested next step: Policy correctness/de-duplication review.

## 14. BE-AUD-008 Production Status

Production evidence:

- Host transfer/admin functions exist, including `_transfer_audit`, `accept_host_transfer_v1`, `admin_approve_transfer_v1`, and `admin_execute_host_identity_transfer_v1`.
- `admin_execute_host_identity_transfer_v1` is `SECURITY DEFINER`, has `search_path=public`, and includes an `auth_is_ops()` gate.
- Broad execute grants were reported for `admin_execute_host_identity_transfer_v1`.

Interpretation:

- Production parity status: Existence confirmed.
- Candidate priority: Unknown / possible grant hardening candidate.
- Production impact: Not confirmed as exploitable because internal ops gate exists.
- Suggested next step: Ops/admin authority and grant surface review.

## 15. Candidate P0 / P1 Status After Production Verification

Candidate P0:

- None confirmed by supplied production evidence.
- BE-AUD-002 remains Candidate P0 only if `push-dispatch` is later deployed as externally callable without JWT/signature/caller-auth controls.

Candidate P1:

- BE-AUD-001: determinism/source-of-truth clarity.
- BE-AUD-005: live `SECURITY DEFINER` search-path hardening review.
- BE-AUD-006: entitlement guard parity remains incomplete.

Unknown or decision-dependent:

- BE-AUD-003: public media exposure priority depends on accepted product/security decision.
- BE-AUD-004: legacy check-in function review needed before priority can be set.
- BE-AUD-007: RLS enabled is confirmed; policy correctness review remains.
- BE-AUD-008: admin RPC existence is confirmed; grant surface and ops/admin model remain review topics.

## 16. Confirmed Production Evidence

- Target sensitive tables exist in `public` and have RLS enabled.
- Production policy surface exists for target sensitive tables.
- `avatars`, `venue-media`, and `venue-posters` are public buckets.
- Public read storage policies exist for the listed buckets.
- Owner/host-constrained storage write policies appear present in supplied policy output.
- `admin_execute_host_identity_transfer_v1` exists in production.
- `admin_execute_host_identity_transfer_v1` has `search_path=public` and an `auth_is_ops()` gate in supplied output.
- Some live `SECURITY DEFINER` functions lack search_path proconfig.
- `checkin_ticket_by_id_v2` denies anon/public execute in supplied grants.
- `checkin_ticket_v2_unsafe` appears denied for anon/authenticated/public.
- `push-dispatch` was not visible in the production Edge Functions dashboard.

## 17. Remaining Unknowns

- Production migration version parity remains Unknown / Needs verification.
- Full policy correctness for sensitive tables is not established.
- Full entitlement guard body coverage is not established.
- Active use and internal guards for legacy/related check-in functions are not established.
- Public media exposure acceptability is not established.
- Grant surface acceptability for broad RPC execute privileges is not established.
- Ops/admin authority model is not established.
- `push-dispatch` future deployment posture remains a guardrail concern.

## 18. ADR Candidates

These are candidate ADR topics only, not accepted ADRs:

- Public media semantics for `avatars`, `venue-media`, and `venue-posters`.
- RPC execute grant posture for `anon`, `authenticated`, `public`, `service_role`, and `postgres`.
- Ops/admin authority model for host identity transfer.
- Backend source-of-truth and overload convention for repeated RPC surfaces.
- Production verification approach when migration metadata is unavailable.

## 19. Patch Plan Candidates

These are candidate patch areas only, not accepted patch plans:

- `SECURITY DEFINER` search-path hardening for live functions without search_path proconfig.
- RPC grant least-privilege review for admin, commerce, reservation, check-in, and entitlement-related functions.
- Entitlement RPC body coverage review for ticket/reservation/wallet paths.
- Legacy check-in RPC review for active use, grants, and internal guard posture.
- Deployment guard for `push-dispatch` if it is introduced or reintroduced in production.

## 20. Non-Accepted Items

- No finding in this report is accepted as a final vulnerability.
- No remediation is accepted.
- No priority is final.
- No ADR is accepted.
- No patch plan is accepted.
- No final exploitability conclusion is made.
- No release readiness conclusion is made.

## 21. Recommended Next Step

The recommended next step is a concise follow-up verification pass focused on the remaining unknowns:

- Full policy correctness and redundancy review for sensitive tables.
- Focused entitlement RPC body review.
- Legacy check-in RPC active-use and guard review.
- RPC grant posture review, especially admin and broad execute surfaces.
- ADR decision for public media exposure semantics.

Only after those results are reviewed should the team decide whether any Candidate P0/P1 patch plan is needed.

## 22. No-Modification / No-Production-Mutation Confirmation

For this report:

- No JoinFolk application code repositories were inspected or modified.
- Dashboard, Mobile, and Web/Public application code were not inspected or modified.
- Codex did not connect to production.
- No production mutation was performed by Codex.
- Supabase CLI was not run.
- No migrations were run or generated.
- No SQL fixes were written.
- No implementation instructions were written.
- No patch plan was created.
- No files were staged or committed.

## 23. Open Questions

- What is the accepted product/security decision for public media buckets?
- Which broad RPC execute grants are intentional, and which rely on internal guards?
- Are the `SECURITY DEFINER` functions without search_path proconfig active and reachable through expected roles?
- Are legacy/related check-in functions still used by production workflows?
- Do entitlement RPC bodies fully cover ticket, reservation, wallet, pending gift claim, and legacy transfer paths?
- What is the canonical ops/admin authority model for host identity transfer?
- Is there an alternative source for production migration metadata?
- Should `push-dispatch` be blocked from future deployment unless inbound auth/signature controls are documented?

# Supabase Focused Backend Follow-Up Report

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Manual focused backend verification summary
- canonical: false

## 2. Purpose

This document is a draft focused backend follow-up report for JoinFolk Supabase/backend.

It is based on manual production SQL outputs and focused backend verification outputs supplied by the operator, plus the draft handbook audit reports. It is not a patch plan, not an accepted vulnerability list, not an accepted remediation list, and not a release readiness report.

No final exploitability or final patch priority is claimed. Backend/RPC/RLS/storage/auth remain the security-sensitive authority. Frontend UX is not security authority.

## 3. Follow-Up Scope

This follow-up is limited to:

- Sensitive table policy correctness and redundancy.
- Entitlement RPC body coverage for ticket/reservation/wallet/pending gift claim/legacy transfer paths.
- Legacy check-in RPC active-use and internal guard posture.
- RPC execute grant posture for broad grants.
- Public media ADR evidence needs.
- `SECURITY DEFINER` search_path hardening candidates.

`push-dispatch` remains outside this focused SQL pass because it was not visible/deployed in the production Edge Functions dashboard. It remains a dormant/local-code risk from the prior report and a future deployment guard topic.

## 4. Safety Constraints

- No JoinFolk application code repositories were inspected or modified.
- Dashboard, Mobile, and Web/Public application code were not inspected or modified.
- Codex did not connect to production.
- Supabase CLI was not run.
- No migrations were run or generated.
- No SQL fixes are included.
- No implementation instructions are included.
- No patch plan is created.
- No files were staged or committed.

## 5. Evidence Summary

The operator ran focused read-only production verification outputs. Some outputs were partial or truncated.

Prior production verification established that target sensitive tables exist and have RLS enabled, `relforcerowsecurity=false` for target tables, public buckets exist for `avatars`, `venue-media`, and `venue-posters`, public read policies exist for those buckets, and some live `SECURITY DEFINER` functions lack search_path proconfig.

Focused verification added evidence about:

- Policy count and command coverage for target sensitive tables.
- Public-role policy usage and policy constraints.
- Zero-policy posture for `tickets` and `event_ticket_claims_v1`.
- Entitlement helper function presence and guard architecture.
- RPC overload/source-of-truth ambiguity.
- Legacy check-in body keyword signals and grants.
- Broad execute grants across admin, transfer, ticket, reservation, check-in, media, and notification categories.
- `SECURITY DEFINER` functions missing search_path while executable by anon/public/authenticated roles.

## 6. Focus Area Results Summary

| Focus area | Evidence status | Interpretation | Candidate status |
| --- | --- | --- | --- |
| Policy correctness and redundancy | Partial evidence supplied | Policy surface exists; correctness and redundancy still need human review | Unknown / Needs verification |
| Entitlement RPC coverage | Guard architecture present; overload ambiguity remains | Positive architecture, incomplete coverage conclusion | Candidate P1 / Needs verification |
| Legacy check-in RPC guard posture | Current scanner path looks constrained; proof-related functions are concerning if externally reachable | Final exploitability not claimed | Conditional Candidate P0 or Candidate P1 review |
| RPC execute grant posture | Broad grants visible across many categories | Not automatically exploitable if internal guards are correct | Candidate P1 review |
| Public media ADR evidence | Public buckets and public read confirmed | Product/security decision required | ADR/security decision |
| `SECURITY DEFINER` search_path | Live missing search_path functions confirmed | Hardening concern; exploitability not assumed | Candidate P1 |

## 7. Policy Correctness and Redundancy

Evidence:

- `commerce_orders` has one `ALL` policy for authenticated role with false qualifier, interpreted as deny-all style.
- `event_media` has five policies, including four `SELECT` policies and one `UPDATE` policy; roles aggregate as public.
- `event_staff_assignments` has four policies covering select/insert/update/delete; roles aggregate as public.
- `event_ticket_claims_v1` has zero policies.
- `events` has twelve policies with anon, authenticated, and public roles; all primary table access command classes are represented.
- `notifications_v2`, `push_tokens_v1`, and `user_notification_settings_v1` have authenticated-role policies.
- `reservations` has three policies, including public-role usage.
- `tickets` has zero policies.
- `venue_media` and `venues` have authenticated-role policy surfaces.

Interpretation:

- Policy surface exists for most target tables.
- Some public-role policies appear intentionally constrained through `auth.uid()`, host ownership, participant/staff state, or visibility rules.
- `tickets` and `event_ticket_claims_v1` having zero policies is not a bug by itself because RLS was previously confirmed enabled; it means direct table access likely defaults to deny and RPC internal guards become critical.
- Full policy correctness, redundancy, and intended public-role usage remain Unknown / Needs verification.

## 8. Entitlement RPC Coverage

Evidence:

- Helper functions exist and are `SECURITY DEFINER` with search_path configured.
- `_assert_narrow_ticket_acquisition_conflict_v1` checks non-null user input, calls active reservation, active gift entitlement, and recipient pending claim helpers, and raises `ACTIVE_ENTITLEMENT_EXISTS` or `RECIPIENT_ALREADY_HAS_PENDING_CLAIM`.
- `_has_active_reservation_v1` checks reservations for event/user/session and pending/active/approved statuses.
- `_has_active_gift_entitlement_v1` checks gift claim/gift transfer tickets and pending `event_ticket_claims_v1`.
- `_has_active_recipient_pending_claim_v1` checks pending unexpired recipient claims.
- `create_commerce_order_v1` references the narrow ticket acquisition guard for self or `p_for_user_id`.
- `create_reservation_v2` mentions reservation and pending claim signals, but full interpretation is incomplete.
- `create_ticket_claim_v1` references active entitlement and helper checks.
- `purchase_event_ticket_v4` has multiple overloads; one overload mentions the narrow ticket guard and another does not.
- `purchase_event_ticket_v5` mentions the narrow ticket guard.
- `request_ticket_v2` output was partial/truncated.

Interpretation:

- Entitlement guard architecture is present and largely positive.
- Overload/source-of-truth ambiguity remains, especially around multiple `purchase_event_ticket_v4` and reservation surfaces.
- Coverage cannot be called complete from the supplied evidence.
- Candidate priority: Candidate P1 / Needs verification for entitlement overload/source-of-truth review.

## 9. Legacy Check-In RPC Guard Posture

Evidence:

- `checkin_ticket_by_id_v2` is `SECURITY DEFINER`, has search_path configured, denies anon/public execute, allows authenticated execute, and checks `auth.uid()`, event host ownership, live event status, ticket event scope, code match, and ticket status state.
- `staff_checkin_ticket_v1` references `auth.uid()`, host, staff, `event_staff_assignments`, ticket owner/user signals, and raises `AUTH_REQUIRED`.
- `checkin_ticket_v2` references `auth.uid()`, ticket owner/user signals, and raises `AUTH_REQUIRED`.
- `check_in_ticket` has broad anon/authenticated grants but checks event host ownership through `auth.uid()` and event/check-in status.
- `assert_checkin_open` has broad anon/authenticated grants and checks lifecycle/check-in timing, but appears helper-style and does not itself verify caller authority.
- `control_open_checkin` is `SECURITY DEFINER`, lacks search_path in focused output, has broad anon/authenticated grants, and checks host ownership through `auth.uid()`.
- Proof-related functions including `ensure_ticket_checkin_proof_v1`, `record_checkin_proof_v1`, and `remove_ticket_checkin_proof_v1` lack visible keyword signals for `auth.uid()`, host, staff, or `event_staff_assignments` in the supplied scan and accept caller-provided event/user/ticket/proof parameters.

Interpretation:

- Current `checkin_ticket_by_id_v2` scanner path is a positive control, not a patch candidate from this evidence.
- `staff_checkin_ticket_v1` shows staff/host guard signals.
- Proof-related check-in functions are concerning if externally reachable and able to mutate check-in proof state without caller authority.
- Candidate status: Candidate P0 if proof-related RPCs are externally reachable and able to mutate check-in proof state without caller authority; otherwise Candidate P1 reachability/hardening review.
- Final exploitability is not claimed.

## 10. RPC Execute Grant Posture

Evidence:

- Broad execute grants were visible on many functions where anon/public/authenticated can execute.
- `admin_execute_host_identity_transfer_v1` is `SECURITY DEFINER`, has `search_path=public`, and is broadly executable; prior evidence showed an internal `auth_is_ops()` gate.
- Check-in-related broad grant examples include `assert_checkin_open`, `control_open_checkin`, `get_event_checkin_summary`, `get_my_event_checkin_truth_v1`, `public_verify_checkin`, and `record_checkin_proof_v1`.
- Media functions are often `SECURITY DEFINER` and broadly executable, including examples such as `create_event_media_v1/v2`, `create_media_comment_v1/v2`, `delete_owned_media_v1`, `host_moderate_media_v1`, `list_venue_media_v1`, `remove_venue_media_v1`, and `update_venue_media_v1`.
- Reservation examples include `create_reservation_v1/v2`, `create_venue_reservation_v1/v2`, `update_reservation_status_v1`, `decide_venue_reservation_v2`, and `get_host_reservation_inbox_v1`.
- Ticket examples include `_issue_tickets_from_order_v1`, `approve_ticket_v2`, `approve_ticket_v2_unsafe`, `check_in_ticket`, `claim_ticket_v1`, `create_ticket_claim_v1`, `create_ticket_order_v1`, `ensure_ticket_checkin_proof_v1`, and `get_event_ticket_products_v1`.

Interpretation:

- Broad execute grants are not automatically exploitable if internal guards are correct.
- Admin, transfer, ticket, reservation, check-in, media, and notification categories warrant least-privilege/grant-surface review.
- Candidate priority: Candidate P1 review, not confirmed exploitability.

## 11. SECURITY DEFINER Search Path Hardening Candidates

Evidence:

- Live `SECURITY DEFINER` functions with `proconfig = null` were confirmed.
- Examples include `control_cancel_event`, `control_end_event`, `control_open_checkin`, `delete_personal_reminder`, `list_active_reminders`, `list_personal_reminders`, `publish_event`, `publish_event_with_groups`, and `upsert_personal_reminder`.
- Several are executable by anon/public/authenticated roles.
- Some bodies include `auth.uid()` ownership checks, such as host-owned event updates or owned reminder filtering.
- `publish_event_with_groups` raises a deprecated exception in supplied output.

Interpretation:

- Missing search_path on live `SECURITY DEFINER` functions is confirmed.
- Internal ownership checks reduce but do not remove the hardening concern.
- Exploitability is not assumed.
- Candidate priority: Candidate P1.

## 12. Public Media ADR Evidence

Evidence:

- Prior production verification confirmed `avatars`, `venue-media`, and `venue-posters` are public buckets.
- Public read policies exist for those buckets.
- Storage write policies appeared owner/host constrained from supplied output.

Interpretation:

- Public read exposure is confirmed.
- This is not automatically a bug.
- Public media semantics require an accepted product/security decision.
- Candidate status: ADR/security decision unless product/security decision says otherwise.

## 13. Candidate P0 / P1 Status After Focused Verification

Candidate P0:

- Proof-related check-in functions only if externally reachable and able to mutate check-in proof state without caller authority.
- No final Candidate P0 is accepted by this report.

Candidate P1:

- `SECURITY DEFINER` functions missing search_path.
- Broad RPC execute grant review across admin, transfer, ticket, reservation, check-in, media, and notification categories.
- Entitlement overload/source-of-truth review.
- Proof-related check-in function reachability/hardening review if Candidate P0 conditions are not established.

Unknown / decision-dependent:

- Sensitive table policy correctness and public-role policy intent.
- Public media exposure priority pending ADR/security decision.
- `tickets` and `event_ticket_claims_v1` zero-policy posture, because RPC-only security reliance depends on internal guard correctness.
- `push-dispatch` remains dormant/local-code risk from prior report and is not a focused SQL finding here.

## 14. Confirmed Positive Controls

- Target sensitive tables were previously confirmed to exist with RLS enabled.
- `commerce_orders` has deny-all style policy evidence.
- `tickets` and `event_ticket_claims_v1` have zero policies with RLS enabled, suggesting direct table access likely defaults to deny.
- Entitlement helper architecture exists and includes active reservation, gift entitlement, and pending recipient claim checks.
- `checkin_ticket_by_id_v2` denies anon/public execute and has visible host/status/ticket/code checks.
- `staff_checkin_ticket_v1` shows staff/host guard signals.
- `admin_execute_host_identity_transfer_v1` has prior evidence of an `auth_is_ops()` gate.
- Public media write policies appeared owner/host constrained from supplied output.

## 15. Confirmed Concerns

- Live `SECURITY DEFINER` functions without search_path are confirmed.
- Broad execute grants are confirmed across multiple sensitive function categories.
- Proof-related check-in functions have concerning keyword-scan signals if externally reachable.
- Entitlement RPC overload/source-of-truth ambiguity remains.
- Public media buckets and public read policies are confirmed and require ADR/security decision.
- Public-role policies exist on several sensitive tables and require correctness review.

## 16. Remaining Unknowns

- Whether proof-related check-in functions are externally reachable in a way that can mutate check-in proof state without caller authority.
- Whether all broad-grant RPCs have adequate internal guards.
- Whether all entitlement acquisition, ownership, pending gift claim, and legacy transfer paths are covered by active guards.
- Which overloads are active and canonical for ticket/reservation purchase flows.
- Whether public-role table policies match intended product/security semantics.
- Whether public media exposure has an accepted product/security decision.
- Whether functions missing search_path are reachable in exploitable contexts.
- Whether legacy check-in functions are still used by production workflows.

## 17. ADR Candidates

These are candidate ADR topics only, not accepted ADRs:

- Public media semantics for `avatars`, `venue-media`, and `venue-posters`.
- Public-role policy usage on sensitive tables.
- RPC execute grant posture and reliance on internal guards.
- RPC-only security model for tables with RLS enabled and zero direct policies.
- Canonical source-of-truth and overload convention for entitlement-related RPCs.

## 18. Patch Plan Candidates

These are candidate patch areas only, not accepted patch plans:

- `SECURITY DEFINER` search-path hardening for live functions without search_path proconfig.
- RPC grant surface review for admin, transfer, ticket, reservation, check-in, media, and notification functions.
- Proof-related check-in RPC reachability and guard review.
- Entitlement RPC overload and active guard coverage review.
- Sensitive table policy correctness and redundancy review.

## 19. Non-Accepted Items

- No finding in this report is accepted as a final vulnerability.
- No remediation is accepted.
- No priority is final.
- No ADR is accepted.
- No patch plan is accepted.
- No final exploitability conclusion is made.
- No release readiness conclusion is made.
- No production mutation task is authorized by this report.

## 20. Recommended Next Step

Recommended next step:

- Perform body-level human review of proof-related check-in RPCs to determine external reachability, mutating behavior, and caller authority checks.
- Perform focused entitlement overload review to identify canonical active purchase/reservation/claim paths.
- Review broad execute grants against internal guard evidence.
- Decide whether public media exposure and public-role policies need ADRs.
- Turn only evidence-backed, accepted concerns into a separate Candidate P0/P1 patch plan later if needed.

## 21. No-Modification / No-Production-Mutation Confirmation

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

## 22. Open Questions

- Are proof-related check-in RPCs externally reachable by anon/public/authenticated callers?
- Can proof-related check-in RPCs mutate check-in proof state without caller authority?
- Which ticket/reservation/purchase/claim RPC overloads are canonical and active?
- Do all active entitlement paths call the intended guard helpers?
- Which broad execute grants are intentional because of internal guards?
- Which broad execute grants should remain broad for product/API compatibility?
- Are public-role policies on sensitive tables intentional and correctly constrained?
- Is the public media bucket posture accepted by product/security decision?
- Which `SECURITY DEFINER` functions without search_path are reachable by external roles?

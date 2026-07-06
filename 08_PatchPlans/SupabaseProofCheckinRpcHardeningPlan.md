# Supabase Proof Check-In RPC Hardening Plan

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Manual focused backend verification summary
- canonical: false

## 2. Purpose

This document is a draft candidate patch plan for proof-related check-in RPC hardening in JoinFolk Supabase/backend.

It is not an accepted patch plan, not an approved remediation, not an accepted vulnerability list, and not a release readiness report. It does not include SQL, migration code, implementation code, or instructions to run mutating commands.

No final exploitability or final priority is claimed. Backend/RPC/RLS/storage/auth are the security-sensitive authority. Frontend UX is not security authority.

## 3. Candidate Patch Plan Status

- Plan status: Draft candidate only.
- Approval status: Not approved for execution.
- Remediation status: Not accepted.
- Priority status: Not final.
- Production vulnerability status: Not claimed.
- Canonical status: false.

## 4. Scope

In scope:

- Candidate hardening review for proof-related check-in RPCs.
- Reachability and active-use verification for proof-related check-in RPCs.
- Caller authority requirements for proof-state mutation.
- Grant-surface review for the primary functions of concern.
- Compatibility review against the current positive scanner path.

Primary functions of concern:

- `ensure_ticket_checkin_proof_v1`
- `record_checkin_proof_v1`
- `remove_ticket_checkin_proof_v1`

Related functions for boundary comparison:

- `checkin_ticket_by_id_v2`
- `checkin_ticket_v2`
- `check_in_ticket`
- `staff_checkin_ticket_v1`
- `issue_checkin_proof`
- `public_verify_checkin`
- `get_event_checkin_summary`
- `get_my_event_checkin_truth_v1`
- `undo_checkin_ticket_v2`
- `undo_checkin_ticket_v2_unsafe`

## 5. Source-Path Assumption

This draft is based on production SQL/operator evidence plus handbook audits.

The local implementation target is not yet canonical. `C:\dev\hostos\supabase` is the primary plausible backend source for current audit purposes, but it is not canonical until competing Supabase directories are classified.

Competing Supabase paths exist:

- `C:\dev\joinfolk-web\supabase`
- `C:\dev\hostos\apps\mobile\supabase`

This draft must not be treated as implementation-ready until the target Supabase source path is named and verified. Production SQL evidence outranks local source assumptions for production-state claims.

## 6. Non-Goals

- This draft does not modify Supabase/backend code.
- This draft does not include SQL fixes.
- This draft does not include ready-to-run SQL.
- This draft does not include migration code.
- This draft does not authorize production changes.
- This draft does not decide final exploitability.
- This draft does not decide final priority.
- This draft does not change the accepted product check-in model.
- This draft does not treat frontend UX as security evidence.

## 7. Evidence Summary

Known positive control evidence for `checkin_ticket_by_id_v2`:

- `SECURITY DEFINER`.
- search_path includes `public, extensions`.
- `can_execute_anon = false`.
- `can_execute_public = false`.
- `can_execute_authenticated = true`.
- Body checks `auth.uid()`.
- Body checks event `host_id`.
- Body checks event status `live`.
- Body checks ticket `event_id` scope.
- Body checks ticket code match.
- Body checks ticket status state machine.

Related boundary evidence:

- `check_in_ticket` has broad anon/authenticated execution, but body checks event `host_id = auth.uid()` and event/check-in status.
- `staff_checkin_ticket_v1` keyword scan shows `auth.uid()`, `host_id`, staff, `event_staff_assignments`, ticket owner/user_id, and `AUTH_REQUIRED` signals.

Proof-related function concern evidence:

- `ensure_ticket_checkin_proof_v1` had broad grants observed: anon, authenticated, public, and service_role. Keyword scan lacked `auth.uid()`, `host_id`, staff, and `event_staff_assignments`. It accepts caller-provided `p_event_id`, `p_user_id`, `p_ticket_id`, `p_method`, and `p_proof_ref`.
- `record_checkin_proof_v1` had broad grants shown in the broad grant matrix. Keyword scan lacked `auth.uid()`, `host_id`, staff, and `event_staff_assignments`. It accepts caller-provided `p_event_id`, `p_user_id`, `p_method`, `p_ticket_id`, `p_proof_ref`, and `p_session_id`.
- `remove_ticket_checkin_proof_v1` keyword scan lacked `auth.uid()`, `host_id`, staff, and `event_staff_assignments`. It accepts `p_event_id`, `p_user_id`, and `p_ticket_id`.

Prior focused classification:

- Candidate P0 if proof-related RPCs are externally reachable and able to mutate check-in proof state without caller authority.
- Otherwise Candidate P1 reachability/hardening review.

## 8. Risk Hypothesis

The risk hypothesis is that proof-related check-in RPCs may expose proof-state mutation paths that rely on caller-provided event, user, ticket, method, or proof parameters without sufficient caller authority checks.

This is a hypothesis, not a final exploitability claim. Production vulnerability is not claimed by this draft.

## 9. Candidate Priority

- Candidate P0 if externally reachable and able to mutate check-in proof state without caller authority.
- Otherwise Candidate P1 reachability/hardening review.

This priority is conditional and not final.

## 10. Functions in Scope

Primary hardening targets:

- `ensure_ticket_checkin_proof_v1`
- `record_checkin_proof_v1`
- `remove_ticket_checkin_proof_v1`

Boundary comparison targets:

- `checkin_ticket_by_id_v2`
- `checkin_ticket_v2`
- `check_in_ticket`
- `staff_checkin_ticket_v1`
- `issue_checkin_proof`
- `public_verify_checkin`
- `get_event_checkin_summary`
- `get_my_event_checkin_truth_v1`
- `undo_checkin_ticket_v2`
- `undo_checkin_ticket_v2_unsafe`

## 11. Functions Out of Scope

Out of scope for this candidate plan:

- Non-check-in entitlement RPCs.
- Public media storage policies.
- General `SECURITY DEFINER` search_path hardening outside proof/check-in RPCs.
- `push-dispatch`, which was not visible/deployed in the production Edge Functions dashboard and remains a separate future deployment guard topic.
- Dashboard, Mobile, or Web/Public application code.

## 12. Positive Control Baseline

The positive control baseline is the current scanner path represented by `checkin_ticket_by_id_v2`:

- Authenticated execution only for normal client roles.
- Caller identity is derived from backend auth context.
- Event host authority is checked.
- Event status is checked.
- Ticket scope is tied to the event.
- Ticket code is checked.
- Ticket status transition rules are checked.

This plan should keep current positive scanner path behavior intact unless a separate approval changes that path.

## 13. Proof RPC Concern Summary

The proof-related RPC concern is narrower than general check-in behavior:

- The primary concern is proof-state mutation based on caller-provided identifiers.
- `p_user_id` or equivalent caller-provided user identifiers cannot be trusted without authority validation.
- `p_method` and `p_proof_ref` cannot authorize an action by themselves.
- `p_event_id` and `p_ticket_id` must be scoped together.
- Caller authority should be derived from backend auth context and verified against event/ticket/staff/owner state.

No conclusion is made here that the functions are exploitable or that production is vulnerable.

## 14. Required Pre-Patch Verification

Before promoting this draft, verify:

- Whether each primary proof-related RPC is active in production.
- Whether each primary proof-related RPC is externally reachable by anon, public, authenticated, service_role, or internal-only callers.
- Whether each function mutates proof state, reads proof state, or acts as an internal helper.
- Which approved product paths call each function.
- Whether current scanner flow depends on these functions.
- Whether staff scanner flow depends on these functions.
- Whether public verification is read-only and intentionally public.
- Whether deprecated or unsafe variants are still reachable.

## 15. Proposed Patch Strategy

This strategy is conceptual only and is not approved for execution.

- Verify active usage and external reachability.
- Identify whether proof-related functions should be classified as internal-only helper RPCs, authenticated-only RPCs, host/staff gated RPCs, or replaced by the current scanner path.
- Require caller authority checks for any proof-state mutation.
- Require backend auth context for mutation paths.
- Allow event host authority where product semantics require host check-in control.
- Allow assigned staff authority where product semantics allow staff check-in control.
- Allow ticket owner authority only where product semantics allow owner proof access or mutation.
- Ensure event and ticket identifiers are scoped together before proof state is trusted.
- Treat caller-provided user identifiers as claims that require authority validation, not as authority by themselves.
- Treat method and proof reference values as metadata, not authorization.
- Prefer least-privilege execute grants after internal guards are confirmed.
- Keep the current positive scanner path intact unless separately approved.
- Avoid breaking legitimate check-in proof generation from the current scanner flow.

## 16. Security Invariants

Candidate security invariants:

- Anonymous callers cannot mutate check-in proof state.
- Public role execution cannot mutate check-in proof state unless the function is proven internal-only and unreachable through external client paths.
- Authenticated callers cannot create or remove proof state for another user without host, staff, or accepted ticket-owner authority.
- Caller-provided `user_id` cannot override backend caller identity.
- `event_id` and `ticket_id` must match a valid ticket/event relationship.
- Proof mutation must respect event/check-in lifecycle state where product semantics require it.
- Unsafe or deprecated paths must not remain externally callable unless explicitly accepted.
- Read-only public verification, if required by product semantics, must remain read-only.

## 17. Backward Compatibility Considerations

Compatibility areas to review before any accepted change:

- Current scanner flow using `checkin_ticket_by_id_v2`.
- Staff scanner flow using `staff_checkin_ticket_v1`, if accepted as current.
- Any legitimate proof creation path used after successful check-in.
- Public verification behavior, if product requires public proof verification.
- Ticket owner proof visibility, if product requires owner-facing proof display.
- Existing event host workflows for opening check-in or viewing summaries.
- Deprecated or unsafe paths that may still be called by older clients.

## 18. Testing Strategy

Read-only verification should happen before any accepted patch work. Local/staging tests should precede production consideration.

Positive tests:

- Host can create/check proof through an approved path.
- Assigned staff can create/check proof through an approved path if product allows.
- Ticket owner can read own proof where product allows.

Negative tests:

- Anonymous caller cannot mutate proof state.
- Unrelated authenticated user cannot create proof for another user.
- Unrelated authenticated user cannot remove proof for another user.
- Caller-provided `user_id` cannot override authority.
- Mismatched `event_id` and `ticket_id` are rejected.
- Removed or deprecated unsafe path cannot be called by anon/public/authenticated if not needed.

Regression tests:

- `checkin_ticket_by_id_v2` still works.
- `staff_checkin_ticket_v1` still works if accepted current path.
- Public verification remains read-only if product requires it.

## 19. Rollback / Recovery Considerations

Rollback and recovery planning should be defined only after an accepted implementation approach exists.

Candidate considerations:

- Preserve the currently working scanner path as the baseline.
- Keep a verified list of approved check-in RPCs before changes are considered.
- Ensure any grant or authority posture change has a documented compatibility assessment.
- Ensure operational support can distinguish proof generation failure from check-in failure.
- Prefer staged validation before production consideration.

## 20. Deployment Safety

This draft does not authorize deployment.

Deployment safety gates before any future accepted work:

- Production reachability and active-use evidence reviewed.
- Security invariants accepted.
- Backward compatibility impact reviewed.
- Local/staging test plan accepted.
- Rollback/recovery approach documented.
- Separate migration/SQL patch plan approved later if needed.
- Production rollout owner identified.

## 21. Manual Verification Checklist

Manual verification items before promoting this draft:

- Confirm which proof-related RPCs are active.
- Confirm which roles can execute each proof-related RPC.
- Confirm whether each function mutates or reads proof state.
- Confirm whether each function checks backend caller identity.
- Confirm whether each function checks host or staff authority.
- Confirm whether ticket owner authority is allowed by product semantics.
- Confirm whether `event_id` and `ticket_id` are scoped together.
- Confirm whether caller-provided `user_id` is validated against caller authority.
- Confirm whether public verification is read-only.
- Confirm whether current scanner and staff scanner flows depend on these proof RPCs.

## 22. Open Questions

- Are `ensure_ticket_checkin_proof_v1`, `record_checkin_proof_v1`, or `remove_ticket_checkin_proof_v1` externally reachable?
- Do any primary proof-related RPCs mutate proof state without caller authority?
- Which approved product path should own proof creation?
- Should proof mutation be host-only, staff-enabled, ticket-owner-enabled, or internal-only?
- Is `public_verify_checkin` intended to be public read-only?
- Are any older clients still using deprecated or unsafe check-in/proof paths?
- Should proof generation be coupled only to successful scanner check-in?
- Which execute grants are required for backward compatibility?
- Which Supabase source tree should receive any future accepted proof check-in RPC hardening migration?

## 23. Acceptance Criteria for Promoting This Draft

This draft can be considered for promotion only after:

- Canonical target Supabase source path is confirmed.
- Production reachability is confirmed.
- Active-use/caller-path is confirmed.
- Security invariants are accepted.
- Backward compatibility impact is reviewed.
- Manual/staging test plan is accepted.
- A separate migration/SQL patch plan is approved later if needed.
- The team explicitly accepts a Candidate P0 / Candidate P1 classification based on verified evidence.

## 24. No-Modification / No-Production-Mutation Confirmation

For this draft:

- No JoinFolk application code repositories were inspected or modified.
- Dashboard, Mobile, and Web/Public application code were not inspected or modified.
- Codex did not connect to production.
- No production mutation was performed by Codex.
- Supabase CLI was not run.
- No migrations were run or generated.
- No SQL fixes were written.
- No ready-to-run SQL was written.
- No Supabase/backend code was modified.
- No files were staged or committed.

# Security Definer and Function Grant Hardening Patch Plan

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: PP-01 metadata evidence and committed decision records
- canonical: false
- Plan status: Proposed; not approved for implementation
- Implementation status: Not authorized
- Production mutation status: Not executed
- Legal status: Engineering security patch plan only; not legal advice

## 2. Purpose

This plan translates the committed security/function grant decision into a controlled future implementation plan.

The plan is intended to prepare, scope, sequence, verify, and rollback a future function grant / SECURITY DEFINER hardening patch.

- This is not implementation.
- This does not authorize SQL.
- This does not authorize migration creation.
- This does not authorize production access.
- This does not authorize source changes.

## 3. Evidence Boundary

This document is based only on sanitized PP-01 metadata evidence and committed handbook decision records.

No new production access, SQL, CLI, dashboard action, source inspection, private data inspection, storage object listing, build, test, dependency install, migration, verification query, or implementation was performed.

No credentials, hostnames, full project refs, connection strings, service_role keys, anon keys, JWT secrets, tokens, webhook secrets, API keys, environment variable values, private row values, storage object names, storage object paths, message bodies, auth user rows, tickets/orders/reservations/claims rows, diagnostics payloads, notification payloads, support notes, or payment payloads are included.

## 4. Plan Status

Plan status: Proposed / Draft.

This plan is not approved for implementation.

Owner review is required before implementation.

Exact SQL/migration must be generated only in a later approved implementation step.

This plan must not be treated as launch approval.

## 5. Decision Inputs

- SecurityDefinerAndFunctionGrantHardeningDecision.md: Defines the intended hardening approach for broad function EXECUTE grants, SECURITY DEFINER posture, missing proconfig/search_path candidates, and future verifier-role boundaries.
- RLSPolicyAndGrantMatrixClassification.md: Requires table grants, RLS policies, relation sensitivity, and function-mediated paths to be classified together before access assumptions are accepted.
- SupabaseMigrationSourceOfTruthDecision.md: Requires migration provenance and rollback posture before future SQL or migration work is generated.
- RLSDisabledRelationTriageDecision.md: Requires RLS-disabled relations to be classified so function-mediated access does not hide relation-level exposure.
- StorageBucketExposureDecision.md: Requires storage/media access paths and signed URL behavior to be cross-referenced where functions touch storage.
- EdgeFunctionDeploymentInventoryDecision.md: Requires Edge Function dependencies to be classified where functions are called by deployed serverless paths.

## 6. PP-01 Evidence Summary

Known PP-01 evidence and follow-up records reported:

- Verifier table/storage direct access closed.
- Verifier had 0 effective table privilege rows for public/auth/storage/realtime.
- Storage schema usage/select false.
- No private rows inspected.
- No storage objects listed.
- No functions invoked.
- No mutation executed.
- Verifier revoked / NOLOGIN confirmed.
- Function EXECUTE grants were broader than expected.
- Verifier had public function execute exposure and security_definer execute exposure.
- Missing proconfig candidate list exists.
- No exploitability claim.
- No implementation authorized.

## 7. Problem Statement

Broad EXECUTE grants create unclear caller authority boundaries.

SECURITY DEFINER functions can bypass caller table privileges by design.

Missing proconfig/search_path creates hardening concern.

Function exposure must be classified before revocation or grant changes.

Revoking too broadly may break app/dashboard/runtime flows.

Metadata alone does not prove exploitability.

Leaving function grant/proconfig posture unknown blocks launch-ready claims.

## 8. Patch Goals

- Classify app-owned functions by intended caller role.
- Identify which EXECUTE grants are intentional.
- Identify which EXECUTE grants are accidental/broad/inherited.
- Harden app-owned SECURITY DEFINER functions with explicit proconfig/search_path in future implementation.
- Reduce accidental PUBLIC/anon/authenticated exposure where not intended.
- Preserve intended app/dashboard/mobile/web behavior.
- Avoid Supabase-managed schema changes unless separately approved.
- Create rollback and verification path before any production mutation.

## 9. Explicit Non-Goals

- No SQL in this plan.
- No migration in this plan.
- No source changes.
- No production access.
- No production mutation.
- No function body rewrite.
- No Supabase-managed schema change.
- No Edge Function change.
- No storage policy change.
- No RLS/table grant change except future cross-referenced implementation if approved.
- No launch approval.
- No legal/compliance claim.

## 10. Function Scope Classification

| Scope bucket | Description | Default patch posture |
|---|---|---|
| App-owned public schema RPCs | Public schema functions owned by the app. | Classify intended caller role before any grant/proconfig change. |
| App-owned SECURITY DEFINER RPCs | App functions that execute with definer privileges. | Highest priority for grant/proconfig classification and careful verification. |
| App-owned SECURITY INVOKER RPCs | App functions that execute with caller privileges. | Classify grants and caller expectations, but definer-specific hardening may not apply. |
| Admin/ops/support RPCs | Functions supporting privileged operational workflows. | Require PP-08 authority/auditability cross-reference before preserving exposure. |
| Event lifecycle RPCs | Functions for publish, check-in, lifecycle, host, or event operations. | Require host/staff/participant dependency mapping and smoke tests. |
| Commerce/ticket/reservation RPCs | Functions for orders, tickets, reservations, check-in proof, refunds, or commerce state. | Require commerce owner review and strict caller mapping. |
| Messaging/notification RPCs | Functions for DM, notification, reminder, or delivery behavior. | Require privacy and notification dependency review before grant changes. |
| Media/storage RPCs | Functions touching media rows, signed URLs, uploads, or storage metadata. | Cross-reference storage exposure decision and media lifecycle constraints. |
| Deprecated/legacy RPCs | Functions no longer expected to serve active flows. | Restrict, remove, or document only after dependency review and owner approval. |
| Supabase-managed/platform functions | Functions owned by auth/storage/realtime/platform schemas. | Exclude unless a separate platform-managed schema decision approves review. |
| Unknown ownership functions | Functions whose owner/source is not classified. | Defer and block launch-ready claim until classified if security-impacting. |

## 11. Candidate Missing proconfig List

- `control_cancel_event(event_id uuid)`
- `control_end_event(event_id uuid)`
- `control_open_checkin(event_id uuid)`
- `delete_personal_reminder(p_id uuid)`
- `list_active_reminders()`
- `list_personal_reminders()`
- `publish_event(p_event_id uuid, p_visibility text)`
- `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])`
- `upsert_personal_reminder(p_id uuid, p_title text, p_note text, p_reminder_date date, p_color_theme text, p_remind_before_days integer)`

These are hardening candidates only.

They are not proven exploitable.

They require dependency and body/gate review before implementation.

No function body change is authorized by this plan.

## 12. Function EXECUTE Grant Patch Classes

| Patch class | Description | Future action type | Risk |
|---|---|---|---|
| Preserve explicit anon/public-safe execute | Function is intentionally callable by unauthenticated/public callers. | Preserve with documented justification. | Breakage risk if incorrectly restricted; exposure risk if misclassified. |
| Preserve authenticated app execute | Function is intentionally callable by signed-in app users. | Preserve for authenticated role with dependency tests. | Medium if caller policy/body assumptions are wrong. |
| Restrict accidental anon execute | Function should not be callable by unauthenticated callers. | Future revoke/restrict after dependency review. | High if left open; breakage risk if app uses anon path. |
| Restrict accidental authenticated execute | Function should not be broadly callable by signed-in users. | Future revoke/restrict after caller mapping. | High for privileged functions. |
| Restrict PUBLIC/inherited execute | Function exposure is inherited broadly rather than explicitly intended. | Future explicit grant model after review. | High because inherited exposure obscures authority. |
| Preserve service_role/internal execute | Function is internal-only or server-side. | Preserve only for intended internal role/path. | Medium if internal path leaks to clients. |
| Move to function-mediated controlled access | Access should occur through a narrower wrapper or app-controlled boundary. | Future design decision, not first hardening patch by default. | High implementation complexity. |
| Defer pending app dependency review | Function dependency is unclear. | No change until classified. | Unknown; blocks launch-ready claim if security-impacting. |
| Exclude Supabase-managed/platform function | Function belongs to platform-managed schema. | Exclude unless separately approved. | High if modified without platform boundary decision. |

## 13. Proposed Patch Phases

### Phase 0 — Preconditions

- Owner approval required.
- Sanitized function inventory required.
- App/dashboard RPC dependency mapping required.
- Migration source-of-truth decision reviewed.
- Rollback posture drafted.
- No production action yet.

### Phase 1 — Classification

- Classify functions by owner/schema.
- Classify SECURITY DEFINER vs SECURITY INVOKER.
- Classify caller roles.
- Classify app/dashboard/mobile/web dependency.
- Classify PUBLIC/anon/authenticated/service_role exposure.
- Classify missing proconfig candidates.

### Phase 2 — Patch Design

- Decide explicit grants to preserve.
- Decide accidental grants to revoke.
- Decide proconfig/search_path additions.
- Decide excluded Supabase-managed functions.
- Decide deferred functions.
- Write rollback notes.
- Still no production mutation.

### Phase 3 — Implementation Candidate

- Create future migration or controlled SQL only after explicit owner approval.
- No destructive function drop.
- No function body rewrite unless separately approved.
- Include rollback notes.
- Split into small scoped patch if risk is high.

### Phase 4 — Verification

- Metadata verification.
- App/dashboard smoke tests.
- Anon negative tests.
- Authenticated positive/negative tests.
- Host/staff/admin path tests where relevant.
- Rollback verification.

### Phase 5 — Acceptance

- Update verification report.
- Document residual risks.
- Do not claim launch-ready unless all P0/P1 gates pass.

## 14. Required Future Inventory

Required sanitized inventory columns:

- schema
- function_name
- argument_signature
- owner/source classification
- security mode
- proconfig/search_path state
- volatility category if available
- current anon EXECUTE exposure
- current authenticated EXECUTE exposure
- current service_role/internal exposure
- PUBLIC/inherited exposure
- app dependency
- dashboard dependency
- mobile/web dependency
- Edge Function dependency
- storage/RLS dependency
- target caller role
- target grant posture
- risk class: P0/P1/P2/Unknown
- patch action: preserve/restrict/harden/defer/exclude
- rollback requirement
- verification requirement
- owner approval required

This inventory must use sanitized metadata only.

## 15. Implementation Candidate Rules

- No implementation without exact function list.
- No implementation without dependency classification.
- No implementation without rollback notes.
- No implementation without owner approval.
- Avoid function body changes in first hardening patch.
- Prefer grant/proconfig-only patch for first scope.
- Exclude Supabase-managed schemas unless separate decision.
- Do not bundle unrelated RLS/storage/Edge changes.
- Split high-risk function families into separate patches if needed.

## 16. Verification Plan

- Diff review.
- Migration diff review if SQL is later created.
- No private data inspection.
- No storage object listing.
- Metadata-only verification where possible.
- Verify intended grants after patch.
- Verify revoked grants after patch.
- Verify SECURITY DEFINER proconfig/search_path state after patch.
- Anon negative tests for restricted functions.
- Authenticated positive tests for intended app functions.
- Authenticated negative tests for non-owner/non-authorized paths.
- Host/staff/admin/ops smoke tests where relevant.
- Ticket/reservation/event publish/check-in smoke tests if affected.
- Rollback verification.

## 17. Rollback Plan Requirements

- Every future grant revoke must have reversible grant restoration notes.
- Every future proconfig/search_path change must have rollback notes.
- Rollback must not expose secrets or private data.
- Rollback SQL must be reviewed before implementation.
- No destructive function drop in first patch.
- Failed verification blocks release-readiness claims.

## 18. Safety Gates

- [ ] Owner approval obtained.
- [ ] Exact function inventory reviewed.
- [ ] Supabase-managed functions excluded or separately approved.
- [ ] App/dashboard dependency mapping reviewed.
- [ ] Rollback notes prepared.
- [ ] Migration source-of-truth status accepted for this patch.
- [ ] Diff reviewed.
- [ ] Verification plan accepted.
- [ ] Implementation prompt approved.

## 19. Risk Classification

| Risk | Meaning | Handling |
|---|---|---|
| P0 | Security-impacting or authority-boundary item that blocks safe implementation sequencing or launch-ready claims. | Resolve, patch, or explicitly defer with owner acceptance before release-readiness claim. |
| P1 | Important hardening item that should be addressed before public launch or broad beta. | Resolve, patch, or explicitly defer with documented owner acceptance. |
| P2 | Follow-up hardening, documentation, monitoring, or lower-risk verification item. | Track in follow-up artifacts and avoid bundling with P0/P1 patches unless necessary. |
| Unknown | Evidence is insufficient to classify risk. | Classify before implementation; Unknown security-impacting functions block launch-ready claims. |

Unknown security-impacting functions block launch-ready claims.

P0/P1 must be resolved, deferred with explicit owner acceptance, or patched before release-readiness claim.

## 20. Patch Output Expectations

A future approved implementation should produce:

- One migration or controlled SQL patch if approved.
- No app source changes unless dependency review proves necessary.
- Rollback notes.
- Verification report.
- Updated decision/status artifact if residual risks remain.
- Clean git diff review before commit.

## 21. Explicitly Blocked Actions

- Do not write SQL in this plan.
- Do not create migration in this plan.
- Do not run Supabase CLI.
- Do not connect to production.
- Do not invoke RPCs/functions.
- Do not inspect private rows.
- Do not inspect storage objects.
- Do not change app/dashboard/mobile/web/backend source.
- Do not change Edge Functions.
- Do not change storage policies.
- Do not change RLS policies.
- Do not stage or commit.

## 22. Explicitly Blocked Claims

- Do not claim exploitability.
- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim legal compliance.
- Do not claim security hardened.
- Do not claim function grants fixed.
- Do not claim implementation authorized.
- Do not claim all RPC/function risk is resolved.

## 23. Implementation Authorization Status

Implementation remains not authorized.

No SQL, migration, source change, grant change, function change, proconfig change, production mutation, Supabase CLI action, dashboard action, verification query, RPC invocation, or deployment action is authorized by this plan.

A separate owner-approved implementation prompt is required after this plan is reviewed.

## 24. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No database role was created.
- No production connection was made.
- No production mutation was executed.
- Supabase CLI was not run.
- No dashboard action was performed.
- No migration was executed or rolled back.
- No schema dump or production diff query was performed.
- No builds/tests/installs were run.
- No Edge Function was deployed, updated, deleted, invoked, or inspected.
- No secret or environment variable value was inspected, copied, printed, rotated, or changed.
- No storage object was listed, downloaded, uploaded, modified, or deleted.
- No signed URL was generated.
- No credentials, hostnames, full project refs, service_role keys, database passwords, connection strings, webhook secrets, API keys, environment variable values, or secrets were included.
- No private rows, storage objects, object paths, messages, tickets, orders, diagnostics, reports, support notes, auth users, or payment payloads were inspected.
- No application RPC/function was invoked.
- No implementation/admin/support/storage/media/messaging/deletion/export/refund/payment/moderation/RLS/RPC/storage/realtime/Edge/notification/commerce action was executed.
- No files were staged or committed.
- Only 08_PatchPlans/SecurityDefinerAndFunctionGrantHardeningPatchPlan.md was created/modified.

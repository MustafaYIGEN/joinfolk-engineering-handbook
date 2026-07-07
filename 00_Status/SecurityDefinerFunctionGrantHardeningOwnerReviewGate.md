# Security Definer Function Grant Hardening Owner Review Gate

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: PP-01 metadata evidence, committed decision records, and patch plan
- canonical: false
- Gate status: Approved for next preparation step only
- Implementation status: Not authorized
- Production mutation status: Not executed
- Legal status: Engineering owner-review gate only; not legal advice

## 2. Purpose

This file records the owner-review gate for the SecurityDefinerAndFunctionGrantHardeningPatchPlan.

The goal is to decide whether the patch plan is acceptable for the next non-production preparation step: sanitized function inventory and dependency classification.

- This is not implementation.
- This does not authorize SQL.
- This does not authorize migration creation.
- This does not authorize production access.
- This does not authorize Supabase CLI.
- This does not authorize source changes.

## 3. Evidence Boundary

This file is based only on sanitized PP-01 metadata evidence, committed decision records, and the committed patch plan.

No new production access, SQL, CLI, dashboard action, source inspection, private data inspection, storage object listing, build, test, dependency install, migration, verification query, or implementation was performed.

No secrets or private data are included.

## 4. Reviewed Inputs

- `08_PatchPlans/SecurityDefinerAndFunctionGrantHardeningPatchPlan.md`
- `09_Decisions/SecurityDefinerAndFunctionGrantHardeningDecision.md`
- `09_Decisions/RLSPolicyAndGrantMatrixClassification.md`
- `09_Decisions/SupabaseMigrationSourceOfTruthDecision.md`
- `09_Decisions/RLSDisabledRelationTriageDecision.md`
- `09_Decisions/StorageBucketExposureDecision.md`
- `09_Decisions/EdgeFunctionDeploymentInventoryDecision.md`

## 5. Owner Review Question

Should the SecurityDefinerAndFunctionGrantHardeningPatchPlan be accepted as the controlling plan for the next preparation gate: sanitized function inventory and dependency classification?

Accepting this gate does not authorize implementation.

## 6. Gate Decision

Gate decision: Approved for next preparation step only.

- The patch plan may be used to prepare the sanitized function inventory / dependency classification.
- The patch plan may not be used to generate SQL yet.
- The patch plan may not be used to create migrations yet.
- The patch plan may not be used to connect to production.
- The patch plan may not be used to run verification queries.
- The patch plan may not be used to mutate app/source/database/storage/Edge/runtime behavior.

## 7. Approved Next Step

Approved next step: Create a documentation-only sanitized function inventory template / classification artifact.

Suggested next artifact:

`07_Audits/SecurityDefinerFunctionGrantInventoryClassification.md`

- This next artifact must not query production.
- This next artifact must not include secrets or private data.
- This next artifact must be based on existing sanitized evidence and existing handbook/audit material unless owner separately authorizes new metadata collection.
- The artifact should classify candidate functions, dependencies, risk, and missing evidence.

## 8. Not Approved

- SQL implementation.
- Migration creation.
- Production access.
- Supabase CLI.
- Dashboard action.
- Function invocation.
- RPC invocation.
- Private row inspection.
- Storage object listing.
- App/dashboard/mobile/web/backend source modification.
- Function body changes.
- Grant changes.
- proconfig/search_path changes.
- RLS/storage/Edge changes.
- Launch-ready claim.

## 9. Required Preconditions Before Implementation Prompt

- Exact sanitized function inventory.
- Function owner/source classification.
- SECURITY DEFINER vs SECURITY INVOKER classification.
- proconfig/search_path classification.
- Current EXECUTE exposure classification.
- App/dashboard/mobile/web dependency classification.
- Edge/storage/RLS dependency classification.
- Target caller role classification.
- Proposed grant/proconfig action classification.
- Rollback notes.
- Verification plan.
- Owner approval for implementation prompt.

## 10. Candidate Function List for Next Classification

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

They are not approved for mutation.

They require dependency and authority classification first.

## 11. Risk Position

Function grant/proconfig posture remains a P0/P1 security hardening candidate.

Unknown security-impacting function exposure blocks launch-ready claims.

P0/P1 must be resolved, explicitly accepted, or deferred with owner approval before release-readiness claim.

No production safe/unsafe final claim is made.

## 12. Owner Acceptance Scope

Owner acceptance applies only to:

- Using the patch plan as the controlling preparation plan.
- Creating the next documentation-only inventory/classification artifact.
- Keeping implementation blocked until a later explicit implementation authorization.

Owner acceptance does not apply to:

- SQL.
- Migration.
- Production access.
- Production mutation.
- Source changes.
- Verification queries.
- Runtime behavior changes.

## 13. Next Artifact Requirements

For `07_Audits/SecurityDefinerFunctionGrantInventoryClassification.md`, require sections:

- metadata
- purpose
- evidence boundary
- inventory status
- classification model
- candidate function list
- function-by-function classification table
- missing evidence
- risk class
- dependency class
- proposed future action
- rollback requirement
- verification requirement
- implementation authorization status
- no-modification confirmation

## 14. Explicitly Blocked Claims

- Do not claim exploitability.
- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim legal compliance.
- Do not claim security hardened.
- Do not claim function grants fixed.
- Do not claim implementation authorized.
- Do not claim all RPC/function risk is resolved.

## 15. Implementation Authorization Status

Implementation remains not authorized.

No SQL, migration, source change, grant change, function change, proconfig change, production mutation, Supabase CLI action, dashboard action, verification query, RPC invocation, or deployment action is authorized by this owner-review gate.

A separate owner-approved implementation prompt is required after inventory/classification and rollback plan are reviewed.

## 16. No-Modification Confirmation

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
- Only `00_Status/SecurityDefinerFunctionGrantHardeningOwnerReviewGate.md` was created/modified.

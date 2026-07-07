# Security Definer Function Grant Metadata Collection Plan

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: committed decision records, patch plan, owner-review gate, inventory classification, and completeness review
- canonical: false
- Plan status: Proposed
- Metadata collection execution status: Not authorized
- Implementation status: Not authorized
- Production mutation status: Not executed
- Legal status: Engineering metadata planning only; not legal advice

## 2. Purpose

This plan defines the sanitized metadata required before any future SecurityDefiner/function grant implementation prompt.

- This is not metadata collection execution.
- This is not implementation.
- This does not authorize executable SQL.
- This does not authorize migration creation.
- This does not authorize production access.
- This does not authorize Supabase CLI.
- This does not authorize source changes.
- This does not authorize verification queries.
- This does not authorize RPC/function invocation.

## 3. Evidence Boundary

This plan is based only on committed handbook artifacts and sanitized PP-01 evidence already recorded.

No new production access, SQL, CLI, dashboard action, source inspection, private data inspection, storage object listing, build, test, dependency install, migration, verification query, RPC/function invocation, metadata collection execution, or implementation was performed.

No secrets or private data are included.

## 4. Reviewed Inputs

- `00_Status/SecurityDefinerFunctionGrantClassificationCompletenessReview.md`
- `07_Audits/SecurityDefinerFunctionGrantInventoryClassification.md`
- `00_Status/SecurityDefinerFunctionGrantHardeningOwnerReviewGate.md`
- `08_PatchPlans/SecurityDefinerAndFunctionGrantHardeningPatchPlan.md`
- `09_Decisions/SecurityDefinerAndFunctionGrantHardeningDecision.md`
- `09_Decisions/RLSPolicyAndGrantMatrixClassification.md`
- `09_Decisions/SupabaseMigrationSourceOfTruthDecision.md`
- `09_Decisions/RLSDisabledRelationTriageDecision.md`
- `09_Decisions/StorageBucketExposureDecision.md`
- `09_Decisions/EdgeFunctionDeploymentInventoryDecision.md`
- `07_Audits/AuditIndex.md`

## 5. Planning Question

What sanitized function metadata must be collected before any future implementation prompt can be considered?

Answer:

A function-by-function metadata inventory is required for ownership, schema, security mode, proconfig/search_path state, EXECUTE exposure, dependency, authority gate, rollback, and verification classification.

This plan answers only what must be collected; it does not authorize collection execution.

## 6. Candidate Function Scope

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

They are not approved for invocation.

They require exact metadata before implementation.

## 7. Required Sanitized Metadata Fields

| Metadata field | Purpose | Sensitive data boundary | Required before implementation? |
|---|---|---|---|
| function schema | Identify namespace and platform/app ownership context. | Metadata only; no private rows, no storage objects, no secrets, no payloads. | Yes |
| function name | Identify the function being classified. | Metadata only; no private rows, no storage objects, no secrets, no payloads. | Yes |
| argument signature | Distinguish overloads and exact candidates. | Metadata only; no private rows, no storage objects, no secrets, no payloads. | Yes |
| function owner/source classification | Determine app-owned, platform-managed, legacy, or unknown ownership. | Metadata only; no private rows, no storage objects, no secrets, no payloads. | Yes |
| app-owned vs platform-managed classification | Determine whether app hardening scope applies. | Metadata only; no private rows, no storage objects, no secrets, no payloads. | Yes |
| SECURITY DEFINER vs SECURITY INVOKER state | Classify privilege execution posture. | Metadata only; no private rows, no storage objects, no secrets, no payloads. | Yes |
| proconfig/search_path state | Determine whether explicit hardening posture exists. | Metadata only; no private rows, no storage objects, no secrets, no payloads. | Yes |
| anon EXECUTE exposure | Identify unauthenticated caller exposure. | Metadata only; no private rows, no storage objects, no secrets, no payloads. | Yes |
| authenticated EXECUTE exposure | Identify signed-in user caller exposure. | Metadata only; no private rows, no storage objects, no secrets, no payloads. | Yes |
| service_role/internal EXECUTE exposure | Identify internal/privileged path dependency. | Metadata only; no private rows, no storage objects, no secrets, no payloads. | Yes |
| PUBLIC/inherited EXECUTE exposure | Identify broad inherited exposure. | Metadata only; no private rows, no storage objects, no secrets, no payloads. | Yes |
| function body/gate classification | Classify authority gate behavior where separately approved and redacted. | Metadata/classification only; no private rows, no storage objects, no secrets, no payloads. | Yes, or explicit owner-approved deferral |
| table/RLS dependency classification | Identify table/RLS dependencies that affect access assumptions. | Metadata only; no private rows, no storage objects, no secrets, no payloads. | Yes |
| storage dependency classification | Identify storage or signed URL dependency. | Metadata only; no private rows, no storage objects, no secrets, no payloads. | Yes |
| Edge Function dependency classification | Identify deployed serverless dependency. | Metadata only; no private rows, no storage objects, no secrets, no payloads. | Yes |
| app/dashboard/mobile/web call-site dependency | Prevent breaking active product flows. | Sanitized references only; no private rows, no storage objects, no secrets, no payloads. | Yes |
| intended caller role | Define target caller after hardening. | Metadata/classification only; no private rows, no storage objects, no secrets, no payloads. | Yes |
| proposed future action | Preserve, restrict, harden, defer, exclude, or accept. | Classification only; no private rows, no storage objects, no secrets, no payloads. | Yes |
| rollback requirement | Define reversibility needs before implementation. | Planning only; no executable SQL in this artifact. | Yes |
| verification requirement | Define verification needs before implementation. | Planning only; no verification execution in this artifact. | Yes |
| smoke test requirement | Define manual/automated smoke scope after approval. | Planning only; no private rows, no storage objects, no secrets, no payloads. | Yes |
| risk class | Classify P0/P1/P2/Unknown. | Classification only; no private rows, no storage objects, no secrets, no payloads. | Yes |
| owner approval requirement | Identify required owner approval before action. | Planning only; no private rows, no storage objects, no secrets, no payloads. | Yes |

## 8. Allowed Future Evidence Categories

Allowed future evidence categories, if separately owner-approved later:

- Sanitized function metadata.
- Sanitized grant metadata.
- Sanitized role exposure metadata.
- Sanitized proconfig/search_path metadata.
- Sanitized function source classification, only if body/gate review is separately approved and redacted.
- Sanitized app/dashboard call-site references.
- Sanitized Edge dependency references.
- Sanitized RLS/storage dependency references.
- Sanitized migration provenance references.

These categories are not authorized to be collected by this plan.

## 9. Explicitly Blocked Evidence Categories

- Private table rows.
- Auth user rows.
- Storage object names.
- Storage object paths.
- Storage object downloads.
- Signed URL generation.
- Message bodies.
- Tickets/orders/reservations/payment payloads.
- Notification payloads.
- Diagnostics payloads.
- Support notes.
- Secrets.
- Environment variable values.
- Hostnames.
- Full project refs.
- Database passwords.
- service_role keys.
- anon keys.
- JWT secrets.
- Webhook secrets.
- API keys.
- Production data exports.
- Function/RPC invocation results.
- Mutation results.

## 10. Future Collection Approval Gate

Before any metadata collection execution, a separate owner-approved status/gate artifact is required.

Suggested next gate:

`00_Status/SecurityDefinerFunctionGrantMetadataCollectionApprovalGate.md`

That future gate must define:

- Operator.
- Access model.
- Allowed metadata categories.
- Exact blocked categories.
- Whether read-only production metadata access is approved.
- Whether any temporary verifier role is approved.
- Whether any CLI/dashboard access is approved.
- Exact output redaction rules.
- Exact no-private-data rules.
- Revocation/cleanup requirements.
- Whether execution is approved or still blocked.

This current plan does not approve the gate outcome.

## 11. Future Collection Output Format

Sanitized output table template:

| Function | Schema | Owner/source | Security mode | proconfig/search_path | anon execute | authenticated execute | service/internal execute | PUBLIC/inherited execute | Body/gate class | Dependencies | Risk | Proposed future action | Rollback note needed | Verification needed | Implementation authorized? |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | No |

Rows are template / TBD only.

Exact metadata values are not filled here.

No values are invented.

No execution happened.

## 12. Classification Rules After Collection

- App-owned SECURITY DEFINER functions with missing proconfig/search_path require hardening review.
- Functions with anon exposure require intended-public classification or restriction review.
- Functions with authenticated broad exposure require caller-role classification.
- PUBLIC/inherited execute exposure requires explicit-grant review.
- Lifecycle mutation functions require host/staff/admin authority review.
- Reminder/privacy functions require user-owned data and privacy review.
- Platform-managed functions must be excluded unless separately approved.
- Unknown ownership/dependency blocks implementation.
- Unknown security-impacting exposure blocks launch-ready claims.

## 13. Future Patch Eligibility Criteria

A future implementation prompt remains blocked until all are true:

- Exact sanitized metadata collected and reviewed.
- Owner/source classified.
- Security mode classified.
- proconfig/search_path classified.
- Execute exposure classified per role.
- Dependencies classified.
- Body/gate behavior classified or explicitly deferred.
- Target caller role classified.
- Future action classified.
- Rollback notes drafted.
- Verification plan drafted.
- Owner approves implementation prompt.

## 14. Rollback Planning Requirement

Every future grant revoke must have reversible grant restoration notes.

Every future proconfig/search_path change must have rollback notes.

No destructive function drop is allowed in first patch.

Rollback notes must be reviewed before implementation prompt.

Rollback must not expose secrets or private data.

## 15. Verification Planning Requirement

- Metadata-only verification where possible.
- Verify intended grants after patch.
- Verify revoked grants after patch.
- Verify SECURITY DEFINER proconfig/search_path after patch.
- Anon negative tests.
- Authenticated positive tests.
- Authenticated non-owner/non-authorized negative tests.
- Host/staff/admin smoke tests where relevant.
- Event publish/control/check-in smoke tests if affected.
- Reminder list/upsert/delete smoke tests if affected.
- Rollback verification.

Verification planning is allowed in this document.

Verification execution is not authorized by this document.

## 16. Not Approved

- Executable SQL.
- SQL execution.
- Metadata collection execution.
- Production access.
- Supabase CLI.
- Dashboard action.
- Verification query.
- RPC/function invocation.
- Migration creation.
- Grant changes.
- proconfig/search_path changes.
- Function body changes.
- RLS changes.
- Storage policy changes.
- Edge Function changes.
- App/dashboard/mobile/web/backend source changes.
- Launch-ready claim.

## 17. Risk Position

Function grant/proconfig posture remains a P0/P1 security hardening candidate.

Unknown security-impacting function exposure remains a launch-readiness blocker.

No production safe/unsafe final claim is made.

No exploitability claim is made.

No implementation authorization is made.

No metadata collection execution authorization is made.

## 18. Implementation Authorization Status

Implementation remains not authorized.

Metadata collection execution remains not authorized.

No executable SQL, SQL execution, migration, source change, grant change, function change, proconfig change, production access, production mutation, Supabase CLI action, dashboard action, verification query, RPC invocation, metadata collection execution, or deployment action is authorized by this plan.

A separate owner-approved metadata collection approval gate is required before metadata collection execution.

A separate owner-approved implementation prompt is required after exact metadata inventory, rollback plan, and verification plan are reviewed.

## 19. Explicitly Blocked Claims

- Do not claim exploitability.
- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim legal compliance.
- Do not claim security hardened.
- Do not claim function grants fixed.
- Do not claim implementation authorized.
- Do not claim metadata collection authorized.
- Do not claim all RPC/function risk is resolved.

## 20. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No executable SQL was written.
- No SQL or migration was created.
- No database role was created.
- No production connection was made.
- No production mutation was executed.
- Supabase CLI was not run.
- No dashboard action was performed.
- No verification query was executed.
- No metadata collection was executed.
- No RPC/function was invoked.
- No migration was executed or rolled back.
- No schema dump or production diff query was performed.
- No builds/tests/installs were run.
- No Edge Function was deployed, updated, deleted, invoked, or inspected.
- No secret or environment variable value was inspected, copied, printed, rotated, or changed.
- No storage object was listed, downloaded, uploaded, modified, or deleted.
- No signed URL was generated.
- No credentials, hostnames, full project refs, service_role keys, database passwords, connection strings, webhook secrets, API keys, environment variable values, or secrets were included.
- No private rows, storage objects, object paths, messages, tickets, orders, diagnostics, reports, support notes, auth users, or payment payloads were inspected.
- No implementation/admin/support/storage/media/messaging/deletion/export/refund/payment/moderation/RLS/RPC/storage/realtime/Edge/notification/commerce action was executed.
- No files were staged or committed.
- Only `08_PatchPlans/SecurityDefinerFunctionGrantMetadataCollectionPlan.md` was created/modified.

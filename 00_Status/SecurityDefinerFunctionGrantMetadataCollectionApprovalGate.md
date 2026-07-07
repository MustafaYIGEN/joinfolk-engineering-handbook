# Security Definer Function Grant Metadata Collection Approval Gate

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: committed decision records, patch plans, owner-review gate, inventory classification, completeness review, and metadata collection plan
- canonical: false
- Gate status: Approved for bounded read-only sanitized metadata collection preparation/execution under this gate only
- Metadata collection execution status: Conditionally approved under strict boundaries
- Implementation status: Not authorized
- Production mutation status: Not executed
- Legal status: Engineering metadata approval gate only; not legal advice

## 2. Purpose

This file records the owner approval gate for bounded read-only sanitized metadata collection needed before any future SecurityDefiner/function grant implementation prompt.

- This gate approves only bounded metadata collection under the stated constraints.
- This gate does not authorize implementation.
- This gate does not authorize executable SQL inside this document.
- This gate does not authorize migration creation.
- This gate does not authorize production mutation.
- This gate does not authorize Supabase CLI unless separately approved.
- This gate does not authorize dashboard action.
- This gate does not authorize RPC/function invocation.
- This gate does not authorize private row inspection.
- This gate does not authorize storage object listing.

## 3. Reviewed Inputs

- `08_PatchPlans/SecurityDefinerFunctionGrantMetadataCollectionPlan.md`
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

## 4. Gate Question

Should bounded read-only sanitized metadata collection be approved so the exact function-by-function inventory can be produced?

Answer:

Yes, conditionally.

The preliminary classification is incomplete for implementation.

Exact metadata is required before implementation can be considered.

Metadata collection is approved only within the strict evidence boundaries below.

Implementation remains blocked.

## 5. Gate Decision

Gate decision: Approved for bounded read-only sanitized metadata collection only.

Approved purpose: collect exact sanitized metadata needed to classify the listed candidate functions.

Approved output: redacted metadata inventory only.

Not approved: implementation, mutation, migration, source change, private data inspection, storage object listing, RPC/function invocation, or launch-readiness claim.

## 6. Approved Operator and Access Model

- Operator: Mustafa / owner, or an explicitly owner-approved operator.
- Access model: read-only metadata access only.
- Temporary verifier role may be used only if separately created with read-only metadata intent and revoked after use.
- service_role usage is not approved by this gate.
- CLI usage is not approved by this gate.
- Dashboard-based production changes are not approved by this gate.
- The operator must not collect or print credentials, hostnames, full project refs, connection strings, keys, secrets, private row values, storage object names, or storage object paths.

## 7. Approved Metadata Categories

Approved only for these categories:

- Function schema.
- Function name.
- Argument signature.
- Function owner/source classification.
- App-owned vs platform-managed classification.
- SECURITY DEFINER vs SECURITY INVOKER state.
- proconfig/search_path state.
- anon EXECUTE exposure.
- authenticated EXECUTE exposure.
- service_role/internal EXECUTE exposure.
- PUBLIC/inherited EXECUTE exposure.
- Sanitized function body/gate classification only if redacted and limited to authority-gate description, not full function body.
- Table/RLS dependency classification by table name/category only; no row values.
- Storage dependency classification by bucket/category only; no object names or paths.
- Edge Function dependency classification by function name/category only; no secrets or environment values.
- App/dashboard/mobile/web call-site references by file/path/function reference only; no secrets.
- Intended caller role.
- Proposed future action.
- Rollback requirement.
- Verification requirement.
- Smoke test requirement.
- Risk class.
- Owner approval requirement.

## 8. Explicitly Blocked Data and Evidence

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
- Full function bodies if they include secrets, private data, payload handling, or sensitive operational details.

## 9. Approved Candidate Function Scope

- `control_cancel_event(event_id uuid)`
- `control_end_event(event_id uuid)`
- `control_open_checkin(event_id uuid)`
- `delete_personal_reminder(p_id uuid)`
- `list_active_reminders()`
- `list_personal_reminders()`
- `publish_event(p_event_id uuid, p_visibility text)`
- `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])`
- `upsert_personal_reminder(p_id uuid, p_title text, p_note text, p_reminder_date date, p_color_theme text, p_remind_before_days integer)`

Scope is limited to metadata classification for these candidates and directly required dependency metadata.

They are not proven exploitable.

They are not approved for mutation.

They are not approved for invocation.

## 10. Approved Output Artifact

Approved future output artifact:

`07_Audits/SecurityDefinerFunctionGrantCollectedMetadataReport.md`

The report must contain only sanitized metadata and must include:

- metadata.
- evidence boundary.
- operator/access model.
- candidate function inventory table.
- grant exposure table.
- proconfig/search_path table.
- dependency classification.
- missing evidence.
- redaction confirmation.
- no-private-data confirmation.
- implementation authorization status.
- no-modification confirmation.

## 11. Required Output Table

Future sanitized output table columns:

| Function | Schema | Owner/source | Security mode | proconfig/search_path | anon execute | authenticated execute | service/internal execute | PUBLIC/inherited execute | Body/gate class | Dependencies | Risk | Proposed future action | Rollback note needed | Verification needed | Implementation authorized? |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | No |

The report may fill these values only after approved metadata collection is performed.

Values must be sanitized.

Secrets/private data must not be included.

Implementation authorized must remain No unless a later separate implementation gate changes it.

## 12. Execution Constraints

- Metadata collection must be read-only.
- Metadata collection must not mutate production.
- Metadata collection must not invoke app RPCs/functions.
- Metadata collection must not inspect private rows.
- Metadata collection must not list storage objects.
- Metadata collection must not generate signed URLs.
- Metadata collection must not export production data.
- Metadata collection must not use service_role unless a separate explicit approval exists.
- Metadata collection must stop immediately if private data, secrets, object paths, or payloads appear in output.
- Output must be redacted before being committed.

## 13. Revocation and Cleanup Requirements

- Any temporary verifier role must be revoked after use.
- Any temporary access must be disabled after use.
- Revocation/cleanup evidence must be recorded without secrets.
- No long-lived elevated credential may be created by this gate.
- No credential value may be stored in the handbook.

## 14. Not Approved

- Implementation.
- Executable SQL committed into this file.
- Production mutation.
- Migration creation.
- Migration execution.
- Source changes.
- Grant changes.
- proconfig/search_path changes.
- Function body changes.
- RLS changes.
- Storage policy changes.
- Edge Function changes.
- Dashboard action.
- Supabase CLI.
- RPC/function invocation.
- Private row inspection.
- Storage object listing.
- Signed URL generation.
- Production data export.
- Launch-ready claim.
- Production safe claim.
- Security hardened claim.

## 15. Risk Position

Function grant/proconfig posture remains a P0/P1 security hardening candidate.

Unknown security-impacting function exposure remains a launch-readiness blocker.

Metadata collection may reduce unknowns but does not itself fix risk.

No production safe/unsafe final claim is made.

No exploitability claim is made.

No implementation authorization is made.

## 16. Implementation Authorization Status

Implementation remains not authorized.

Metadata collection is conditionally approved only under this gate's boundaries.

No migration, source change, grant change, function change, proconfig change, production mutation, Supabase CLI action, dashboard action, RPC invocation, private row inspection, storage object listing, signed URL generation, or deployment action is authorized by this gate.

A separate owner-approved implementation prompt is required after the collected metadata report, rollback plan, and verification plan are reviewed.

## 17. Explicitly Blocked Claims

- Do not claim exploitability.
- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim legal compliance.
- Do not claim security hardened.
- Do not claim function grants fixed.
- Do not claim implementation authorized.
- Do not claim all RPC/function risk is resolved.

## 18. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No executable SQL was written in this file.
- No SQL or migration was created.
- No database role was created by this file.
- No production connection was made during creation of this file.
- No production mutation was executed.
- Supabase CLI was not run.
- No dashboard action was performed.
- No verification query was executed during creation of this file.
- No metadata collection was executed during creation of this file.
- No RPC/function was invoked.
- No migration was executed or rolled back.
- No schema dump or production diff query was performed during creation of this file.
- No builds/tests/installs were run.
- No Edge Function was deployed, updated, deleted, invoked, or inspected.
- No secret or environment variable value was inspected, copied, printed, rotated, or changed.
- No storage object was listed, downloaded, uploaded, modified, or deleted.
- No signed URL was generated.
- No credentials, hostnames, full project refs, service_role keys, database passwords, connection strings, webhook secrets, API keys, environment variable values, or secrets were included.
- No private rows, storage objects, object paths, messages, tickets, orders, diagnostics, reports, support notes, auth users, or payment payloads were inspected.
- No implementation/admin/support/storage/media/messaging/deletion/export/refund/payment/moderation/RLS/RPC/storage/realtime/Edge/notification/commerce action was executed.
- No files were staged or committed.
- Only `00_Status/SecurityDefinerFunctionGrantMetadataCollectionApprovalGate.md` was created/modified.

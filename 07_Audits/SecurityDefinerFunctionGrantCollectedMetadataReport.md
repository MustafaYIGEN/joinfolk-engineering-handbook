# Security Definer Function Grant Collected Metadata Report

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: committed metadata collection approval gate and prior handbook artifacts
- canonical: false
- Report status: Shell created; metadata collection not executed in this task
- Metadata collection execution status: Not executed in this task
- Implementation status: Not authorized
- Production mutation status: Not executed
- Legal status: Engineering metadata report shell only; not legal advice

## 2. Purpose

This report is the controlled destination artifact for bounded sanitized metadata collected under the approval gate.

- This file is currently a report shell.
- Exact metadata values are TBD until approved metadata collection is executed separately.
- This file does not execute metadata collection.
- This file does not authorize implementation.
- This file does not authorize SQL execution.
- This file does not authorize executable SQL.
- This file does not authorize migration creation.
- This file does not authorize production mutation.
- This file does not authorize Supabase CLI.
- This file does not authorize RPC/function invocation.
- This file does not authorize private row inspection.
- This file does not authorize storage object listing.

## 3. Evidence Boundary

This shell is based only on committed handbook artifacts.

No metadata collection was executed during creation of this file.

No production access, SQL execution, CLI action, dashboard action, verification query, RPC/function invocation, migration, source inspection, private row inspection, storage object listing, storage object download, signed URL generation, build, test, install, or implementation was performed during creation of this file.

No secrets or private data are included.

## 4. Reviewed Inputs

- `00_Status/SecurityDefinerFunctionGrantMetadataCollectionApprovalGate.md`
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

## 5. Operator / Access Model

- Operator: TBD
- Access model used: Not executed in this task
- Temporary verifier role used: No
- service_role used: No
- Supabase CLI used: No
- Dashboard action used: No
- Production connection made during this task: No
- Revocation/cleanup required from this task: No temporary access created

Future filled reports must record the approved operator/access model without secrets.

## 6. Candidate Function Inventory Table

| Function | Schema | Owner/source | Security mode | proconfig/search_path | anon execute | authenticated execute | service/internal execute | PUBLIC/inherited execute | Body/gate class | Dependencies | Risk | Proposed future action | Rollback note needed | Verification needed | Implementation authorized? |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| `control_cancel_event(event_id uuid)` | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | No |
| `control_end_event(event_id uuid)` | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | No |
| `control_open_checkin(event_id uuid)` | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | No |
| `delete_personal_reminder(p_id uuid)` | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | No |
| `list_active_reminders()` | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | No |
| `list_personal_reminders()` | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | No |
| `publish_event(p_event_id uuid, p_visibility text)` | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | No |
| `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])` | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | No |
| `upsert_personal_reminder(p_id uuid, p_title text, p_note text, p_reminder_date date, p_color_theme text, p_remind_before_days integer)` | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | No |

## 7. Grant Exposure Table

| Function | anon execute | authenticated execute | service/internal execute | PUBLIC/inherited execute | Exposure status | Missing evidence |
|---|---|---|---|---|---|
| `control_cancel_event(event_id uuid)` | TBD | TBD | TBD | TBD | Missing | Metadata collection not executed. |
| `control_end_event(event_id uuid)` | TBD | TBD | TBD | TBD | Missing | Metadata collection not executed. |
| `control_open_checkin(event_id uuid)` | TBD | TBD | TBD | TBD | Missing | Metadata collection not executed. |
| `delete_personal_reminder(p_id uuid)` | TBD | TBD | TBD | TBD | Missing | Metadata collection not executed. |
| `list_active_reminders()` | TBD | TBD | TBD | TBD | Missing | Metadata collection not executed. |
| `list_personal_reminders()` | TBD | TBD | TBD | TBD | Missing | Metadata collection not executed. |
| `publish_event(p_event_id uuid, p_visibility text)` | TBD | TBD | TBD | TBD | Missing | Metadata collection not executed. |
| `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])` | TBD | TBD | TBD | TBD | Missing | Metadata collection not executed. |
| `upsert_personal_reminder(p_id uuid, p_title text, p_note text, p_reminder_date date, p_color_theme text, p_remind_before_days integer)` | TBD | TBD | TBD | TBD | Missing | Metadata collection not executed. |

## 8. proconfig / search_path Table

| Function | SECURITY DEFINER candidate? | proconfig/search_path state | Missing evidence | Future hardening review required? |
|---|---|---|---|---|
| `control_cancel_event(event_id uuid)` | Candidate from PP-01 evidence | TBD | exact current state | Yes, pending metadata |
| `control_end_event(event_id uuid)` | Candidate from PP-01 evidence | TBD | exact current state | Yes, pending metadata |
| `control_open_checkin(event_id uuid)` | Candidate from PP-01 evidence | TBD | exact current state | Yes, pending metadata |
| `delete_personal_reminder(p_id uuid)` | Candidate from PP-01 evidence | TBD | exact current state | Yes, pending metadata |
| `list_active_reminders()` | Candidate from PP-01 evidence | TBD | exact current state | Yes, pending metadata |
| `list_personal_reminders()` | Candidate from PP-01 evidence | TBD | exact current state | Yes, pending metadata |
| `publish_event(p_event_id uuid, p_visibility text)` | Candidate from PP-01 evidence | TBD | exact current state | Yes, pending metadata |
| `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])` | Candidate from PP-01 evidence | TBD | exact current state | Yes, pending metadata |
| `upsert_personal_reminder(p_id uuid, p_title text, p_note text, p_reminder_date date, p_color_theme text, p_remind_before_days integer)` | Candidate from PP-01 evidence | TBD | exact current state | Yes, pending metadata |

## 9. Dependency Classification Table

| Function | Product domain | App/dashboard/mobile/web dependency | Edge dependency | Storage/RLS dependency | Smoke test scope | Dependency status |
|---|---|---|---|---|---|---|
| `control_cancel_event(event_id uuid)` | Event lifecycle / host authority | TBD / Missing | TBD / Missing | TBD / Missing | TBD / Missing | Missing until metadata collection and call-site review are performed. |
| `control_end_event(event_id uuid)` | Event lifecycle / host authority | TBD / Missing | TBD / Missing | TBD / Missing | TBD / Missing | Missing until metadata collection and call-site review are performed. |
| `control_open_checkin(event_id uuid)` | Event lifecycle / host authority | TBD / Missing | TBD / Missing | TBD / Missing | TBD / Missing | Missing until metadata collection and call-site review are performed. |
| `delete_personal_reminder(p_id uuid)` | Notification/reminder/privacy | TBD / Missing | TBD / Missing | TBD / Missing | TBD / Missing | Missing until metadata collection and call-site review are performed. |
| `list_active_reminders()` | Notification/reminder/privacy | TBD / Missing | TBD / Missing | TBD / Missing | TBD / Missing | Missing until metadata collection and call-site review are performed. |
| `list_personal_reminders()` | Notification/reminder/privacy | TBD / Missing | TBD / Missing | TBD / Missing | TBD / Missing | Missing until metadata collection and call-site review are performed. |
| `publish_event(p_event_id uuid, p_visibility text)` | Event lifecycle / visibility / host authority | TBD / Missing | TBD / Missing | TBD / Missing | TBD / Missing | Missing until metadata collection and call-site review are performed. |
| `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])` | Event lifecycle / visibility / host authority | TBD / Missing | TBD / Missing | TBD / Missing | TBD / Missing | Missing until metadata collection and call-site review are performed. |
| `upsert_personal_reminder(p_id uuid, p_title text, p_note text, p_reminder_date date, p_color_theme text, p_remind_before_days integer)` | Notification/reminder/privacy | TBD / Missing | TBD / Missing | TBD / Missing | TBD / Missing | Missing until metadata collection and call-site review are performed. |

## 10. Missing Evidence

- Exact function schema.
- Exact function owner/source.
- Exact app-owned vs platform-managed classification.
- Exact SECURITY DEFINER vs SECURITY INVOKER state.
- Exact proconfig/search_path state.
- Exact anon EXECUTE exposure per function.
- Exact authenticated EXECUTE exposure per function.
- Exact service_role/internal exposure per function.
- Exact PUBLIC/inherited exposure per function.
- Sanitized body/gate classification.
- App/dashboard/mobile/web call-site references.
- Edge Function dependency classification.
- Table/RLS dependency classification.
- Storage dependency classification.
- Intended caller role.
- Proposed future action.
- Rollback requirement.
- Verification requirement.
- Smoke test requirement.
- Risk class finalization.

## 11. Redaction Confirmation

- This shell contains no secrets.
- This shell contains no credentials.
- This shell contains no hostnames.
- This shell contains no full project refs.
- This shell contains no service_role keys.
- This shell contains no anon keys.
- This shell contains no database passwords.
- This shell contains no connection strings.
- This shell contains no environment variable values.
- This shell contains no private row values.
- This shell contains no storage object names.
- This shell contains no storage object paths.
- This shell contains no message/ticket/order/reservation/payment/notification/diagnostic/support payloads.

## 12. No-Private-Data Confirmation

- No private rows were inspected during creation of this shell.
- No auth user rows were inspected during creation of this shell.
- No storage objects were listed during creation of this shell.
- No storage objects were downloaded during creation of this shell.
- No signed URLs were generated during creation of this shell.
- No app RPC/function was invoked during creation of this shell.
- No mutation result was collected during creation of this shell.

## 13. Collection Execution Status

Metadata collection was not executed during creation of this file.

The approval gate allows only bounded sanitized metadata collection under strict constraints.

This shell must be updated only with sanitized metadata after approved collection is actually performed.

If private data, secrets, object paths, payloads, or credentials appear in output, collection must stop and output must not be committed.

## 14. Risk Position

Function grant/proconfig posture remains a P0/P1 security hardening candidate.

Unknown security-impacting function exposure remains a launch-readiness blocker.

This shell does not reduce risk by itself.

No production safe/unsafe final claim is made.

No exploitability claim is made.

No implementation authorization is made.

## 15. Implementation Authorization Status

Implementation remains not authorized.

Metadata collection was not executed in this task.

No SQL execution, executable SQL, migration, source change, grant change, function change, proconfig change, production mutation, Supabase CLI action, dashboard action, RPC invocation, private row inspection, storage object listing, signed URL generation, or deployment action is authorized by this report shell.

A separate owner-approved implementation prompt is required after the collected metadata, rollback plan, and verification plan are reviewed.

## 16. Explicitly Blocked Claims

- Do not claim exploitability.
- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim legal compliance.
- Do not claim security hardened.
- Do not claim function grants fixed.
- Do not claim implementation authorized.
- Do not claim all RPC/function risk is resolved.

## 17. No-Modification Confirmation

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
- Only `07_Audits/SecurityDefinerFunctionGrantCollectedMetadataReport.md` was created/modified.

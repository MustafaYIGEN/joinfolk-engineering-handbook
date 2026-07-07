# Security Definer Function Grant Collected Metadata Report

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: committed metadata collection approval gate and prior handbook artifacts
- canonical: false
- Report status: Local-only evidence added; production metadata collection not executed in this task
- Metadata collection execution status: Local source/migration/call-site review performed; production metadata collection not executed
- Implementation status: Not authorized
- Production mutation status: Not executed
- Legal status: Engineering metadata report shell only; not legal advice

## 2. Purpose

This report is the controlled destination artifact for bounded sanitized metadata collected under the approval gate.

- This file currently contains local-only source, migration, call-site, and handbook evidence where available.
- Exact production metadata values are TBD until approved production metadata collection is executed separately.
- This file does not execute production metadata collection.
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

This report is based only on committed handbook artifacts and local read-only source/migration/call-site evidence from allowed local roots.

No production metadata collection was executed during creation of this file.

No production access, SQL execution, CLI action, dashboard action, verification query, RPC/function invocation, migration, private row inspection, storage object listing, storage object download, signed URL generation, build, test, install, source modification, or implementation was performed during creation of this file.

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

- Operator: Local source review by owner/Codex under handbook constraints
- Access model used: Local filesystem read-only; no production access
- Temporary verifier role used: No
- service_role used: No
- Supabase CLI used: No
- Dashboard action used: No
- Production connection made during this task: No
- Revocation/cleanup required from this task: No temporary access created

- Evidence source: local migrations, local source references, and committed handbook artifacts only.
- Production metadata collection: Not executed.
- Production parity: Not claimed.

Future production-metadata-filled reports must record the approved operator/access model without secrets.

## 6. Candidate Function Inventory Table

| Function | Schema | Owner/source | Security mode | proconfig/search_path | anon execute | authenticated execute | service/internal execute | PUBLIC/inherited execute | Body/gate class | Dependencies | Risk | Proposed future action | Rollback note needed | Verification needed | Implementation authorized? |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| `control_cancel_event(event_id uuid)` | TBD / Requires approved metadata collection | Handbook-only candidate; exact local migration definition not found under this name | TBD / Requires approved metadata collection | Handbook candidate; exact local state TBD | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | Handbook references `EventLifecycleContractAudit.md` and `SupabaseFocusedBackendFollowUpReport.md`; older local cancel/unpublish migration family exists, but exact function name not found locally. | P0/P1 candidate | Defer pending production metadata verification and exact local/prod mapping | Yes | Yes | No |
| `control_end_event(event_id uuid)` | public, local migration evidence only; production not verified | Local migration evidence: `[hostos]/supabase/migrations/20260127_control_actions.sql`; later local grant hardening reference in `20260604_revoke_legacy_lifecycle_and_restore_city_walls.sql` | SECURITY DEFINER in local migration evidence; production not verified | Local evidence indicates missing explicit proconfig/search_path in initial definition; production not verified | Local-only later migration references prior broad exposure removal; production not verified | Local-only later migration references authenticated execute restoration; production not verified | TBD / Requires approved metadata collection | Local-only later migration references inherited/broad exposure removal; production not verified | Local body/gate category: host ownership via current user identity, event lifecycle update; body not pasted. | Local migration evidence; no exact `joinfolk-web` call-site found; handbook lifecycle dependency references. | P0/P1 candidate | Future proconfig hardening review and grant exposure review after production metadata | Yes | Yes | No |
| `control_open_checkin(event_id uuid)` | public, local migration evidence uses `p_event_id`; production not verified | Local migration evidence: `[hostos]/supabase/migrations/20260127_checkin_rpc.sql` | SECURITY DEFINER in local migration evidence; production not verified | Local evidence indicates missing explicit proconfig/search_path; production not verified | Handbook audit reports broad anon exposure; local grant evidence not found; production not verified | Handbook audit reports broad authenticated exposure; local grant evidence not found; production not verified | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | Local body/gate category: host ownership via current user identity, event state/check-in update; body not pasted. | Local migration evidence; handbook check-in broad grant references; no exact `joinfolk-web` call-site found. | P0/P1 candidate | Future proconfig hardening review and grant exposure review after production metadata | Yes | Yes | No |
| `delete_personal_reminder(p_id uuid)` | public, local migration evidence only; production not verified | Local migration evidence: `[hostos]/supabase/migrations/20260314_personal_reminders.sql` | SECURITY DEFINER in local migration evidence; production not verified | Local evidence indicates missing explicit proconfig/search_path; production not verified | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | Local body/gate category: user-owned personal reminder delete via current user identity; body not pasted. | Local migration evidence; `NotificationPushReminderContractAudit.md` references reminder RPC use; no exact `joinfolk-web` call-site found. | P1 / Unknown candidate | Future proconfig hardening review and grant exposure review after production metadata | Yes | Yes | No |
| `list_active_reminders()` | public, local migration evidence only; production not verified | Local migration evidence: `[hostos]/supabase/migrations/20260314_personal_reminders.sql` | SECURITY DEFINER in local migration evidence; production not verified | Local evidence indicates missing explicit proconfig/search_path; production not verified | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | Local body/gate category: user-owned personal reminder read via current user identity; body not pasted. | Local migration evidence; `NotificationPushReminderContractAudit.md` references reminder RPC concerns; no exact `joinfolk-web` call-site found. | P1 / Unknown candidate | Future proconfig hardening review and grant exposure review after production metadata | Yes | Yes | No |
| `list_personal_reminders()` | public, local migration evidence only; production not verified | Local migration evidence: `[hostos]/supabase/migrations/20260314_personal_reminders.sql` | SECURITY DEFINER in local migration evidence; production not verified | Local evidence indicates missing explicit proconfig/search_path; production not verified | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | Local body/gate category: user-owned personal reminder read via current user identity; body not pasted. | Local migration evidence; `NotificationPushReminderContractAudit.md` references reminder RPC use; no exact `joinfolk-web` call-site found. | P1 / Unknown candidate | Future proconfig hardening review and grant exposure review after production metadata | Yes | Yes | No |
| `publish_event(p_event_id uuid, p_visibility text)` | TBD / Requires approved metadata collection | Handbook-only candidate; exact local migration definition not found under this name | TBD / Requires approved metadata collection | Handbook candidate; exact local state TBD | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | Handbook lifecycle audits classify older publish function as legacy/unknown; `joinfolk-web` local call-sites use `publish_event_with_groups_and_snapshot*`, not this exact name. | P0/P1 candidate | Defer pending production metadata verification and exact legacy/current publish mapping | Yes | Yes | No |
| `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])` | TBD / Requires approved metadata collection | Handbook-only candidate; exact local migration definition not found under this name | TBD / Requires approved metadata collection | Handbook candidate; exact local state TBD | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | Handbook audit reports deprecated behavior in supplied output; body not reviewed here. | Handbook lifecycle audits classify older publish function as legacy/unknown; `joinfolk-web` local call-sites use `publish_event_with_groups_and_snapshot*`, not this exact name. | P0/P1 candidate | Defer pending production metadata verification and exact legacy/current publish mapping | Yes | Yes | No |
| `upsert_personal_reminder(p_id uuid, p_title text, p_note text, p_reminder_date date, p_color_theme text, p_remind_before_days integer)` | public, local migration evidence only; production not verified | Local migration evidence: `[hostos]/supabase/migrations/20260314_personal_reminders.sql` | SECURITY DEFINER in local migration evidence; production not verified | Local evidence indicates missing explicit proconfig/search_path; production not verified | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | Local body/gate category: user-owned personal reminder create/update via current user identity; body not pasted. | Local migration evidence; `NotificationPushReminderContractAudit.md` references reminder RPC use; no exact `joinfolk-web` call-site found. | P1 / Unknown candidate | Future proconfig hardening review and grant exposure review after production metadata | Yes | Yes | No |

## 7. Grant Exposure Table

| Function | anon execute | authenticated execute | service/internal execute | PUBLIC/inherited execute | Exposure status | Missing evidence |
|---|---|---|---|---|---|
| `control_cancel_event(event_id uuid)` | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | Handbook audit references broad grants for live candidate; local exact definition not found; production not verified. | Exact production grant metadata and local/prod function mapping. |
| `control_end_event(event_id uuid)` | Local-only migration reference indicates prior broad execute removal; production not verified | Local-only migration reference indicates authenticated execute restoration; production not verified | TBD / Requires approved metadata collection | Local-only migration reference indicates inherited/broad execute removal; production not verified | Local-only evidence exists; production exposure not verified. | Exact production grant metadata and active function definition. |
| `control_open_checkin(event_id uuid)` | Handbook audit reports broad anon exposure; local grant line not found; production not verified | Handbook audit reports broad authenticated exposure; local grant line not found; production not verified | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | Handbook evidence only for exposure; production not verified in this task. | Exact production grant metadata. |
| `delete_personal_reminder(p_id uuid)` | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | Local definition found; local grant evidence not found. | Exact production grant metadata. |
| `list_active_reminders()` | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | Local definition found; local grant evidence not found. | Exact production grant metadata. |
| `list_personal_reminders()` | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | Local definition found; local grant evidence not found. | Exact production grant metadata. |
| `publish_event(p_event_id uuid, p_visibility text)` | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | Handbook-only candidate; exact local definition not found; production not verified. | Exact production grant metadata and legacy/current publish mapping. |
| `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])` | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | Handbook-only candidate; exact local definition not found; production not verified. | Exact production grant metadata and legacy/current publish mapping. |
| `upsert_personal_reminder(p_id uuid, p_title text, p_note text, p_reminder_date date, p_color_theme text, p_remind_before_days integer)` | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | TBD / Requires approved metadata collection | Local definition found; local grant evidence not found. | Exact production grant metadata. |

## 8. proconfig / search_path Table

| Function | SECURITY DEFINER candidate? | proconfig/search_path state | Missing evidence | Future hardening review required? |
|---|---|---|---|---|
| `control_cancel_event(event_id uuid)` | Candidate from PP-01 evidence | Handbook/PP-01 candidate; exact local definition not found; production not verified | exact current production state | Yes, pending metadata |
| `control_end_event(event_id uuid)` | Candidate from PP-01 evidence | Local evidence indicates missing explicit proconfig/search_path; production not verified | exact current production state | Yes, pending metadata |
| `control_open_checkin(event_id uuid)` | Candidate from PP-01 evidence | Local evidence indicates missing explicit proconfig/search_path; production not verified | exact current production state | Yes, pending metadata |
| `delete_personal_reminder(p_id uuid)` | Candidate from PP-01 evidence | Local evidence indicates missing explicit proconfig/search_path; production not verified | exact current production state | Yes, pending metadata |
| `list_active_reminders()` | Candidate from PP-01 evidence | Local evidence indicates missing explicit proconfig/search_path; production not verified | exact current production state | Yes, pending metadata |
| `list_personal_reminders()` | Candidate from PP-01 evidence | Local evidence indicates missing explicit proconfig/search_path; production not verified | exact current production state | Yes, pending metadata |
| `publish_event(p_event_id uuid, p_visibility text)` | Candidate from PP-01 evidence | Handbook/PP-01 candidate; exact local definition not found; production not verified | exact current production state | Yes, pending metadata |
| `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])` | Candidate from PP-01 evidence | Handbook/PP-01 candidate; exact local definition not found; production not verified | exact current production state | Yes, pending metadata |
| `upsert_personal_reminder(p_id uuid, p_title text, p_note text, p_reminder_date date, p_color_theme text, p_remind_before_days integer)` | Candidate from PP-01 evidence | Local evidence indicates missing explicit proconfig/search_path; production not verified | exact current production state | Yes, pending metadata |

## 9. Dependency Classification Table

| Function | Product domain | App/dashboard/mobile/web dependency | Edge dependency | Storage/RLS dependency | Smoke test scope | Dependency status |
|---|---|---|---|---|---|---|
| `control_cancel_event(event_id uuid)` | Event lifecycle / host authority | Handbook references cancel/control path; no exact `joinfolk-web` call-site found. | TBD / Missing | Handbook/RLS authority dependency; exact table dependency requires production metadata. | Event cancel/lifecycle smoke and negative tests if implemented later. | Handbook evidence found; exact local source/prod behavior not verified. |
| `control_end_event(event_id uuid)` | Event lifecycle / host authority | Local migration evidence; no exact `joinfolk-web` call-site found. | TBD / Missing | Local migration indicates events table lifecycle dependency; production behavior not verified. | Event end/lifecycle smoke and negative tests if implemented later. | Local migration evidence found; production behavior not verified. |
| `control_open_checkin(event_id uuid)` | Event lifecycle / host authority | Local migration evidence; no exact `joinfolk-web` call-site found. | TBD / Missing | Local migration indicates events table/check-in lifecycle dependency; production behavior not verified. | Check-in open/host authority smoke and negative tests if implemented later. | Local migration and handbook evidence found; production behavior not verified. |
| `delete_personal_reminder(p_id uuid)` | Notification/reminder/privacy | Handbook reminder audit says implementation uses reminder RPCs; no exact `joinfolk-web` call-site found. | TBD / Missing | Local migration indicates personal_reminders table/RLS dependency; production behavior not verified. | Reminder delete owner/non-owner smoke and negative tests if implemented later. | Local migration and handbook evidence found; production behavior not verified. |
| `list_active_reminders()` | Notification/reminder/privacy | Handbook reminder audit references active reminder RPC; no exact `joinfolk-web` call-site found. | TBD / Missing | Local migration indicates personal_reminders table/RLS dependency; production behavior not verified. | Reminder active-list owner/non-owner smoke and negative tests if implemented later. | Local migration and handbook evidence found; production behavior not verified. |
| `list_personal_reminders()` | Notification/reminder/privacy | Handbook reminder audit says implementation uses reminder RPCs; no exact `joinfolk-web` call-site found. | TBD / Missing | Local migration indicates personal_reminders table/RLS dependency; production behavior not verified. | Reminder list owner/non-owner smoke and negative tests if implemented later. | Local migration and handbook evidence found; production behavior not verified. |
| `publish_event(p_event_id uuid, p_visibility text)` | Event lifecycle / visibility / host authority | Handbook lifecycle audit classifies older publish function as legacy/unknown; `joinfolk-web` local call-sites use `publish_event_with_groups_and_snapshot*`, not exact candidate. | TBD / Missing | TBD / Requires approved metadata collection | Publish/visibility smoke and negative tests if exact function remains reachable. | Handbook evidence found; exact local definition and production behavior not verified. |
| `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])` | Event lifecycle / visibility / host authority | Handbook lifecycle audit classifies older publish function as legacy/unknown; `joinfolk-web` local call-sites use `publish_event_with_groups_and_snapshot*`, not exact candidate. | TBD / Missing | TBD / Requires approved metadata collection | Publish/group visibility smoke and negative tests if exact function remains reachable. | Handbook evidence found; exact local definition and production behavior not verified. |
| `upsert_personal_reminder(p_id uuid, p_title text, p_note text, p_reminder_date date, p_color_theme text, p_remind_before_days integer)` | Notification/reminder/privacy | Handbook reminder audit says implementation uses reminder RPCs; no exact `joinfolk-web` call-site found. | TBD / Missing | Local migration indicates personal_reminders table/RLS dependency; production behavior not verified. | Reminder upsert owner/non-owner smoke and negative tests if implemented later. | Local migration and handbook evidence found; production behavior not verified. |

## 10. Missing Evidence

### Local evidence reviewed

- Local migration references under `[hostos]/supabase\migrations` for exact candidate names.
- Local migration references for `control_open_checkin`, `control_end_event`, and personal reminder RPCs.
- Local migration grant/revoke references for `control_end_event`.
- Local `joinfolk-web` call-site search for exact candidate names.
- Local `joinfolk-web` publish call-site references for the related `publish_event_with_groups_and_snapshot*` function family.
- Committed handbook/audit classifications in `SupabaseFocusedBackendFollowUpReport.md`, `EventLifecycleContractAudit.md`, and `NotificationPushReminderContractAudit.md`.

### Still missing / requires approved metadata collection

- Exact production function schema for every candidate.
- Exact production function owner/source.
- Exact app-owned vs platform-managed classification for every candidate.
- Exact production SECURITY DEFINER vs SECURITY INVOKER state for every candidate.
- Exact production proconfig/search_path state for every candidate.
- Exact production anon EXECUTE exposure per function.
- Exact production authenticated EXECUTE exposure per function.
- Exact production service_role/internal exposure per function.
- Exact production PUBLIC/inherited exposure per function.
- Sanitized body/gate classification for candidates without local body evidence.
- Complete app/dashboard/mobile/web call-site dependency classification across all active surfaces.
- Edge Function dependency classification.
- Production table/RLS dependency classification.
- Storage dependency classification.
- Intended target caller role.
- Proposed future action.
- Rollback requirement.
- Verification requirement.
- Smoke test requirement.
- Risk class finalization.
- Production parity evidence.

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

Local source/migration/call-site review was performed for this update.

Production metadata collection was not executed.

The approval gate allows only bounded sanitized metadata collection under strict constraints.

This shell must be updated only with sanitized metadata after approved collection is actually performed.

If private data, secrets, object paths, payloads, or credentials appear in output, collection must stop and output must not be committed.

No production connection was made.

No SQL was executed.

No RPC/function was invoked.

No private rows/storage objects were inspected.

Any exact production metadata remains TBD until approved metadata collection.

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
- No production metadata collection was executed during creation of this file.
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

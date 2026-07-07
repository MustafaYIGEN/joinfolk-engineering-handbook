# Security Definer Function Grant Rollback Plan

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: committed production metadata report and owner review
- canonical: false
- Plan status: Rollback planning only
- Implementation status: Not authorized
- Production mutation status: Not executed
- Legal status: Engineering rollback plan only; not legal advice

## 2. Purpose

- This document defines rollback requirements for a future SecurityDefiner/function grant/proconfig hardening patch.
- It does not authorize implementation.
- It does not include executable SQL.
- It does not create migrations.
- It does not perform production metadata collection.
- It does not connect to production.
- It does not invoke RPCs/functions.
- It does not inspect private rows or storage objects.

## 3. Rollback Scope

Candidate functions:

- `control_cancel_event(event_id uuid)`
- `control_end_event(event_id uuid)`
- `control_open_checkin(event_id uuid)`
- `delete_personal_reminder(p_id uuid)`
- `list_active_reminders()`
- `list_personal_reminders()`
- `publish_event(p_event_id uuid, p_visibility text)`
- `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])`
- `upsert_personal_reminder(p_id uuid, p_title text, p_note text, p_reminder_date date, p_color_theme text, p_remind_before_days integer)`

Future implementation may include all or a subset, but rollback must explicitly match the final implementation scope.

No function body is included.

No executable SQL is included.

## 4. Pre-Change Snapshot Requirements

| Snapshot item | Required? | Purpose | Private data allowed? |
| --- | --- | --- | --- |
| Function identity signatures | Yes | Ensure rollback targets exact overloads | No |
| Owner role metadata | Yes | Detect unexpected owner drift | No |
| SECURITY DEFINER/INVOKER state | Yes | Restore or compare security mode | No |
| proconfig/search_path state | Yes | Restore previous proconfig state if needed | No |
| row_security proconfig state | Yes | Restore previous proconfig state if needed | No |
| effective execute matrix | Yes | Compare anon/authenticated/service_role/PUBLIC state | No |
| explicit grant matrix | Yes | Restore explicit grants if rollback required | No |
| migration ID / patch ID | Yes | Trace deployment and rollback | No |
| function body text | No | Body leakage is out of scope for this rollback plan | No |
| private rows/storage objects | No | Not required and prohibited | No |

## 5. Rollback Strategy

- Rollback must restore the pre-change grant/proconfig state for each function included in the implementation patch.
- Rollback must restore only metadata-level changes made by the patch.
- Rollback must not alter function bodies unless a future implementation patch changes function definitions.
- Rollback must not broaden grants beyond the pre-change snapshot.
- Rollback must not rely on memory or assumptions.
- Rollback must be generated from pre-change snapshot evidence and the exact implementation diff.
- Rollback must be reviewed before any production execution.

No SQL is written here.

## 6. Function-Level Rollback Matrix

| Function | Rollback object | Required rollback capability | Notes |
| --- | --- | --- | --- |
| `control_cancel_event(event_id uuid)` | grant/proconfig metadata | restore pre-change EXECUTE grants and pre-change proconfig/search_path state | Event lifecycle function; preserve host/control lifecycle behavior after rollback. |
| `control_end_event(event_id uuid)` | grant/proconfig metadata | restore pre-change EXECUTE grants and pre-change proconfig/search_path state | Event lifecycle function; preserve host/control lifecycle behavior after rollback. |
| `control_open_checkin(event_id uuid)` | grant/proconfig metadata | restore pre-change EXECUTE grants and pre-change proconfig/search_path state | Event lifecycle function; preserve host/control lifecycle behavior after rollback. |
| `delete_personal_reminder(p_id uuid)` | grant/proconfig metadata | restore pre-change EXECUTE grants and pre-change proconfig/search_path state | Reminder function; preserve owner/privacy behavior after rollback. |
| `list_active_reminders()` | grant/proconfig metadata | restore pre-change EXECUTE grants and pre-change proconfig/search_path state | Reminder function; preserve owner/privacy behavior after rollback. |
| `list_personal_reminders()` | grant/proconfig metadata | restore pre-change EXECUTE grants and pre-change proconfig/search_path state | Reminder function; preserve owner/privacy behavior after rollback. |
| `publish_event(p_event_id uuid, p_visibility text)` | grant/proconfig metadata | restore pre-change EXECUTE grants and pre-change proconfig/search_path state | Publish function; preserve active/deprecated mapping until separately resolved. |
| `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])` | grant/proconfig metadata | restore pre-change EXECUTE grants and pre-change proconfig/search_path state | Publish function; preserve active/deprecated mapping until separately resolved. |
| `upsert_personal_reminder(p_id uuid, p_title text, p_note text, p_reminder_date date, p_color_theme text, p_remind_before_days integer)` | grant/proconfig metadata | restore pre-change EXECUTE grants and pre-change proconfig/search_path state | Reminder function; preserve owner/privacy behavior after rollback. |

## 7. Failure Triggers

- app/dashboard flow fails after patch.
- publish lifecycle flow fails.
- check-in lifecycle flow fails.
- reminder create/list/delete flow fails.
- intended caller loses required access.
- unintended caller still has access after expected restriction.
- metadata re-check does not match expected post-change state.
- proconfig/search_path hardening not applied as expected.
- grant matrix differs from expected state.
- production error rate or support signals indicate regression.
- rollback verification cannot be completed.

## 8. Rollback Verification Requirements

Rollback verification must confirm:

- exact function signatures still exist.
- grants match pre-change snapshot or approved rollback target.
- proconfig/search_path state matches pre-change snapshot or approved rollback target.
- affected app/dashboard flows return to pre-change behavior.
- no private row data was inspected.
- no storage objects were listed.
- no function bodies were exposed.
- no secrets were printed.

No SQL is written here.

## 9. Rollback Exclusions

- This rollback plan does not authorize production execution.
- This rollback plan does not authorize source changes.
- This rollback plan does not authorize SQL/migration creation.
- This rollback plan does not authorize function body changes.
- This rollback plan does not authorize RLS policy changes.
- This rollback plan does not authorize storage policy changes.
- This rollback plan does not authorize Edge Function changes.
- This rollback plan does not authorize dashboard/admin/support actions.

## 10. Required Companion Verification Plan

- A separate verification plan is required before implementation.
- Rollback plan alone is not sufficient.
- Verification plan must define post-patch metadata checks, role-based checks, and smoke tests.
- Implementation remains blocked until rollback and verification plans are owner-reviewed.

## 11. Decision

- Rollback planning requirements are defined.
- Rollback execution is not authorized.
- Implementation is not authorized.
- Next valid artifact is the verification plan.
- No implementation prompt is authorized yet.

## 12. Required Next Gates

| Next gate | Required before | Status |
| --- | --- | --- |
| Verification plan | Any implementation prompt | Required |
| Owner review of rollback + verification | Any implementation prompt | Required |
| Implementation prompt | Any SQL/migration/source/grant/proconfig change | Not authorized |

## 13. Risk Position

- Risk remains P0/P1 candidate.
- Rollback planning reduces implementation risk only after verification planning is also complete.
- Rollback planning does not reduce production risk by itself.
- No exploitability claim is made.
- No production safe/unsafe final claim is made.
- No launch-ready claim is made.
- No security hardened claim is made.
- No function grants fixed claim is made.

## 14. Implementation Authorization Status

- Implementation remains not authorized.
- No source change, SQL, executable SQL, migration, grant change, function change, proconfig change, production mutation, Supabase CLI action, dashboard action, verification query, RPC invocation, deployment action, private row inspection, or storage object listing is authorized by this rollback plan.
- A separate owner-approved implementation prompt is required after rollback plan and verification plan are reviewed.

## 15. Explicitly Blocked Claims

- Do not claim exploitability.
- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim security hardened.
- Do not claim function grants fixed.
- Do not claim rollback executed.
- Do not claim implementation authorized.
- Do not claim all RPC/function risk is resolved.

## 16. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No executable SQL was written in this file.
- No production connection was made by this documentation task.
- No production mutation was executed.
- Supabase CLI was not run.
- No dashboard action was performed.
- No verification query was executed.
- No RPC/function was invoked by this documentation task.
- No private rows were inspected.
- No storage objects were listed.
- No builds/tests/installs were run.
- No function bodies are included.
- No secret or environment variable value was inspected, copied, printed, rotated, or changed.
- No credentials, hostnames, full project refs, service_role keys, database passwords, connection strings, webhook secrets, API keys, environment variable values, or secrets were included.
- No files were staged or committed.
- Only `08_PatchPlans/SecurityDefinerFunctionGrantRollbackPlan.md` was created/modified.

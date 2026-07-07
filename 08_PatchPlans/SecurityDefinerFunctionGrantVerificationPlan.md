# Security Definer Function Grant Verification Plan

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: committed production metadata report, owner review, and rollback plan
- canonical: false
- Plan status: Verification planning only
- Implementation status: Not authorized
- Production mutation status: Not executed
- Legal status: Engineering verification plan only; not legal advice

## 2. Purpose

- This document defines verification requirements for a future SecurityDefiner/function grant/proconfig hardening patch.
- It does not authorize implementation.
- It does not include executable SQL.
- It does not create migrations.
- It does not perform production verification.
- It does not connect to production.
- It does not invoke RPCs/functions.
- It does not inspect private rows or storage objects.

## 3. Verification Scope

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

Future implementation may include all or a subset, but verification must explicitly match the final implementation scope.

No function body is included.

No executable SQL is included.

## 4. Verification Objectives

| Objective | Required? | Purpose | Private data allowed? |
| --- | --- | --- | --- |
| Function identity verification | Yes | Confirm exact overloads remain targeted | No |
| SECURITY DEFINER/INVOKER verification | Yes | Confirm expected post-change security mode | No |
| proconfig/search_path verification | Yes | Confirm expected hardening state | No |
| row_security proconfig verification | Yes | Confirm expected setting or approved absence | No |
| effective execute matrix verification | Yes | Confirm anon/authenticated/service_role/PUBLIC state | No |
| explicit grant matrix verification | Yes | Confirm explicit grants match expected state | No |
| intended caller verification | Yes | Confirm required app roles still work | No private row inspection |
| unintended caller negative verification | Yes, where safe and owner-approved | Confirm restricted roles cannot call unauthorized paths | No private row inspection |
| app/dashboard smoke tests | Yes | Confirm product flows still work | No private payload logging |
| rollback verification readiness | Yes | Confirm rollback can be validated if needed | No |
| function body text review | No | Body leakage is out of scope for this verification plan | No |
| private rows/storage objects | No | Not required and prohibited | No |

## 5. Metadata Verification Requirements

Verification must confirm:

- exact function signatures still exist.
- owner role is unchanged unless explicitly approved.
- SECURITY DEFINER/INVOKER state matches expected post-patch state.
- proconfig/search_path state matches expected post-patch state.
- row_security proconfig state matches expected post-patch state or approved absence.
- effective execute exposure for anon/authenticated/service_role/PUBLIC matches expected post-patch matrix.
- explicit execute exposure for anon/authenticated/service_role/PUBLIC matches expected post-patch matrix.
- no function bodies are printed.
- no private rows are inspected.
- no storage objects are listed.

No SQL is written here.

## 6. Role-Based Verification Requirements

| Role / caller category | Verification purpose | Allowed verification type | Private data allowed? |
| --- | --- | --- | --- |
| anon | Confirm intended restrictions for public/anonymous access | Metadata and safe negative check only if owner-approved | No |
| authenticated | Confirm intended app-facing access remains only where required | Metadata and safe smoke/negative checks only if owner-approved | No |
| service_role/internal | Confirm internal access remains available where intentionally required | Metadata only unless owner-approved | No |
| host/event owner | Confirm event lifecycle control remains functional where required | Safe app/dashboard smoke test with controlled test event only if owner-approved | No private payload logging |
| non-owner authenticated user | Confirm unauthorized lifecycle/reminder actions are blocked | Safe negative test only if owner-approved | No private payload logging |
| reminder owner | Confirm reminder owner flows remain functional | Safe smoke test only if owner-approved | No private payload logging |
| reminder non-owner | Confirm privacy boundary remains enforced | Safe negative test only if owner-approved | No private payload logging |

## 7. Product Smoke Test Scope

| Product area | Candidate functions | Required smoke test | Status before implementation |
| --- | --- | --- | --- |
| Event lifecycle cancel | `control_cancel_event(event_id uuid)` | Host can cancel where allowed; non-owner cannot cancel | Required |
| Event lifecycle end | `control_end_event(event_id uuid)` | Host can end where allowed; non-owner cannot end | Required |
| Check-in open/control | `control_open_checkin(event_id uuid)` | Host can open check-in where allowed; non-owner cannot | Required |
| Publish legacy/simple | `publish_event(p_event_id uuid, p_visibility text)` | Active/deprecated mapping must be resolved before smoke test | Required |
| Publish groups legacy | `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])` | Active/deprecated mapping must be resolved before smoke test | Required |
| Reminder delete | `delete_personal_reminder(p_id uuid)` | Owner can delete own reminder; non-owner cannot | Required |
| Reminder active list | `list_active_reminders()` | Intended caller receives only authorized reminder scope | Required |
| Reminder personal list | `list_personal_reminders()` | Intended caller receives only authorized reminder scope | Required |
| Reminder upsert | `upsert_personal_reminder(...)` | Owner can create/update own reminder; non-owner cannot affect another user | Required |

## 8. Function-Level Verification Matrix

| Function | Metadata verification | Role/negative verification | Product smoke verification | Implementation authorized? |
| --- | --- | --- | --- | --- |
| `control_cancel_event(event_id uuid)` | Required | host vs non-owner required where safe and owner-approved | Event lifecycle cancel smoke scope | No |
| `control_end_event(event_id uuid)` | Required | host vs non-owner required where safe and owner-approved | Event lifecycle end smoke scope | No |
| `control_open_checkin(event_id uuid)` | Required | host vs non-owner required where safe and owner-approved | Check-in open/control smoke scope | No |
| `delete_personal_reminder(p_id uuid)` | Required | owner vs non-owner required where safe and owner-approved | Reminder delete smoke scope | No |
| `list_active_reminders()` | Required | owner vs non-owner required where safe and owner-approved | Reminder active-list smoke scope | No |
| `list_personal_reminders()` | Required | owner vs non-owner required where safe and owner-approved | Reminder personal-list smoke scope | No |
| `publish_event(p_event_id uuid, p_visibility text)` | Required | intended caller + active/deprecated mapping required | Publish legacy/simple smoke scope | No |
| `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])` | Required | intended caller + active/deprecated mapping required | Publish groups legacy smoke scope | No |
| `upsert_personal_reminder(p_id uuid, p_title text, p_note text, p_reminder_date date, p_color_theme text, p_remind_before_days integer)` | Required | owner vs non-owner required where safe and owner-approved | Reminder upsert smoke scope | No |

## 9. Pass Criteria

- All functions included in final patch must have post-change metadata matching expected matrix.
- No unexpected broad EXECUTE exposure remains unless explicitly owner-accepted and documented.
- proconfig/search_path hardening must match expected state.
- intended app/dashboard flows must still work.
- unauthorized role/caller paths must fail where negative tests are approved.
- no private rows are inspected.
- no storage objects are listed.
- no function bodies are exposed.
- no secrets are printed.
- rollback readiness must be confirmed.

## 10. Fail Criteria

- metadata state differs from expected post-change state.
- required caller loses access.
- unauthorized caller retains access where restriction was expected.
- proconfig/search_path hardening is missing.
- app/dashboard smoke test fails.
- publish lifecycle smoke test fails.
- check-in lifecycle smoke test fails.
- reminder/privacy smoke test fails.
- rollback verification cannot be completed.
- private data appears in output.
- storage objects are listed.
- secrets or credentials appear in output.
- function bodies are printed.

## 11. Verification Exclusions

- This verification plan does not authorize production execution.
- This verification plan does not authorize source changes.
- This verification plan does not authorize SQL/migration creation.
- This verification plan does not authorize function body changes.
- This verification plan does not authorize RLS policy changes.
- This verification plan does not authorize storage policy changes.
- This verification plan does not authorize Edge Function changes.
- This verification plan does not authorize dashboard/admin/support actions.
- This verification plan does not authorize private row inspection.
- This verification plan does not authorize storage object listing.

## 12. Required Companion Rollback Plan

- Rollback plan already exists at `08_PatchPlans/SecurityDefinerFunctionGrantRollbackPlan.md`.
- Verification plan must be reviewed together with rollback plan.
- Verification plan alone is not sufficient.
- Implementation remains blocked until rollback and verification plans are owner-reviewed.

## 13. Decision

- Verification planning requirements are defined.
- Verification execution is not authorized.
- Implementation is not authorized.
- Next valid artifact is owner review of rollback + verification plans.
- No implementation prompt is authorized yet.

## 14. Required Next Gates

| Next gate | Required before | Status |
| --- | --- | --- |
| Owner review of rollback + verification | Any implementation prompt | Required |
| Implementation prompt | Any SQL/migration/source/grant/proconfig change | Not authorized |

## 15. Risk Position

- Risk remains P0/P1 candidate.
- Verification planning reduces implementation risk only after owner review and approved implementation scope.
- Verification planning does not reduce production risk by itself.
- No exploitability claim is made.
- No production safe/unsafe final claim is made.
- No launch-ready claim is made.
- No security hardened claim is made.
- No function grants fixed claim is made.

## 16. Implementation Authorization Status

- Implementation remains not authorized.
- No source change, SQL, executable SQL, migration, grant change, function change, proconfig change, production mutation, Supabase CLI action, dashboard action, verification query execution, RPC invocation, deployment action, private row inspection, or storage object listing is authorized by this verification plan.
- A separate owner-approved implementation prompt is required after rollback plan and verification plan are reviewed.

## 17. Explicitly Blocked Claims

- Do not claim exploitability.
- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim security hardened.
- Do not claim function grants fixed.
- Do not claim verification executed.
- Do not claim implementation authorized.
- Do not claim all RPC/function risk is resolved.

## 18. No-Modification Confirmation

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
- Only `08_PatchPlans/SecurityDefinerFunctionGrantVerificationPlan.md` was created/modified.

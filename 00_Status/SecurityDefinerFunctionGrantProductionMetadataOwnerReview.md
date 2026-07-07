# Security Definer Function Grant Production Metadata Owner Review

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: committed collected metadata report with owner-provided sanitized production metadata
- canonical: false
- Review status: Production metadata reviewed for hardening planning
- Implementation status: Not authorized
- Production mutation status: Not executed
- Legal status: Engineering status review only; not legal advice

## 2. Purpose

This status artifact reviews the sanitized production metadata recorded in the collected metadata report.

It does not perform production metadata collection.

It does not authorize implementation.

It does not authorize SQL, executable SQL, migration creation, source changes, production access, Supabase CLI, RPC/function invocation, private row inspection, storage object listing, or deployment.

## 3. Reviewed Artifacts

- `07_Audits/SecurityDefinerFunctionGrantCollectedMetadataReport.md`
- `00_Status/SecurityDefinerFunctionGrantLocalEvidenceOwnerReview.md`
- `08_PatchPlans/SecurityDefinerFunctionGrantMetadataCollectionPlan.md`
- `00_Status/SecurityDefinerFunctionGrantMetadataCollectionApprovalGate.md`
- `08_PatchPlans/SecurityDefinerAndFunctionGrantHardeningPatchPlan.md`
- `09_Decisions/SecurityDefinerAndFunctionGrantHardeningDecision.md`
- `07_Audits/AuditIndex.md`
- `08_PatchPlans/PatchPlanIndex.md`
- `00_Status/StatusIndex.md`

## 4. Production Metadata Summary

| Finding | Count / Scope | Owner interpretation | Implementation authorized? |
| --- | --- | --- | --- |
| Listed functions exist in production metadata | 9/9 | Production metadata confirms the candidate set exists under recorded metadata | No |
| SECURITY DEFINER | 9/9 | All listed functions require hardening review | No |
| NO_PROCONFIG / no search_path | 9/9 | All listed functions are search_path/proconfig hardening candidates | No |
| No row_security setting | 9/9 | Requires owner review before deciding standard | No |
| effective anon EXECUTE true | 8/9 | Broad app-facing exposure candidate; not exploitability proof | No |
| effective authenticated EXECUTE true | 8/9 | Broad app-facing exposure candidate; not exploitability proof | No |
| effective service_role EXECUTE true | 9/9 | Expected or internal exposure must still be reviewed per function | No |
| effective PUBLIC EXECUTE true | 5/9 | PUBLIC/inherited exposure candidate; high-priority grant review | No |

## 5. Function-Level Owner Classification

| Function | Metadata-confirmed state | Owner classification | Required next action | Implementation authorized? |
| --- | --- | --- | --- | --- |
| `control_cancel_event(event_id uuid)` | SECURITY DEFINER; NO_PROCONFIG; anon/auth/service execute true; public false | P0/P1 event lifecycle authority hardening candidate | Body/gate review, intended caller review, rollback plan, verification plan | No |
| `control_end_event(event_id uuid)` | SECURITY DEFINER; NO_PROCONFIG; service execute true only among listed roles | P0/P1 event lifecycle authority hardening candidate | Body/gate review, intended caller review, rollback plan, verification plan | No |
| `control_open_checkin(event_id uuid)` | SECURITY DEFINER; NO_PROCONFIG; anon/auth/service execute true; public false | P0/P1 check-in authority hardening candidate | Body/gate review, intended caller review, rollback plan, verification plan | No |
| `delete_personal_reminder(p_id uuid)` | SECURITY DEFINER; NO_PROCONFIG; anon/auth/service/public execute true | P1/P0 privacy/reminder authority hardening candidate | Intended caller review, public/anon grant review, rollback plan, verification plan | No |
| `list_active_reminders()` | SECURITY DEFINER; NO_PROCONFIG; anon/auth/service/public execute true | P1/P0 privacy/reminder read exposure candidate | Intended caller review, public/anon grant review, rollback plan, verification plan | No |
| `list_personal_reminders()` | SECURITY DEFINER; NO_PROCONFIG; anon/auth/service/public execute true | P1/P0 privacy/reminder read exposure candidate | Intended caller review, public/anon grant review, rollback plan, verification plan | No |
| `publish_event(p_event_id uuid, p_visibility text)` | SECURITY DEFINER; NO_PROCONFIG; anon/auth/service/public execute true | P0/P1 publish/visibility authority hardening candidate; legacy/current mapping unclear | Active/deprecated mapping, intended caller review, rollback plan, verification plan | No |
| `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])` | SECURITY DEFINER; NO_PROCONFIG; anon/auth/service execute true; public false | P0/P1 publish/group visibility authority hardening candidate; legacy/current mapping unclear | Active/deprecated mapping, intended caller review, rollback plan, verification plan | No |
| `upsert_personal_reminder(p_id uuid, p_title text, p_note text, p_reminder_date date, p_color_theme text, p_remind_before_days integer)` | SECURITY DEFINER; NO_PROCONFIG; anon/auth/service/public execute true | P1/P0 privacy/reminder write exposure candidate | Intended caller review, public/anon grant review, rollback plan, verification plan | No |

## 6. Owner Findings

- Production metadata is accepted as sufficient to confirm the metadata-level hardening problem.
- Production metadata confirms the candidate functions are SECURITY DEFINER with NO_PROCONFIG and no search_path setting.
- Production metadata confirms broad EXECUTE exposure for multiple candidates.
- Production metadata is not sufficient to prove exploitability.
- Production metadata is not sufficient to prove function body/gate correctness.
- Production metadata is not sufficient to prove runtime behavior.
- Implementation remains blocked until rollback and verification planning are completed.
- Security/function grant hardening is not complete.

## 7. Remaining Evidence Gaps

- Function body/gate behavior.
- Intended caller role per function.
- Active/deprecated mapping for publish legacy functions.
- Call-site completeness across mobile/web/dashboard/edge.
- Table/RLS dependency finalization.
- Rollback plan.
- Verification plan.
- Smoke test plan.
- Exact implementation patch scope.
- Owner acceptance/defer decision for any intentionally broad grant.

## 8. Required Rollback Planning

Rollback plan must define:

- pre-change grant/proconfig snapshot evidence.
- exact functions included.
- exact grant restoration strategy.
- exact proconfig restoration strategy.
- failure triggers.
- verification after rollback.
- no private data requirement.
- no function body leakage requirement.

No SQL is written here.

## 9. Required Verification Planning

Verification plan must define:

- metadata re-check after patch.
- grant matrix re-check after patch.
- proconfig/search_path re-check after patch.
- role-based negative checks where safe and approved.
- app/dashboard smoke tests for affected flows.
- publish lifecycle smoke tests.
- check-in lifecycle smoke tests.
- reminder/privacy smoke tests.
- no private row exposure.
- no storage object listing.

No SQL is written here.

## 10. Decision

- Production metadata owner review is accepted as a hardening planning input.
- Metadata-level hardening problem is confirmed.
- Implementation is not authorized by this review.
- Next valid artifacts are:
  1. rollback plan.
  2. verification plan.
  3. implementation patch prompt only after owner approval.
- No implementation prompt is authorized yet.

## 11. Required Next Gates

| Next gate | Required before | Status |
| --- | --- | --- |
| Rollback plan | Any implementation prompt | Required |
| Verification plan | Any implementation prompt | Required |
| Owner approval of rollback + verification | Any implementation prompt | Required |
| Implementation prompt | Any SQL/migration/source/grant/proconfig change | Not authorized |

## 12. Risk Position

- Risk remains P0/P1 candidate.
- Production metadata reduces uncertainty at metadata level.
- Production metadata does not reduce production risk by itself.
- No exploitability claim is made.
- No production safe/unsafe final claim is made.
- No launch-ready claim is made.
- No security hardened claim is made.
- No function grants fixed claim is made.

## 13. Implementation Authorization Status

- Implementation remains not authorized.
- No source change, SQL, executable SQL, migration, grant change, function change, proconfig change, production mutation, Supabase CLI action, dashboard action, verification query, RPC invocation, deployment action, private row inspection, or storage object listing is authorized by this review.
- A separate owner-approved implementation prompt is required after rollback plan and verification plan are reviewed.

## 14. Explicitly Blocked Claims

- Do not claim exploitability.
- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim security hardened.
- Do not claim function grants fixed.
- Do not claim all RPC/function risk is resolved.
- Do not claim implementation authorized.

## 15. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
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
- Only `00_Status/SecurityDefinerFunctionGrantProductionMetadataOwnerReview.md` was created/modified.

# Security Definer Function Grant Pre-Change Production Snapshot Plan

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: committed local migration preparation execution report owner review and prior gates
- canonical: false
- Plan status: Pre-change production snapshot planning only
- Snapshot execution status: Not authorized
- Production mutation status: Not executed
- Supabase CLI status: Not executed
- Legal status: Engineering snapshot plan only; not legal advice

## 2. Purpose

- This plan defines the sanitized metadata-only pre-change production snapshot required before any production mutation.
- This plan does not authorize snapshot execution.
- This plan does not authorize production SQL execution.
- This plan does not authorize production mutation.
- This plan does not authorize Supabase CLI.
- This plan does not authorize applying the local migration.
- This plan does not authorize post-change verification.
- This plan does not inspect private rows or storage objects.

## 3. Reviewed Artifacts

- `00_Status/SecurityDefinerFunctionGrantLocalMigrationPreparationExecutionReportOwnerReview.md`
- `00_Status/SecurityDefinerFunctionGrantLocalMigrationPreparationExecutionReport.md`
- `00_Status/SecurityDefinerFunctionGrantLocalMigrationPreparationPromptOwnerApproval.md`
- `08_PatchPlans/SecurityDefinerFunctionGrantLocalMigrationPreparationPrompt.md`
- `08_PatchPlans/SecurityDefinerFunctionGrantBoundedImplementationPrompt.md`
- `08_PatchPlans/SecurityDefinerFunctionGrantRollbackPlan.md`
- `08_PatchPlans/SecurityDefinerFunctionGrantVerificationPlan.md`
- `07_Audits/SecurityDefinerFunctionGrantCollectedMetadataReport.md`

## 4. Snapshot Objective

- The snapshot objective is to capture sanitized pre-change production metadata for the Phase 1 function scope.
- The snapshot is required so production rollback and post-change verification can compare before/after metadata.
- The snapshot must capture metadata only.
- The snapshot must not include function bodies.
- The snapshot must not include private table rows.
- The snapshot must not include storage object names or paths.
- The snapshot must not include secrets or raw project identifiers.
- The snapshot must not mutate production.

## 5. Function Scope

- `public.control_cancel_event(uuid)`
- `public.control_end_event(uuid)`
- `public.control_open_checkin(uuid)`
- `public.delete_personal_reminder(uuid)`
- `public.list_active_reminders()`
- `public.list_personal_reminders()`
- `public.publish_event(uuid, text)`
- `public.publish_event_with_groups(uuid, text, uuid[])`
- `public.upsert_personal_reminder(uuid, text, text, date, text, integer)`

Snapshot execution must target only exact matching signatures.

Missing or changed signatures must be recorded and escalated.

No function body text may be captured.

No private data may be captured.

## 6. Required Snapshot Metadata Fields

| Field | Required? | Privacy classification | Purpose |
| --- | --- | --- | --- |
| Schema name | Yes | Sanitized metadata | Confirm target schema |
| Function name | Yes | Sanitized metadata | Confirm target function |
| Identity argument types | Yes | Sanitized metadata | Confirm exact overload |
| Full identity signature | Yes | Sanitized metadata | Prevent overload ambiguity |
| Function owner role | Yes | Sanitized role metadata | Rollback and drift detection |
| SECURITY DEFINER/INVOKER mode | Yes | Sanitized metadata | Confirm security mode preservation |
| Function-level proconfig | Yes | Sanitized metadata | Capture current search_path/proconfig state |
| search_path state | Yes | Sanitized metadata | Confirm pre-change value |
| row_security proconfig state | Yes | Sanitized metadata | Confirm absence/presence before change |
| Effective EXECUTE matrix | Yes | Sanitized role metadata | Capture anon/authenticated/service_role/PUBLIC effective access |
| Explicit EXECUTE grant matrix | Yes | Sanitized role metadata | Capture direct grants before change |
| Function body text | No | Prohibited | Not needed and prohibited |
| Private row values | No | Prohibited | Not needed and prohibited |
| Storage object names/paths | No | Prohibited | Not needed and prohibited |
| Secrets/credentials/project refs | No | Prohibited | Prohibited |

## 7. Required Snapshot Output Shape

- The future snapshot execution output must be a sanitized report, not raw unrestricted database output.
- The output must group results by function signature.
- Each function must have one row or one clearly bounded record.
- The report must include whether each function was found.
- The report must include whether each exact signature matched.
- The report must include current owner, security mode, proconfig/search_path, row_security proconfig state, effective grant matrix, and explicit grant matrix.
- The report must not include function body text.
- The report must not include private data.
- The report must not include storage object data.
- The report must not include secrets, hostnames, or full project identifiers.

## 8. Allowed Snapshot Categories

| Category | Allowed? | Notes |
| --- | --- | --- |
| System catalog function metadata | Yes | Metadata-only, no function body text |
| Role/grant metadata | Yes | Sanitized role-level metadata only |
| Function proconfig metadata | Yes | Required for search_path comparison |
| Function owner metadata | Yes | Required for rollback comparison |
| Function body text | No | Prohibited |
| Application table rows | No | Prohibited |
| Private user/event/order/payment data | No | Prohibited |
| Storage object listing | No | Prohibited |
| Secrets/environment variables | No | Prohibited |
| Supabase project identifiers/URLs | No | Prohibited |

## 9. Required Future Snapshot Execution Prompt Constraints

- Future snapshot execution must be separately owner-approved.
- Future snapshot execution must be read-only.
- Future snapshot execution must be metadata-only.
- Future snapshot execution must not mutate production.
- Future snapshot execution must not run Supabase CLI unless separately approved.
- Future snapshot execution must not inspect private rows.
- Future snapshot execution must not list storage objects.
- Future snapshot execution must not print function bodies.
- Future snapshot execution must not print secrets or full project identifiers.
- Future snapshot execution must produce a reviewable snapshot report before any production mutation.

## 10. Rollback Linkage

- The snapshot is required for rollback readiness.
- Rollback must restore pre-change proconfig/search_path state if Phase 1 must be reverted.
- Rollback must preserve pre-change grants.
- Rollback must not broaden grants beyond the snapshot.
- Rollback must not use function bodies.
- Rollback execution remains not authorized by this plan.

## 11. Verification Linkage

- The snapshot is required for post-change verification comparison.
- Post-change verification must compare pre-change and post-change proconfig/search_path state.
- Post-change verification must confirm grants were preserved.
- Post-change verification must confirm owner and SECURITY DEFINER mode were preserved.
- Post-change verification must not inspect private rows.
- Post-change verification must not list storage objects.
- Post-change verification remains not authorized by this plan.

## 12. Explicit Non-Goals

- Do not execute snapshot queries in this plan.
- Do not connect to production in this plan.
- Do not run Supabase CLI.
- Do not apply the local migration.
- Do not execute production SQL.
- Do not mutate production.
- Do not deploy.
- Do not inspect private rows.
- Do not list storage objects.
- Do not print function bodies.
- Do not print secrets.
- Do not fix broad grants in Phase 1.
- Do not claim production safe.
- Do not claim security hardened.
- Do not claim function grants fixed.

## 13. Decision

- Pre-change production snapshot plan is prepared for owner review.
- Snapshot execution remains not authorized.
- Production execution remains not authorized.
- Supabase CLI remains not authorized.
- Next valid gate is owner approval of this pre-change production snapshot plan.

## 14. Required Next Gates

| Next gate | Required before | Status |
| --- | --- | --- |
| Owner approval of pre-change snapshot plan | Any snapshot execution | Required |
| Pre-change snapshot execution prompt | Any snapshot execution | Not authorized by this plan |
| Pre-change snapshot execution report | Any production mutation | Required after approved snapshot execution |
| Rollback readiness confirmation | Any production execution | Required |
| Production execution prompt | Any production mutation | Not authorized |
| Production execution | Any production mutation | Not authorized |
| Post-change verification | Any production safety claim | Not executed |

## 15. Risk Position

- Risk remains P0/P1 candidate.
- Snapshot planning reduces production execution uncertainty only.
- Snapshot planning does not reduce production risk by itself.
- Production risk is not reduced until production execution and verification occur.
- Phase 1 does not fix broad grant exposure.
- No exploitability claim is made.
- No production safe/unsafe final claim is made.
- No launch-ready claim is made.
- No security hardened claim is made.
- No function grants fixed claim is made.

## 16. Implementation Authorization Status

- Snapshot execution remains not authorized.
- Production execution remains not authorized.
- Supabase CLI remains not authorized.
- Owner approval of this snapshot plan is required before any snapshot execution prompt.
- No production mutation, deployment action, SQL execution, verification query execution, RPC invocation, private row inspection, storage object listing, grant change, function body change, owner change, or SECURITY DEFINER mode change is authorized by this plan.
- A separate owner-approved snapshot execution prompt is required before any production metadata snapshot action.

## 17. Explicitly Blocked Claims

- Do not claim exploitability.
- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim security hardened.
- Do not claim function grants fixed.
- Do not claim snapshot executed.
- Do not claim production execution authorized.
- Do not claim Supabase CLI executed.
- Do not claim post-change verification executed.
- Do not claim all RPC/function risk is resolved.

## 18. No-Modification Confirmation

- No application code was modified by this handbook task.
- No dashboard/mobile/web code was modified by this handbook task.
- No Supabase tree was modified by this handbook task.
- No SQL or migration was created by this handbook task.
- No executable SQL was written in this handbook file.
- No production connection was made by this handbook task.
- No production mutation was executed.
- Supabase CLI was not run by this handbook task.
- No dashboard action was performed.
- No verification query was executed.
- No RPC/function was invoked by this handbook task.
- No private rows were inspected.
- No storage objects were listed.
- No builds/tests/installs were run.
- No function bodies are included.
- No secret or environment variable value was inspected, copied, printed, rotated, or changed.
- No credentials, hostnames, full project refs, service_role keys, database passwords, connection strings, webhook secrets, API keys, environment variable values, or secrets were included.
- No files were staged or committed by this handbook task.
- Only `08_PatchPlans/SecurityDefinerFunctionGrantPreChangeProductionSnapshotPlan.md` was created/modified.

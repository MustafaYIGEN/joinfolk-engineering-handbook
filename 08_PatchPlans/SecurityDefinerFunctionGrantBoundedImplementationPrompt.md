# Security Definer Function Grant Bounded Implementation Prompt

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: committed production metadata report, production metadata owner review, rollback plan, verification plan, and rollback/verification owner review
- canonical: false
- Prompt status: Bounded implementation prompt draft
- Implementation execution status: Not authorized
- Production mutation status: Not executed
- Legal status: Engineering implementation prompt draft only; not legal advice

## 2. Purpose

- This artifact drafts the first bounded implementation prompt for SecurityDefiner/function grant/proconfig hardening.
- It is intended for review before any source, migration, SQL, grant, proconfig, or production action.
- It does not authorize execution.
- It does not write SQL.
- It does not create migrations.
- It does not change source.
- It does not connect to production.
- It does not invoke RPCs/functions.
- It does not inspect private rows or storage objects.

## 3. Reviewed Gates

- `07_Audits/SecurityDefinerFunctionGrantCollectedMetadataReport.md`
- `00_Status/SecurityDefinerFunctionGrantProductionMetadataOwnerReview.md`
- `08_PatchPlans/SecurityDefinerFunctionGrantRollbackPlan.md`
- `08_PatchPlans/SecurityDefinerFunctionGrantVerificationPlan.md`
- `00_Status/SecurityDefinerFunctionGrantRollbackVerificationOwnerReview.md`
- `08_PatchPlans/SecurityDefinerAndFunctionGrantHardeningPatchPlan.md`
- `09_Decisions/SecurityDefinerAndFunctionGrantHardeningDecision.md`
- `00_Status/StatusIndex.md`
- `08_PatchPlans/PatchPlanIndex.md`

## 4. Bounded Implementation Phase

- Phase: Phase 1
- Phase name: SECURITY DEFINER proconfig/search_path hardening
- Target: metadata-level proconfig/search_path hardening only
- Grant changes: Not included in Phase 1
- Function body changes: Not included in Phase 1
- RLS/storage/Edge changes: Not included in Phase 1
- Production execution: Not authorized by this artifact
- Intended implementation after owner approval: local migration/source preparation only, followed by review

## 5. Phase 1 Function Scope

- `control_cancel_event(event_id uuid)`
- `control_end_event(event_id uuid)`
- `control_open_checkin(event_id uuid)`
- `delete_personal_reminder(p_id uuid)`
- `list_active_reminders()`
- `list_personal_reminders()`
- `publish_event(p_event_id uuid, p_visibility text)`
- `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])`
- `upsert_personal_reminder(p_id uuid, p_title text, p_note text, p_reminder_date date, p_color_theme text, p_remind_before_days integer)`

Final implementation may include all nine functions only if exact signatures still match the pre-change snapshot.

Any missing or changed signature must be excluded and escalated.

No function body text may be included.

No private data may be included.

## 6. Phase 1 Target State

| Target item | Phase 1 decision | Notes |
| --- | --- | --- |
| SECURITY DEFINER/INVOKER mode | Preserve existing SECURITY DEFINER state | Do not change security mode in Phase 1 |
| Function owner | Preserve existing owner | Do not change owner in Phase 1 |
| Function body | Preserve existing body | No function body replacement rewrite in Phase 1 |
| proconfig/search_path | Set fixed function-level search_path target | Use stable explicit search_path standard selected for implementation prompt review |
| row_security proconfig | Preserve current absence unless separately owner-approved | Do not add row_security setting in Phase 1 |
| anon EXECUTE grants | Preserve current state | No grant/revoke in Phase 1 |
| authenticated EXECUTE grants | Preserve current state | No grant/revoke in Phase 1 |
| service_role EXECUTE grants | Preserve current state | No grant/revoke in Phase 1 |
| PUBLIC EXECUTE grants | Preserve current state | No grant/revoke in Phase 1 |
| RLS/storage/Edge | Out of scope | No changes |
| Production execution | Out of scope | No execution authorized |

## 7. Exact Search Path Standard For Review

- Proposed Phase 1 target search_path standard: fixed function-level search_path using `public, extensions`.
- Rationale: provides explicit non-mutable search path while minimizing risk for common Supabase functions that may depend on public tables and extension functions.
- This standard must be reviewed before implementation.
- If owner rejects this standard, implementation prompt must be revised before any migration/source work.
- This artifact does not write executable SQL.

## 8. Required Pre-Change Snapshot

Before any future implementation execution, the approved implementation flow must capture sanitized pre-change metadata for all in-scope functions:

- exact function signatures.
- owner role metadata.
- SECURITY DEFINER/INVOKER state.
- proconfig/search_path state.
- row_security proconfig state.
- effective execute matrix.
- explicit grant matrix.
- migration/patch identifier once available.

No function body text.

No private rows.

No storage object names or paths.

No secrets.

No production identifiers beyond sanitized schema/function/role metadata.

## 9. Required Future Implementation Prompt Constraints

| Constraint | Required? | Notes |
| --- | --- | --- |
| Local source/migration preparation only | Yes | First execution prompt may create a local migration file only after owner approval |
| Production execution | No | Must remain blocked unless separately approved |
| Executable SQL in this artifact | No | This artifact is prompt planning only |
| Function body rewrite | No | Out of Phase 1 |
| Grant/revoke changes | No | Out of Phase 1 |
| Owner/security mode changes | No | Out of Phase 1 |
| RLS/storage/Edge changes | No | Out of Phase 1 |
| Supabase CLI | No | Out of Phase 1 |
| Private row inspection | No | Prohibited |
| Storage object listing | No | Prohibited |
| Secrets/credentials | No | Prohibited |

## 10. Required Future Local Migration Preparation Prompt

The future prompt must be written in prose only and without executable SQL.

The future prompt must instruct the implementation agent to:

- work in `[hostos]` only if explicitly approved later.
- create exactly one local Supabase migration file.
- target only the nine listed functions or an owner-approved subset.
- use exact function identity signatures.
- apply only function-level fixed search_path/proconfig hardening.
- preserve existing function bodies.
- preserve existing SECURITY DEFINER state.
- preserve existing owner roles.
- preserve existing grants.
- avoid function body replacement rewrites.
- avoid grant or revoke changes.
- avoid RLS/storage/Edge/dashboard/mobile/web changes.
- avoid production access and Supabase CLI.
- produce diff only for review.
- run no production command.

No SQL is written here.

## 11. Rollback Requirement For Future Implementation

- Any future implementation prompt must include rollback generation or rollback instructions matching `08_PatchPlans/SecurityDefinerFunctionGrantRollbackPlan.md`.
- Rollback must restore pre-change grant/proconfig metadata for each changed function.
- Rollback must not broaden grants beyond pre-change snapshot.
- Rollback must not expose function bodies.
- Rollback must not inspect private rows or storage objects.
- Rollback execution is not authorized by this artifact.

## 12. Verification Requirement For Future Implementation

- Any future implementation prompt must include verification instructions matching `08_PatchPlans/SecurityDefinerFunctionGrantVerificationPlan.md`.
- Verification must cover metadata, grants, proconfig/search_path, and affected product smoke scopes.
- Verification must confirm grants were preserved in Phase 1.
- Verification must confirm fixed search_path/proconfig hardening was applied.
- Verification execution is not authorized by this artifact.

## 13. Explicit Non-Goals

- Do not fix broad grants in Phase 1.
- Do not claim function grants fixed.
- Do not change function bodies.
- Do not change RLS policies.
- Do not change storage policies.
- Do not change Edge Functions.
- Do not change app/dashboard/mobile/web code.
- Do not execute production SQL.
- Do not deploy.
- Do not run Supabase CLI.
- Do not inspect private rows.
- Do not list storage objects.

## 14. Decision

- Bounded implementation prompt draft is prepared for owner review.
- Phase 1 proposed scope is proconfig/search_path hardening only.
- Phase 1 excludes grant changes.
- Phase 1 excludes function body changes.
- Phase 1 excludes production execution.
- Implementation execution remains not authorized.
- Next valid gate is owner approval of this bounded implementation prompt.

## 15. Required Next Gates

| Next gate | Required before | Status |
| --- | --- | --- |
| Owner approval of bounded implementation prompt | Any local migration/source preparation | Required |
| Local migration/source preparation prompt | Any local source/migration change | Not authorized by this artifact |
| Pre-change snapshot | Any production change | Required inside approved implementation flow |
| Production execution | Any production mutation | Not authorized |

## 16. Risk Position

- Risk remains P0/P1 candidate.
- Phase 1 would reduce search_path/proconfig risk only after implementation and verification.
- Phase 1 would not fix broad grant exposure.
- No exploitability claim is made.
- No production safe/unsafe final claim is made.
- No launch-ready claim is made.
- No security hardened claim is made.
- No function grants fixed claim is made.

## 17. Implementation Authorization Status

- Implementation execution remains not authorized.
- This artifact only drafts a bounded implementation prompt.
- No source change, SQL, executable SQL, migration, grant change, function change, proconfig change, production mutation, Supabase CLI action, dashboard action, verification query execution, RPC invocation, deployment action, private row inspection, or storage object listing is authorized by this artifact.
- A separate owner-approved execution prompt is required before any local migration/source preparation.

## 18. Explicitly Blocked Claims

- Do not claim exploitability.
- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim security hardened.
- Do not claim function grants fixed.
- Do not claim implementation executed.
- Do not claim production execution authorized.
- Do not claim all RPC/function risk is resolved.

## 19. No-Modification Confirmation

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
- Only `08_PatchPlans/SecurityDefinerFunctionGrantBoundedImplementationPrompt.md` was created/modified.

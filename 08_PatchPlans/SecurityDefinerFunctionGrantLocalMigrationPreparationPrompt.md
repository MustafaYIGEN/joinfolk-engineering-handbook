# Security Definer Function Grant Local Migration Preparation Prompt

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: committed bounded implementation prompt owner approval and prior gates
- canonical: false
- Prompt status: Local migration preparation prompt draft
- Local migration execution status: Not authorized
- Production mutation status: Not executed
- Legal status: Engineering prompt draft only; not legal advice

## 2. Purpose

- This artifact drafts the future execution prompt for preparing one local migration file in `[hostos]`.
- It is intended for owner review before any local migration/source change.
- It does not authorize local migration creation.
- It does not write SQL.
- It does not create migrations.
- It does not connect to production.
- It does not run Supabase CLI.
- It does not invoke RPCs/functions.
- It does not inspect private rows or storage objects.

## 3. Reviewed Artifacts

- `00_Status/SecurityDefinerFunctionGrantBoundedImplementationPromptOwnerApproval.md`
- `08_PatchPlans/SecurityDefinerFunctionGrantBoundedImplementationPrompt.md`
- `00_Status/SecurityDefinerFunctionGrantRollbackVerificationOwnerReview.md`
- `08_PatchPlans/SecurityDefinerFunctionGrantRollbackPlan.md`
- `08_PatchPlans/SecurityDefinerFunctionGrantVerificationPlan.md`
- `07_Audits/SecurityDefinerFunctionGrantCollectedMetadataReport.md`
- `00_Status/StatusIndex.md`
- `08_PatchPlans/PatchPlanIndex.md`

## 4. Approved Future Execution Target

- Repository target after later owner approval: `[hostos]`.
- File target after later owner approval: exactly one local Supabase migration file.
- Phase: Phase 1.
- Phase name: SECURITY DEFINER proconfig/search_path hardening.
- Search path target for review: fixed function-level search_path using `public, extensions`.
- Grant changes: Not allowed.
- Function body rewrites: Not allowed.
- Owner changes: Not allowed.
- SECURITY DEFINER mode changes: Not allowed.
- RLS/storage/Edge changes: Not allowed.
- Production execution: Not allowed.
- Supabase CLI: Not allowed.

## 5. Future Migration Function Scope

- `control_cancel_event(event_id uuid)`
- `control_end_event(event_id uuid)`
- `control_open_checkin(event_id uuid)`
- `delete_personal_reminder(p_id uuid)`
- `list_active_reminders()`
- `list_personal_reminders()`
- `publish_event(p_event_id uuid, p_visibility text)`
- `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])`
- `upsert_personal_reminder(p_id uuid, p_title text, p_note text, p_reminder_date date, p_color_theme text, p_remind_before_days integer)`

Future execution must target only exact matching signatures.

Missing or changed signatures must be excluded and escalated.

No function body text may be included.

No private data may be included.

## 6. Future Execution Prompt Requirements

| Requirement | Required? | Notes |
| --- | --- | --- |
| Work in `[hostos]` only | Yes | Only after owner-approved execution prompt |
| Create exactly one migration file | Yes | Local file only |
| Use exact function identity signatures | Yes | No overload ambiguity |
| Apply only function-level search_path/proconfig hardening | Yes | Phase 1 only |
| Preserve existing grants | Yes | No grant/revoke changes |
| Preserve function bodies | Yes | No body rewrite |
| Preserve owners | Yes | No owner changes |
| Preserve SECURITY DEFINER mode | Yes | No security mode changes |
| Avoid RLS/storage/Edge changes | Yes | Out of scope |
| Avoid app/dashboard/mobile/web changes | Yes | Out of scope |
| Avoid production access | Yes | No production connection |
| Avoid Supabase CLI | Yes | Not allowed |
| Produce diff only | Yes | Review before commit |
| Run no production command | Yes | Production execution blocked |

## 7. Future Execution Prompt Draft

The future execution prompt must be prose only.

It must instruct the implementation agent to:

- work only in `C:\dev\hostos` after explicit owner approval.
- create exactly one new migration file under `[hostos]/supabase/migrations`.
- name the migration with a timestamped, descriptive filename.
- target only the exact nine functions or an explicitly owner-approved subset.
- use exact function identity signatures.
- add only fixed function-level search_path/proconfig hardening.
- preserve all existing EXECUTE grants.
- preserve all function bodies.
- preserve all function owners.
- preserve SECURITY DEFINER state.
- avoid function body replacement rewrites.
- avoid grant/revoke statements.
- avoid RLS/storage/Edge/dashboard/mobile/web changes.
- avoid production access.
- avoid Supabase CLI.
- avoid builds/tests/installs.
- produce only diff and status for review.
- do not stage.
- do not commit.

No SQL is written here.

## 8. Rollback Requirement

- The later execution prompt must require rollback planning matching `08_PatchPlans/SecurityDefinerFunctionGrantRollbackPlan.md`.
- Rollback must restore pre-change grant/proconfig metadata for each changed function.
- Rollback must not broaden grants beyond pre-change snapshot.
- Rollback must not expose function bodies.
- Rollback execution remains not authorized by this artifact.

## 9. Verification Requirement

- The later execution prompt must require verification planning matching `08_PatchPlans/SecurityDefinerFunctionGrantVerificationPlan.md`.
- Verification must confirm fixed search_path/proconfig hardening.
- Verification must confirm grants were preserved.
- Verification must not inspect private rows or list storage objects.
- Verification execution remains not authorized by this artifact.

## 10. Explicit Non-Goals

- Do not create a migration in this artifact.
- Do not write executable SQL in this artifact.
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

## 11. Decision

- Local migration preparation prompt draft is prepared for owner review.
- Actual local migration creation remains not authorized.
- Production execution remains not authorized.
- Next valid gate is owner approval of this local migration preparation prompt.

## 12. Required Next Gates

| Next gate | Required before | Status |
| --- | --- | --- |
| Owner approval of local migration preparation prompt | Any local migration/source change | Required |
| Local migration preparation execution prompt | Any local migration/source change | Not authorized by this artifact |
| Pre-change snapshot | Any production change | Required inside approved implementation flow |
| Production execution | Any production mutation | Not authorized |

## 13. Risk Position

- Risk remains P0/P1 candidate.
- This prompt draft reduces planning uncertainty only.
- It does not reduce production risk by itself.
- Phase 1 will not fix broad grant exposure.
- No exploitability claim is made.
- No production safe/unsafe final claim is made.
- No launch-ready claim is made.
- No security hardened claim is made.
- No function grants fixed claim is made.

## 14. Implementation Authorization Status

- Local migration creation remains not authorized.
- Implementation execution remains not authorized.
- This artifact only drafts the local migration preparation prompt.
- No source change, SQL, executable SQL, migration, grant change, function change, proconfig change, production mutation, Supabase CLI action, dashboard action, verification query execution, RPC invocation, deployment action, private row inspection, or storage object listing is authorized by this artifact.
- A separate owner-approved execution prompt is required before any local migration/source preparation.

## 15. Explicitly Blocked Claims

- Do not claim exploitability.
- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim security hardened.
- Do not claim function grants fixed.
- Do not claim local migration created.
- Do not claim implementation executed.
- Do not claim production execution authorized.
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
- Only `08_PatchPlans/SecurityDefinerFunctionGrantLocalMigrationPreparationPrompt.md` was created/modified.

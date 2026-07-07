# Governance Index

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: committed governance documents
- canonical: false
- Governance status: Proposed index
- Implementation status: Not authorized
- Production mutation status: Not executed
- Legal status: Engineering governance only; not legal advice

## 2. Purpose

This index maps the governance documents that control handbook lifecycle, patch policy, do-not-touch boundaries, change control, spec status, and repository topology.

This is not implementation.

This does not authorize source changes.

This does not authorize SQL.

This does not authorize migration creation.

This does not authorize production access.

This does not authorize Supabase CLI.

## 3. Governance Document Map

| Document | Role | Primary use | Authority level | Notes |
|---|---|---|---|---|
| `00_Governance/ChangeControl.md` | Change control policy | Defines how changes are proposed, reviewed, verified, and recorded | Governance policy | Prevents random direct changes. |
| `00_Governance/DoNotTouchPolicy.md` | Protected-area policy | Defines areas requiring explicit approval before modification | Governance policy | Applies to dangerous/security-sensitive areas. |
| `00_Governance/HandbookLifecycle.md` | Handbook lifecycle policy | Defines document lifecycle and canonical spec handling | Governance policy | Controls how accepted specs evolve. |
| `00_Governance/PatchPolicy.md` | Patch execution policy | Defines patch categories, approval, build/test, manual verification, rollback | Governance policy | Controls implementation patch discipline. |
| `00_Governance/RepositoryTopologyAndSourceOfTruth.md` | Repository/source-of-truth map | Defines [handbook], [hostos], [joinfolk-web], backup, tooling, and temp roots | Governance policy | Prevents local/prod/source confusion. |
| `00_Governance/SpecStatusModel.md` | Spec status policy | Defines status/version/source confidence/canonical field handling | Governance policy | Controls how documents should be interpreted. |

## 4. Reading Order

Recommended reading order:

1. `SpecStatusModel.md`
2. `HandbookLifecycle.md`
3. `RepositoryTopologyAndSourceOfTruth.md`
4. `ChangeControl.md`
5. `DoNotTouchPolicy.md`
6. `PatchPolicy.md`

Why:

- Status model tells how to interpret documents.
- Lifecycle tells how documents evolve.
- Topology tells which repo/root is source-of-truth.
- Change control tells how work is proposed.
- Do-not-touch defines protected boundaries.
- Patch policy defines implementation discipline.

## 5. Relationship to Handbook Workflow

Governance workflow:

Governance policy
-> audit evidence
-> decision record
-> patch plan
-> status gate
-> implementation only after explicit approval
-> verification report
-> release/readiness status

Governance docs do not themselves authorize implementation.

Governance docs define the rules under which later work may be approved.

## 6. Relationship to Repository Topology

`RepositoryTopologyAndSourceOfTruth.md` is the source-of-truth for aliases and evidence boundaries.

Future handbook docs should use `[handbook]`, `[hostos]`, `[joinfolk-web]`, `[joinfolk-web-backups]` aliases instead of absolute local paths.

Local source evidence must not be treated as production proof.

Production claims require explicit production evidence.

## 7. Relationship to SecurityDefiner Workstream

The current SecurityDefiner/function grant hardening workstream depends on governance rules from:

- `ChangeControl.md`
- `DoNotTouchPolicy.md`
- `PatchPolicy.md`
- `RepositoryTopologyAndSourceOfTruth.md`
- `SpecStatusModel.md`

Current chain:

- `09_Decisions/SecurityDefinerAndFunctionGrantHardeningDecision.md`
- `08_PatchPlans/SecurityDefinerAndFunctionGrantHardeningPatchPlan.md`
- `00_Status/SecurityDefinerFunctionGrantHardeningOwnerReviewGate.md`
- `07_Audits/SecurityDefinerFunctionGrantInventoryClassification.md`
- `00_Status/SecurityDefinerFunctionGrantClassificationCompletenessReview.md`
- `08_PatchPlans/SecurityDefinerFunctionGrantMetadataCollectionPlan.md`
- `00_Status/SecurityDefinerFunctionGrantMetadataCollectionApprovalGate.md`
- `07_Audits/SecurityDefinerFunctionGrantCollectedMetadataReport.md`

## 8. Relationship to Future Feature Runtime Audits

Future runtime/feature audits, including Memory Wall city-specific behavior, must use governance rules to distinguish:

- Expected product behavior.
- Local source evidence.
- Runtime/manual test evidence.
- Production metadata evidence.
- Implementation authorization.

Feature behavior must not be marked fixed or production-correct from local source evidence alone.

Runtime behavior requires test evidence.

## 9. Non-Goals

- No implementation.
- No source modification.
- No SQL.
- No migration.
- No production access.
- No production mutation.
- No Supabase CLI.
- No verification query.
- No RPC/function invocation.
- No launch-ready claim.

## 10. Implementation Authorization Status

Implementation remains not authorized.

No source change, SQL, migration, production mutation, Supabase CLI action, dashboard action, verification query, RPC invocation, metadata collection execution, or deployment action is authorized by this governance index.

A separate owner-approved implementation prompt is required for any app/dashboard/mobile/web/backend/Supabase source modification.

## 11. Explicitly Blocked Claims

- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim security hardened.
- Do not claim repository parity with production.
- Do not claim backups are canonical.
- Do not claim local source evidence proves production behavior.
- Do not claim implementation authorized.

## 12. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- No production mutation was executed.
- Supabase CLI was not run.
- No dashboard action was performed.
- No verification query was executed.
- No RPC/function was invoked.
- No private rows were inspected.
- No storage objects were listed.
- No builds/tests/installs were run.
- No secret or environment variable value was inspected, copied, printed, rotated, or changed.
- No credentials, hostnames, full project refs, service_role keys, database passwords, connection strings, webhook secrets, API keys, environment variable values, or secrets were included.
- No files were staged or committed.
- Only `00_Governance/GovernanceIndex.md` was created/modified.

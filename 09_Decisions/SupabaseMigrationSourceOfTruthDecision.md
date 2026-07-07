# Supabase Migration Source of Truth Decision

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: PP-01 metadata evidence and prior decision records
- canonical: false
- Decision status: Proposed; not implemented
- Source-of-truth status: Proposed; incomplete until sanitized migration provenance evidence is reviewed
- Implementation status: Not authorized
- Production mutation status: Not executed
- Legal status: Engineering release-governance decision only; not legal advice

## 2. Purpose

This record defines how Supabase migration provenance, production database drift, local migration history, dashboard/manual changes, generated SQL, rollback posture, and release authority should be classified before any SQL, migration, patch plan, source implementation, Supabase CLI action, dashboard action, or production change is authorized.

This is not implementation. It does not authorize SQL, migrations, production access, Supabase CLI actions, dashboard actions, source changes, verification queries, or production mutation.

## 3. Evidence Boundary

This document is based only on sanitized PP-01 metadata evidence and committed handbook reports/decisions.

No new production access, SQL, CLI, dashboard action, source inspection, migration inspection, private data inspection, storage object listing, storage object download, build, test, dependency install, deployment, or implementation was performed.

No credentials, hostnames, full project refs, connection strings, service_role keys, anon keys, JWT secrets, tokens, webhook secrets, API keys, environment variable values, private row values, storage object names, storage object paths, message bodies, auth user rows, tickets/orders/reservations/claims rows, diagnostics payloads, notification payloads, support notes, or payment payloads are included.

## 4. Decision Status

Decision status: Proposed / Draft.

Migration source-of-truth model is defined.

Actual migration-by-migration and production-drift classification is not complete until sanitized evidence is reviewed.

Implementation is not authorized.

This document must be reviewed before any SQL/migration patch plan or implementation prompt is issued.

## 5. PP-01 Evidence Summary

PP-01 metadata evidence and follow-up decisions reported:

- Verifier direct table/storage access was closed.
- Verifier role membership rows = 0.
- Verifier effective table privilege rows for public/auth/storage/realtime = 0.
- storage_schema_usage = false.
- storage_objects_select = false.
- No private rows inspected.
- No storage objects listed.
- No functions invoked.
- No mutation executed.
- Verifier access revoked / NOLOGIN confirmed.
- Migration provenance unresolved.
- supabase_migrations was not observed / not verified within the evidence boundary.
- No production schema mutation was executed.
- No implementation is authorized.
- No production safe/unsafe final claim exists.

## 6. Problem Statement

Production database state may not be fully represented by local migration files.

Local migrations may include abandoned, superseded, partial, or manually edited history.

Supabase Dashboard/manual SQL changes may exist outside repo migrations.

Generated SQL may not equal applied production state.

Production drift can invalidate patch plans, rollback assumptions, and security claims.

Migration provenance must be resolved before launch-ready claims or destructive/security-sensitive patches.

Metadata evidence does not prove exploitability.

## 7. Decision Principles

- Classify before patching.
- Production database state is the operational reality, but not automatically the canonical source for future changes.
- Repo migration history should become the canonical forward-change source only after reconciliation.
- Manual dashboard changes must be either captured, documented, or explicitly deprecated.
- No migration should be treated as safe merely because it exists locally.
- No production object should be changed without knowing whether it is migration-managed, dashboard-created, platform-managed, or unknown.
- Rollback must be designed before applying future SQL.
- Supabase-managed schemas must not be modified without separate explicit decision.
- No production-safe or launch-ready claim until provenance and drift are classified.

## 8. Source-of-Truth Candidate Model

| Candidate source | Meaning | Default decision posture |
|---|---|---|
| Production database state | Current deployed database objects and configuration. | Operational reality for verification, but not automatically canonical for future changes. |
| Local Supabase migration files | Repo-tracked migration history and intended forward changes. | Candidate canonical source after reconciliation with production evidence. |
| Supabase migration tracking table | Production-applied migration tracking metadata if available. | Evidence source only after sanitized verification; does not prove runtime safety. |
| Dashboard/manual SQL history | Operator or dashboard-applied changes outside repo migrations. | Must be captured, documented, deprecated, or reconciled before sensitive patches. |
| Generated schema dump | Snapshot of schema generated from an environment. | Useful evidence but not a change authority by itself. |
| Application source expectations | App, dashboard, backend, or function code assumptions about schema and policies. | Dependency evidence only; does not prove production state. |
| Handbook decision records | Planning and decision artifacts in this repository. | Governance input; not implementation or production proof. |
| Platform-managed Supabase state | Supabase-managed auth/storage/realtime/platform objects. | Treat as vendor/platform boundary requiring separate decision before mutation. |
| Unknown/unattributed state | Object or behavior whose origin is not known. | Blocking provenance item for P0/P1 launch-readiness claims. |

## 9. Migration Provenance Classification Model

| Class | Description | Default decision posture |
|---|---|---|
| Applied and represented in repo | Migration appears applied and exists in repo history. | Candidate accepted provenance after sanitized verification and owner review. |
| Applied but missing from repo | Production object/change appears applied but no repo migration is identified. | Requires capture, documentation, or reconciliation before related patches. |
| In repo but not verified applied | Local migration exists but production application is not verified. | Do not rely on it for production claims until verified. |
| Superseded migration | Migration intent was replaced by later changes. | Document supersession and avoid using it as current-state proof. |
| Failed/partial migration | Migration may have applied incompletely or inconsistently. | High-risk provenance item requiring separate owner-approved investigation. |
| Manual dashboard change | Change made outside migration workflow. | Must be documented, codified, deprecated, or explicitly accepted. |
| Generated/temporary SQL | SQL generated for inspection, diffing, or one-off use. | Not a source of truth unless reviewed and adopted. |
| Platform-managed change | Change applied by Supabase or managed platform internals. | Do not modify without separate explicit platform/schema decision. |
| Unknown provenance | Origin of object/change is not known. | Blocking item until classified or explicitly deferred by owner. |

## 10. Production Drift Classification Model

| Drift class | Description | Default decision posture |
|---|---|---|
| No known drift | No difference is currently known within available evidence. | Not proof of parity; continue controlled verification. |
| Benign documented drift | Difference is documented and accepted as non-security-critical. | May be accepted with owner sign-off and future cleanup plan if needed. |
| Security-relevant drift | Difference affects RLS, grants, auth, RPCs, storage, or privileged paths. | Treat as P0/P1 until classified and patched or accepted. |
| Runtime behavior drift | Production behavior differs from source or handbook expectation. | Requires behavior verification and scoped patch/readiness decision. |
| Schema-only drift | Table/column/view/function shape differs without known behavior impact. | Requires dependency mapping before patching. |
| Policy/grant drift | RLS policy or privilege differs from intended state. | Requires RLS/grant matrix decision and rollback plan. |
| Function/RPC drift | Function body, grant, proconfig, or security mode differs. | Requires function hardening decision and controlled implementation path. |
| Storage/Edge-related drift | Bucket, storage policy, Edge deployment, or function config differs. | Requires storage/Edge decision cross-reference. |
| Legacy/backup drift | Backup, snapshot, or deprecated object exists outside intended model. | Requires triage before launch-ready claims. |
| Unknown drift | Drift has not been classified. | Blocking item for P0/P1 surfaces. |

## 11. Change Authority Model

| Change type | Authority requirement | Default posture |
|---|---|---|
| New migration | Owner-approved implementation scope with migration diff and rollback notes. | Not authorized here. |
| RLS policy change | RLS/grant matrix decision and affected-flow verification plan. | Not authorized here. |
| Grant revoke/grant | Explicit role/grant target matrix and rollback plan. | Not authorized here. |
| SECURITY DEFINER proconfig/function grant change | Function grant/proconfig decision and exact function list. | Not authorized here. |
| Function body change | Source/migration ownership decision and smoke/rollback plan. | Not authorized here. |
| Storage policy/bucket config change | Storage exposure classification and owner approval. | Not authorized here. |
| Edge Function deployment/config change | Edge inventory classification and deployment rollback plan. | Not authorized here. |
| Data backfill/repair | Separate runbook, privacy boundary, rollback/validation plan, and owner approval. | Not authorized here. |
| Drop/remove/destructive action | Separate explicit owner decision and backup/rollback posture. | Not authorized here. |
| Documentation-only acceptance | Owner accepts documented current state without mutation. | May be recorded only after evidence review; not implied here. |

This document authorizes none of these changes.

## 12. Environment Boundary

Local, staging, preview, and production environments must not be conflated.

Production state must be verified using sanitized metadata only unless separately authorized.

Local schema does not prove production schema.

Production schema does not prove intended design.

Environment identifiers, project refs, hostnames, and connection strings must not be written into the handbook.

This decision authorizes no environment access.

## 13. supabase_migrations Boundary

supabase_migrations or equivalent migration tracking evidence was not verified in the current PP-01 boundary.

Future evidence may classify migration IDs/names/timestamps only if sanitized and approved.

Migration tracking evidence must not include secrets or private data.

Absence of observed tracking evidence does not prove migrations are absent.

Presence of tracking evidence does not prove runtime safety.

No migration query is authorized here.

## 14. Rollback and Reversibility Boundary

Future SQL/migration patches must include rollback notes.

Rollback notes must distinguish reversible grants/policies from irreversible data operations.

Destructive drops/removals require separate explicit decision.

Backup/snapshot/legacy relation handling requires owner approval.

Rollback must be tested or reasoned about before release-readiness claims.

This decision creates no rollback SQL.

## 15. Relationship to RLS / Grant Matrix

RLS/grant patch plans depend on knowing whether policies/grants are migration-managed or drifted.

RLSPolicyAndGrantMatrixClassification.md must be cross-referenced.

A grant/policy existing in production does not prove it is intended.

No RLS/grant change is authorized here.

## 16. Relationship to SECURITY DEFINER / Function Grants

Function grant/proconfig hardening must be implemented through a controlled source-of-truth path.

SECURITY DEFINER function changes may be high-risk if production function body differs from local expectations.

SecurityDefinerAndFunctionGrantHardeningDecision.md must be cross-referenced.

No function or grant change is authorized here.

## 17. Relationship to Storage / Edge / Realtime

Storage and Edge Function posture may depend on migrations, dashboard config, and source deployment history.

StorageBucketExposureDecision.md and EdgeFunctionDeploymentInventoryDecision.md must be cross-referenced.

Realtime behavior may depend on publication/table/policy configuration.

No storage, Edge, or realtime change is authorized here.

## 18. Proposed Target State

- Production schema provenance is classified.
- Local migration history is reconciled with production tracking evidence.
- Dashboard/manual changes are either captured, documented, or explicitly deprecated.
- Future SQL changes flow through reviewed migrations or explicitly approved operational runbooks.
- Every security-sensitive patch has rollback notes.
- Every destructive action has separate owner approval.
- No Unknown P0/P1 migration/drift classification remains before launch-ready claim.

## 19. Required Future Evidence

Required future evidence includes:

- Sanitized migration file inventory.
- Sanitized production migration tracking inventory if approved.
- Sanitized schema object inventory.
- Object ownership/provenance classification.
- RLS/policy/grant/function provenance classification.
- Storage/Edge/realtime configuration provenance classification where applicable.
- Manual/dashboard change evidence if available.
- App/dashboard dependency mapping where available.
- No private rows.
- No secrets.
- No hostnames/project refs/connection strings.
- No payload bodies.

## 20. Required Future Implementation Scope

No implementation is authorized yet.

Future implementation may include migration reconciliation, documentation-only acceptance, creation of a patch migration, rollback runbook, drift capture, dashboard/manual-change codification, source-code patch, or deprecated-object removal.

Exact action depends on classification and owner approval.

Destructive actions require separate explicit decision.

Implementation must be split into scoped patches.

## 21. Required Verification Scope

Future verification scope should include:

- Diff review.
- Migration diff review if SQL is later created.
- No secret inspection.
- No private data inspection.
- Metadata-only verification where possible.
- Production migration tracking verification only after explicit approval.
- Local-vs-production schema comparison only with sanitized metadata.
- Affected app/dashboard smoke tests after implementation approval.
- Negative permission tests after security patches.
- Rollback verification.

## 22. Rollback / Safety Requirements

- Every future SQL/migration/config change must have rollback notes.
- Migrations must be reversible where feasible.
- Irreversible steps must be isolated and separately approved.
- High-risk security patches must be small and staged.
- No drop/remove without separate decision.
- No data repair/backfill without separate runbook.
- No Supabase-managed schema mutation without separate explicit decision.
- Failed verification blocks release-readiness claims.

## 23. Explicit Non-Goals

- No SQL.
- No migration.
- No source changes.
- No production mutation.
- No Supabase CLI.
- No dashboard action.
- No migration execution.
- No migration rollback.
- No schema dump.
- No production diff query.
- No secret inspection.
- No data repair.
- No relation drop.
- No grant/RLS/function/storage/Edge change.
- No launch approval.
- No legal/compliance claim.

## 24. Risks and Open Questions

- Production may contain manual changes not represented in repo.
- Repo migrations may contain obsolete or superseded intent.
- Migration tracking evidence may be unavailable or incomplete.
- Dashboard-created objects may lack rollback history.
- Security patches may fail if based on stale local assumptions.
- Rollback may be incomplete for destructive or data-changing operations.
- Storage/Edge/realtime configuration may have separate provenance.
- App behavior may depend on drifted schema or policies.
- Runtime behavior is not verified by metadata alone.

## 25. Follow-Up Artifacts

- `SecurityDefinerAndFunctionGrantHardeningPatchPlan.md` only after function classification and owner approval.
- `RLSGrantMatrixPatchPlan.md` only after matrix classification and owner approval.
- `StorageBucketExposurePatchPlan.md` only after bucket/policy classification and owner approval.
- `EdgeFunctionDeploymentPatchPlan.md` only after deployment inventory classification and owner approval.
- `JoinFolkReleaseCandidateReadinessReport.md` only after implementation and verification gates are complete.

## 26. Implementation Authorization Status

Implementation remains not authorized.

No SQL, migration, source change, Supabase CLI action, dashboard action, migration execution, migration rollback, schema dump, production diff query, grant change, RLS change, function change, storage change, Edge Function change, data repair, relation drop, production mutation, or verification query is authorized by this decision.

## 27. Explicitly Blocked Claims

- Do not claim exploitability.
- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim legal compliance.
- Do not claim migrations reconciled.
- Do not claim production drift resolved.
- Do not claim source of truth established.
- Do not claim implementation authorized.
- Do not claim all migration/provenance risk is resolved.

## 28. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No database role was created.
- No production connection was made.
- No production mutation was executed.
- Supabase CLI was not run.
- No dashboard action was performed.
- No migration was executed or rolled back.
- No schema dump or production diff query was performed.
- No builds/tests/installs were run.
- No Edge Function was deployed, updated, deleted, invoked, or inspected.
- No secret or environment variable value was inspected, copied, printed, rotated, or changed.
- No storage object was listed, downloaded, uploaded, modified, or deleted.
- No signed URL was generated.
- No credentials, hostnames, full project refs, service_role keys, database passwords, connection strings, webhook secrets, API keys, environment variable values, or secrets were included.
- No private rows, storage objects, object paths, messages, tickets, orders, diagnostics, reports, support notes, auth users, or payment payloads were inspected.
- No application RPC/function was invoked.
- No implementation/admin/support/storage/media/messaging/deletion/export/refund/payment/moderation/RLS/RPC/storage/realtime/Edge/notification/commerce action was executed.
- No files were staged or committed.
- Only 09_Decisions/SupabaseMigrationSourceOfTruthDecision.md was created/modified.

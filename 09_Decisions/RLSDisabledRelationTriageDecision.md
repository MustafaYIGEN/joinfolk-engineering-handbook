# RLS Disabled Relation Triage Decision

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: PP-01 metadata evidence and classification report
- canonical: false
- Decision status: Proposed; not implemented
- Implementation status: Not authorized
- Production mutation status: Not executed
- Legal status: Engineering security decision only; not legal advice

## 2. Purpose

This record defines how RLS-disabled relations should be classified and triaged before any SQL, migration, grant change, or source implementation is authorized.

It does not implement anything.

## 3. Evidence Boundary

This decision is based only on sanitized PP-01 metadata evidence and existing handbook reports.

No new production access, SQL, CLI, dashboard action, source inspection, private data inspection, build, test, dependency install, migration, or implementation was performed.

No secrets or private data are included.

## 4. Decision Status

Decision status: Proposed / Draft.

Implementation is not authorized.

This record must be reviewed before any RLS/grant patch prompt is issued.

## 5. PP-01 Evidence Summary

PP-01 metadata evidence and classification reported:

- Verifier direct table/storage access was closed.
- Role membership rows = 0.
- Effective table privilege rows for verifier in public/auth/storage/realtime = 0.
- Storage schema usage/select false.
- No private rows inspected.
- No storage objects listed.
- No mutation executed.
- Verifier access revoked.
- RLS-disabled backup/legacy/view relations still require triage.
- Broad app-facing table privileges still require matrix classification.

## 6. Problem Statement

RLS-disabled relation metadata can represent very different risk categories.

Some relations may be views, backups, legacy tables, staging artifacts, or intentionally internal relations.

Some may be app-owned production tables where RLS-disabled is unacceptable.

Metadata alone does not prove exploitability.

It is not acceptable to leave the class unknown before launch-readiness claims.

## 7. Decision Principles

- Classify before patching.
- App-owned production user-data tables should default to RLS enabled unless explicitly justified.
- Legacy/backup/staging artifacts should not remain ambiguous.
- Views require separate handling because view security semantics differ from base table RLS.
- Supabase-managed schemas must not be altered without separate explicit decision.
- No destructive action without owner review.
- No production-safe claim until verification.

## 8. RLS Disabled Relation Classification Model

| Class | Description | Default decision posture |
|---|---|---|
| App-owned production user-data table | Active table containing user, event, commerce, media, messaging, notification, profile, staff, reservation, ticket, claim, diagnostics, moderation, or payment-adjacent data. | Must not remain RLS-disabled without explicit documented justification and owner approval. |
| App-owned production lookup/reference table | Active app table intended to expose non-sensitive reference values. | May remain RLS-disabled only if public-safe exposure is documented and grants are intentional. |
| Internal/admin-only table | Table intended only for operator, admin, support, audit, maintenance, or service workflows. | Should not be broadly exposed; access posture must be explicitly documented. |
| View or materialized view | Relation that derives data from one or more base relations. | Requires view-specific exposure classification; do not assume base table RLS semantics are sufficient. |
| Legacy/deprecated table | Former application relation no longer intended for active use. | Classify dependency and decide retain/restrict/archive/remove separately. |
| Backup/snapshot table | Backup, duplicate, phase, or snapshot relation retained from a past repair or migration. | Should not remain ambiguous; decide retention and access posture separately. |
| Migration/staging/temp artifact | Relation created for migration, staging, temporary processing, or transitional work. | Remove/restrict/archive only after owner decision; document if retained. |
| Supabase-managed/platform relation | Relation in platform-managed auth/storage/realtime or similar schemas. | Do not alter under this decision; requires separate vendor/schema decision. |
| Unknown relation | Relation whose ownership, purpose, or sensitivity is not yet classified. | Treat as blocking triage item before launch-ready claim or related implementation. |

## 9. App-Owned Production Table Standard

App-owned production tables containing user, event, commerce, media, messaging, notification, profile, staff, reservation, ticket, claim, diagnostics, moderation, or payment-adjacent data should not remain RLS-disabled without explicit documented justification.

This standard is a decision target, not an implementation step.

## 10. View / Materialized View Handling

Views are not automatically equivalent to base tables.

Future triage must determine whether each view exposes sensitive data.

Future implementation must check whether security_barrier/security_invoker or equivalent behavior is relevant.

No view behavior claim is made by this decision.

## 11. Legacy / Backup / Snapshot Handling

Legacy/backup/snapshot relations must be classified.

If not needed, removal/archive/restrict decisions require separate owner approval.

No drop/remove is authorized here.

If retained, access posture must be explicitly documented.

## 12. Supabase-Managed Schema Boundary

auth/storage/realtime/platform-managed relations are not to be modified under this decision.

Any vendor/platform-managed schema change requires a separate explicit decision.

## 13. Relationship to Grant Matrix

RLS state alone is not enough.

Table grants, role privileges, policies, SECURITY DEFINER RPCs, and storage policies must be evaluated together.

This decision feeds into `RLSPolicyAndGrantMatrixClassification.md`.

## 14. Proposed Target State

- Every RLS-disabled relation has a class.
- Every app-owned production user-data table has explicit RLS posture.
- Every legacy/backup/staging relation has explicit retain/remove/restrict decision.
- Every view has exposure classification.
- No unknown RLS-disabled relation remains before launch-ready claim.

## 15. Required Future Evidence

- Sanitized relation inventory.
- Schema/name/type only, no private rows.
- Owner/source classification.
- App usage/dependency classification.
- Whether relation is app-owned, platform-managed, view, backup, legacy, staging, or unknown.
- Current RLS state.
- Current grant exposure category.
- Whether relation is used by app/dashboard/RPC/reporting.

## 16. Required Future Implementation Scope

No implementation is authorized yet.

Future implementation may include RLS enablement, grant changes, view replacement/restriction, relation archival/removal, or documentation-only acceptance.

The exact action depends on classification and owner approval.

Destructive actions require separate explicit decision.

## 17. Required Verification Scope

- Diff review.
- Migration diff review if SQL is later created.
- No private data inspection.
- Metadata-only verification where possible.
- App/dashboard smoke tests for affected flows.
- Negative permission tests.
- Rollback verification.

## 18. Rollback / Safety Requirements

- Every future SQL change must have rollback notes.
- RLS/grant changes must be reversible.
- Staged patching is preferred.
- No drop/remove without separate decision.
- Avoid touching Supabase-managed schemas.

## 19. Explicit Non-Goals

- No SQL.
- No migration.
- No source changes.
- No production mutation.
- No RLS enablement.
- No grant revoke.
- No relation drop.
- No launch approval.
- No legal/compliance claim.

## 20. Risks and Open Questions

- Some RLS-disabled relations may be harmless but undocumented.
- Some may be sensitive app-owned surfaces.
- Views may hide risk if base table policies are misunderstood.
- Legacy/backup tables may retain sensitive historical data.
- App dependencies may be unknown without route/RPC usage mapping.
- Broad grants may matter even if verifier direct grants were closed.

## 21. Follow-Up Artifacts

- `RLSPolicyAndGrantMatrixClassification.md`
- `StorageBucketExposureDecision.md`
- `EdgeFunctionDeploymentInventoryDecision.md`
- `SupabaseMigrationSourceOfTruthDecision.md`
- `RLSDisabledRelationPatchPlan.md` only after classification and owner approval.

## 22. Implementation Authorization Status

Implementation remains not authorized.

No SQL, migration, source change, grant change, RLS change, relation drop, production mutation, or verification query is authorized by this decision.

## 23. Explicitly Blocked Claims

- Do not claim exploitability.
- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim legal compliance.
- Do not claim RLS hardened.
- Do not claim implementation authorized.
- Do not claim all relation risk is resolved.

## 24. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No database role was created.
- No production connection was made.
- No production mutation was executed.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No credentials, hostnames, full project refs, service_role keys, database passwords, connection strings, or secrets were included.
- No private rows, storage objects, messages, tickets, orders, diagnostics, reports, support notes, auth users, or payment payloads were inspected.
- No application RPC/function was invoked.
- No implementation/admin/support/storage/media/messaging/deletion/export/refund/payment/moderation/RLS/RPC/storage/realtime/Edge/notification/commerce action was executed.
- No files were staged or committed.
- Only `09_Decisions/RLSDisabledRelationTriageDecision.md` was created/modified.

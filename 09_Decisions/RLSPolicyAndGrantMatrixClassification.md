# RLS Policy and Grant Matrix Classification

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: PP-01 metadata evidence and prior decision records
- canonical: false
- Classification status: Proposed; incomplete until relation inventory is reviewed
- Decision status: Proposed; not implemented
- Implementation status: Not authorized
- Production mutation status: Not executed
- Legal status: Engineering security classification only; not legal advice

## 2. Purpose

This document defines how table grants, RLS state, RLS policies, relation class, caller roles, and application dependency should be classified together before any SQL, migration, grant revoke, RLS enablement, or source implementation is authorized.

This is not implementation. It does not authorize a patch, migration, production change, verification query, or source-code change.

## 3. Evidence Boundary

This document is based only on sanitized PP-01 metadata evidence and committed handbook reports/decisions.

No new production access, SQL, CLI, dashboard action, source inspection, private data inspection, build, test, dependency install, migration, or implementation was performed.

No credentials, hostnames, full project refs, connection strings, service_role keys, anon keys, JWT secrets, tokens, private row values, storage object names, message bodies, auth user rows, tickets/orders/reservations/claims rows, diagnostics payloads, notification payloads, support notes, or payment payloads are included.

## 4. Classification Status

Classification status: Proposed / Draft.

The matrix model is defined.

Actual relation-by-relation classification is not complete until sanitized inventory is reviewed.

Implementation is not authorized.

This document must be reviewed before any RLS/grant patch plan or implementation prompt is issued.

## 5. PP-01 Evidence Summary

PP-01 metadata evidence and follow-up decision records reported:

- Verifier direct table/storage access was closed.
- Verifier role membership rows = 0.
- Verifier effective table privilege rows for public/auth/storage/realtime = 0.
- Storage schema usage/select false for verifier.
- No private rows inspected.
- No storage objects listed.
- No functions invoked.
- No mutation executed.
- Verifier access revoked / NOLOGIN confirmed.
- Broad app-facing table privileges still require classification.
- RLS-disabled relations still require triage.
- Function EXECUTE grants are handled by a separate decision.
- Storage buckets, Edge Functions, migration provenance, and runtime behavior remain unresolved.

## 6. Problem Statement

RLS state alone is not enough to prove safety.

Grants alone are not enough to prove exposure.

RLS policies, table privileges, relation class, SECURITY DEFINER functions, storage policies, and app usage must be evaluated together.

A relation can appear closed to the verifier but still be reachable through app-facing roles or RPCs.

A broad grant may be acceptable only when RLS and data classification justify it.

Metadata evidence does not prove exploitability.

Unknown grant/RLS combinations block launch-ready claims.

## 7. Decision Principles

- Classify before patching.
- Use least privilege by intended caller role.
- Prefer explicit grant over accidental inherited exposure.
- RLS policies must match data sensitivity and app usage.
- App-owned user-data tables default to RLS enabled and explicit policies unless justified.
- Public/anon access must be intentionally public-safe.
- service_role/internal paths must not imply client exposure.
- SECURITY DEFINER bypass paths must be considered separately.
- No destructive action without owner review.
- No production-safe or launch-ready claim until verification.

## 8. Actor / Role Classification Model

| Actor / Role | Intended use | Default classification posture |
|---|---|---|
| anon | Public, unauthenticated app and web access. | Allow only explicitly public-safe reads or constrained writes with documented purpose and owner approval. |
| authenticated | Signed-in user access through app surfaces. | Allow only data-class-appropriate access governed by RLS policies and negative tests. |
| service_role | Internal server-side or platform-level privileged operation. | Treat as internal-only; never infer client exposure from service_role needs. |
| app backend/internal automation | Backend jobs, Edge Functions, or trusted automation. | Require explicit service boundary, auditability where privileged, and no client-equivalent assumption. |
| host/staff/admin/support/operator role implemented inside app logic | Product roles enforced by application, RLS policy, RPC gate, or internal authority checks. | Must map to concrete backend authority; UI role labels alone are not enough. |
| temporary verifier role | Metadata-only PP-01 evidence collection role. | Must have no private table/storage access and should avoid app-owned function execution in future verifier windows. |
| PUBLIC/inherited role exposure | Permissions inherited through broad default role exposure. | Treat as suspect until justified, removed, or isolated. |
| Supabase-managed/platform role | Platform-managed auth/storage/realtime internals. | Do not modify without separate explicit platform/schema decision. |

## 9. Relation Data Sensitivity Classes

| Data class | Examples | Default RLS/grant posture |
|---|---|---|
| Public-safe reference data | Non-sensitive lookup values or public taxonomy. | Public read may be acceptable if documented and intentionally granted. |
| Public event discovery data | Published public event fields, public venue/share metadata. | Public read may be acceptable only for approved public-safe fields and visibility states. |
| Authenticated user-owned data | User profile-private fields, user settings, saved state. | Authenticated self-access only unless separately justified. |
| Participant/host-scoped event data | Event participation, host-managed event operations, attendee-scoped data. | Participant/host/staff scoped policies required. |
| Commerce/ticket/order/payment-adjacent data | Orders, tickets, reservations, claims, payment-adjacent state. | Strict authenticated owner/host/internal boundaries; public access is not acceptable without separate explicit justification. |
| Media/storage metadata | Media rows, uploader metadata, bucket/object references, public media state. | Must align with storage/public URL decisions and media lifecycle classification. |
| Messaging/DM/notification data | Conversations, messages, recipients, notification state, delivery refs. | Not public; participant-scoped and privacy-reviewed policies required. |
| Staff/admin/support/ops data | Support notes, admin action state, operator workflows, staff authority. | Least privilege, reason/audit requirements, and no public/client exposure. |
| Diagnostics/audit/security data | Diagnostics, audit logs, security events, delivery logs. | Internal or tightly scoped support/admin access only with retention/audit decisions. |
| Legacy/backup/staging data | Backup, snapshot, staging, migration, deprecated operational tables. | Must be triaged; default posture is restrict or remove from app-facing exposure after owner decision. |
| Unknown data | Relation purpose or sensitivity not yet classified. | Blocking classification item; no launch-ready claim until resolved for P0/P1 surfaces. |

## 10. Grant Type Classification Model

| Grant pattern | Meaning | Default decision posture |
|---|---|---|
| No app-facing grant | No direct client role table access identified. | May be acceptable if function/internal access is also classified. |
| Explicit anon SELECT | Unauthenticated read path exists. | Accept only for public-safe data and documented visibility contract. |
| Explicit authenticated SELECT | Signed-in users can read subject to RLS. | Require policy classification and negative tests for non-owner/non-participant users. |
| Explicit authenticated INSERT/UPDATE/DELETE | Signed-in users can mutate subject to RLS. | Require ownership/authority policy review, smoke tests, and rollback plan before changes. |
| Broad anon DML | Unauthenticated mutation grant appears broad. | Treat as P0/P1 classification item unless tightly justified by policy and surface design. |
| Broad authenticated DML | Authenticated mutation grant appears broad. | Requires table-by-table RLS/policy matrix and negative tests. |
| service_role-only access | Access expected only through privileged server/internal role. | Ensure no client or public inherited exposure and audit privileged paths where required. |
| PUBLIC/inherited access | Permission appears inherited from broad role defaults. | Default to remove, justify, or isolate after explicit owner decision. |
| Function-mediated access only | Table access is intended through RPC/function boundary. | Cross-reference function grants and SECURITY DEFINER behavior before accepting. |
| Unknown grant exposure | Grant state or effective role exposure is not classified. | Blocking item until sanitized grant inventory is reviewed. |

Exact SQL is not authorized by this classification.

## 11. RLS Policy Classification Model

| Policy class | Description | Default decision posture |
|---|---|---|
| Public read policy | Allows public/anon read for selected rows. | Accept only for approved public-safe data and public visibility states. |
| Authenticated self-access policy | Limits access to current signed-in user identity. | Accept only after identity mapping and negative tests are verified. |
| Participant-scoped policy | Limits access to event/group/conversation participants. | Requires participant membership source and removal/block behavior review. |
| Host-owned policy | Limits access to host/owner-managed resources. | Requires host authority boundary and transfer/delegation review. |
| Staff/scanner/manager policy | Limits access to staff or operational event roles. | Requires staff role determinism and least-privilege review. |
| Admin/support/operator policy | Supports privileged operational access. | Requires PP-08 authority, reason-code, and auditability decisions. |
| Insert-only policy | Allows row creation but not read/update/delete. | Require payload minimization and abuse/rate-limit review where public-facing. |
| Update/delete owner policy | Allows mutation by owner or authorized actor. | Require data sensitivity, concurrency, and restore/rollback expectations. |
| Service/internal-only policy | Intended for server or internal workflows. | Confirm client roles cannot use it and function paths are classified. |
| Missing/disabled RLS policy | No policy or disabled RLS for a relation requiring protection. | Blocking item unless relation is explicitly public-safe or platform-managed. |
| Unknown/ambiguous policy | Policy intent cannot be classified from metadata. | Needs owner and implementation review before patch or launch-ready claim. |

## 12. Combined Matrix Decision Rules

- Public-safe reference table + explicit anon SELECT may be acceptable if documented.
- User-owned data + anon access is not acceptable without separate explicit justification.
- Commerce/payment-adjacent data requires strict authenticated/owner/host/internal boundaries.
- Messaging/notification data must not be public.
- Broad authenticated DML requires policy review and negative tests.
- RLS-disabled app-owned user-data table is blocking unless explicitly justified.
- View exposure must be checked separately.
- SECURITY DEFINER functions can bypass table-level assumptions and must be cross-referenced.

No SQL is authorized by these rules.

## 13. Required Matrix Columns for Future Inventory

The future matrix must include:

- schema
- relation_name
- relation_type
- owner/source classification
- data sensitivity class
- app usage / dependency
- dashboard usage / dependency
- RPC/function dependency
- current RLS state
- current policy class
- anon grant class
- authenticated grant class
- service_role/internal dependency
- PUBLIC/inherited exposure
- verifier exposure
- storage/Edge/realtime dependency if any
- target posture
- risk class: P0/P1/P2/Unknown
- decision: retain/change/restrict/document/remove/later
- owner approval required
- verification required
- rollback notes required

The actual matrix must use sanitized metadata only.

## 14. Relationship to RLS Disabled Relation Triage

RLSDisabledRelationTriageDecision.md classifies RLS-disabled relation types.

This document classifies combined grant/policy/exposure posture.

RLS-disabled triage feeds into this matrix.

This matrix feeds into any future RLS/grant patch plan.

## 15. Relationship to SECURITY DEFINER / Function Grants

SECURITY DEFINER functions and EXECUTE grants are a parallel exposure path.

Table-level RLS/grant posture can be bypassed by SECURITY DEFINER functions if function logic permits it.

Future implementation planning must cross-reference function exposure before claiming a table is protected.

No function patch is authorized here.

## 16. Relationship to Storage / Edge / Realtime

Table RLS/grants do not fully cover storage bucket exposure.

Table RLS/grants do not prove Edge Function deployment safety.

Table RLS/grants do not prove realtime behavior safety.

StorageBucketExposureDecision.md and EdgeFunctionDeploymentInventoryDecision.md remain required.

Runtime realtime behavior remains unresolved.

## 17. Proposed Target State

- Every app-owned relation has a data sensitivity class.
- Every app-owned relation has current grant classification.
- Every app-owned relation has current RLS/policy classification.
- Every app-owned relation has intended target posture.
- Every broad grant has documented justification or patch plan.
- Every RLS-disabled app-owned user-data relation has explicit decision.
- Every PUBLIC/inherited exposure is either removed, justified, or isolated.
- No Unknown P0/P1 security classification remains before launch-ready claim.

## 18. Required Future Evidence

Required future evidence includes:

- Sanitized relation inventory.
- Sanitized grant inventory.
- Sanitized RLS policy inventory.
- Relation type and schema only.
- No private rows.
- No storage object listing.
- No auth user row inspection.
- App/dashboard route or RPC usage mapping where available.
- Classification of SECURITY DEFINER dependencies.
- Runtime smoke/negative tests only after implementation approval.

## 19. Required Future Implementation Scope

No implementation is authorized yet.

Future implementation may include RLS enablement, policy adjustment, grant revoke/grant, function grant adjustment, view restriction, storage policy adjustment, or documentation-only acceptance.

Exact action depends on the classified matrix and owner approval.

Destructive actions require separate explicit decision.

Implementation must be split into scoped patches.

## 20. Required Verification Scope

Future verification scope should include:

- Diff review.
- Migration diff review if SQL is later created.
- No private data inspection.
- Metadata-only verification where possible.
- Anonymous user negative tests.
- Authenticated non-owner negative tests.
- Owner/participant/host/staff positive tests.
- Dashboard smoke tests for affected surfaces.
- RPC smoke tests only after explicit authorization.
- Rollback verification.

## 21. Rollback / Safety Requirements

- Every future SQL/migration change must have rollback notes.
- Grants must be reversible.
- RLS/policy changes must be staged where feasible.
- High-risk relations should be patched in small batches.
- No drop/remove without separate decision.
- Avoid touching Supabase-managed schemas.
- Failed smoke tests block release-readiness claims.

## 22. Explicit Non-Goals

- No SQL.
- No migration.
- No source changes.
- No production mutation.
- No grant revoke.
- No grant creation.
- No RLS enablement.
- No RLS policy creation/change.
- No function change.
- No storage policy change.
- No Edge Function change.
- No launch approval.
- No legal/compliance claim.

## 23. Risks and Open Questions

- Broad grants may be safe only if RLS policies are correct.
- RLS policies may be incomplete or overly broad.
- Views may expose data differently than expected.
- SECURITY DEFINER functions may bypass caller-level assumptions.
- service_role/internal usage may hide app dependency.
- Legacy/backup/staging tables may contain sensitive data.
- App route/RPC usage mapping may be incomplete.
- Runtime behavior is not verified by metadata alone.

## 24. Follow-Up Artifacts

- `StorageBucketExposureDecision.md`
- `EdgeFunctionDeploymentInventoryDecision.md`
- `SupabaseMigrationSourceOfTruthDecision.md`
- `SecurityDefinerAndFunctionGrantHardeningPatchPlan.md` only after function classification and owner approval.
- `RLSGrantMatrixPatchPlan.md` only after this classification is reviewed and owner approval is given.
- `JoinFolkReleaseCandidateReadinessReport.md` only after implementation and verification gates are complete.

## 25. Implementation Authorization Status

Implementation remains not authorized.

No SQL, migration, source change, grant change, RLS change, policy change, function change, storage change, Edge Function change, production mutation, or verification query is authorized by this classification.

## 26. Explicitly Blocked Claims

- Do not claim exploitability.
- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim legal compliance.
- Do not claim RLS hardened.
- Do not claim grants hardened.
- Do not claim implementation authorized.
- Do not claim all table/RLS/grant risk is resolved.

## 27. No-Modification Confirmation

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
- Only 09_Decisions/RLSPolicyAndGrantMatrixClassification.md was created/modified.

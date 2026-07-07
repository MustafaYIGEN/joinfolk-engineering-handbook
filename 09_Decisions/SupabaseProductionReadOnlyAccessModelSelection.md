# Supabase Production Read-Only Access Model Selection

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Owner/operator decision plus handbook synthesis
- canonical: false
- Decision status: Selected for planning; not provisioned
- Selected model: B â€” Temporary Database Read-Only Verifier Role
- Access status: Not provisioned
- Production verification status: Not executed
- Implementation status: Not authorized
- Legal status: Engineering access planning only; not legal advice

## 2. Purpose

This record selects the production read-only access model for future PP-01 metadata evidence collection.

It is not production access execution, not SQL execution, not database role creation, not implementation work, not legal advice, not launch approval, and not patch authorization.

## 3. Decision

Selected model: B â€” Temporary Database Read-Only Verifier Role.

Reason: Model A availability is unknown; Model B is acceptable to owner/operator and gives deterministic database-level privilege control.

Model A remains optional if a true Supabase Read-Only dashboard role is available.

Model C is optional only if a read replica or isolated read context exists and the owner approves it.

No access has been provisioned.

## 4. Decision Rationale

PP-01 needs production metadata evidence before production-dependent implementation decisions can be made.

The prior PP-01 execution report could not collect production evidence because no operator-confirmed read-only access path existed.

Model B allows controlled metadata-only verification without relying on broad dashboard roles or plan-dependent dashboard permissions.

Model B must remain temporary, least-privilege, non-owner, non-service_role, without RLS bypass, without mutation permissions, and without private row inspection.

## 5. Alternatives Considered

| Alternative | Summary | Decision | Reason |
|---|---|---|---|
| A Supabase Read-Only Dashboard Role | Preferred if a true read-only dashboard role is available. | Optional, not selected as primary | Availability is unknown and plan-dependent. |
| B Temporary Database Read-Only Verifier Role | Temporary database-level role for metadata-only evidence collection. | Selected | Provides deterministic privilege control when operator-provisioned correctly. |
| C Read Replica / Isolated Read Context | Metadata-only evidence collection through an isolated read context. | Optional, not selected as primary | Availability is unknown and requires owner approval. |
| Operator-provided sanitized metadata screenshots | Operator supplies sanitized metadata artifacts. | Acceptable fallback for specific evidence | Lower reproducibility, but useful when direct access is not appropriate. |

## 6. Selected Model B Boundary

Model B is bounded by:

- Metadata-only evidence.
- No private rows.
- No user content.
- No storage object listing.
- No RPC/function invocation.
- No mutation.
- No role ownership.
- No service_role.
- No RLS bypass.
- No secrets.
- No implementation.

## 7. Operator Requirements

- Identify exact production project.
- Confirm environment is production.
- Create or arrange a temporary read-only verifier path separately.
- Confirm no service_role.
- Confirm no DML/DDL/mutation permission.
- Confirm no private data access.
- Confirm no storage object listing.
- Confirm expiry/revocation plan.
- Confirm evidence output path.

## 8. Verifier Requirements

- Do not inspect secrets.
- Do not read private rows.
- Do not run mutation SQL.
- Do not invoke RPCs/functions.
- Do not list storage objects.
- Collect only PP-01 metadata.
- Stop if access looks mutation-capable or unclear.
- Mark items Not executed if read-only safety is uncertain.

## 9. Explicitly Forbidden Permissions

- INSERT.
- UPDATE.
- DELETE.
- UPSERT/MERGE.
- CREATE/ALTER/DROP.
- GRANT/REVOKE.
- BYPASSRLS.
- Object ownership.
- service_role semantics.
- Function execution for app/admin flows.
- Storage object listing.
- Secret access.
- Deployment permission.

## 10. Evidence Scope Allowed Under Model B

- Table/schema metadata.
- RLS enabled/disabled metadata.
- Policy metadata.
- Function metadata, security mode, grants, and search_path metadata.
- Storage bucket metadata only.
- Storage policy metadata.
- Migration metadata.
- Realtime metadata.
- Edge Function deployment metadata if separately exposed read-only.
- Notification/diagnostics/commerce/deletion/moderation/admin/media/messaging metadata only.

## 11. Evidence Scope Forbidden Under Model B

- User rows.
- Auth users rows.
- Tickets/orders/reservations/claims rows.
- Message bodies.
- Profile private data.
- Reports/evidence.
- Diagnostics payloads.
- Notification payloads.
- Support notes.
- Storage object names/paths if user-content exposed.
- Signed URL generation.
- Provider/payment payloads.
- Secrets/tokens.

## 12. Provisioning Status

Model B is not provisioned by this decision record.

Provisioning must be a separate owner-approved production administration task.

This document does not include SQL or production commands.

## 13. Revocation / Expiry Requirement

Temporary access must expire or be revoked after evidence collection.

The operator must record revocation.

The verifier must not retain credentials.

If credentials leak, access must be rotated or revoked immediately.

## 14. Handoff to PP-01 Production Evidence Collection

After Model B is provisioned and confirmed, update or rerun `00_Status/PP01ProductionVerificationExecutionReport.md`.

The evidence report must remain metadata-only.

Commit the evidence report only after diff review.

No implementation follows automatically.

## 15. Acceptance Criteria

This decision is complete only when:

- Model B is recorded as selected.
- A and C are documented as alternatives.
- Model B safety boundary is documented.
- Operator requirements are documented.
- Verifier requirements are documented.
- Forbidden permissions are documented.
- Provisioning status is explicitly Not provisioned.
- No production access was executed.

## 16. Explicitly Blocked Claims

- Do not claim access was provisioned.
- Do not claim PP-01 evidence was collected.
- Do not claim production verified.
- Do not claim implementation authorized.
- Do not claim launch-ready.
- Do not claim legally compliant.
- Do not claim security hardened.

## 17. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No database role was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No read-only production access was provisioned or used.
- No private data, secrets, storage objects, messages, tickets, orders, diagnostics, reports, support notes, or auth users were inspected.
- No implementation, production verification, admin/support action, storage/media action, messaging action, deletion/export action, refund/payment action, moderation action, RLS/RPC/storage/realtime action, Edge Function action, notification action, commerce action, or policy publication was executed.
- No files were staged or committed.
- Only `09_Decisions/SupabaseProductionReadOnlyAccessModelSelection.md` was created/modified.

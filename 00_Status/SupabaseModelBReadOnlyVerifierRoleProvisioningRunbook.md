# Supabase Model B Read-Only Verifier Role Provisioning Operator Runbook

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook synthesis plus selected Model B decision only
- canonical: false
- Runbook status: Prepared; not executed
- Selected access model: B â€” Temporary Database Read-Only Verifier Role
- Access status: Not provisioned
- Production verification status: Not executed
- Implementation status: Not authorized
- Legal status: Engineering access planning only; not legal advice

## 2. Purpose

This runbook defines the checklist for an operator to prepare Model B read-only verifier access for PP-01 metadata evidence collection.

It is not production access execution, not SQL execution, not database role creation, not implementation work, not legal advice, not launch approval, and not patch authorization.

## 3. Evidence Boundary

No production access, SQL, CLI, dashboard action, database role creation, source modification, private data inspection, build, test, dependency install, implementation, or legal review was performed.

This runbook is based on the operator-confirmed read-only access protocol, the PP-01 execution report, the release hardening completion report, the Model B access-model selection decision, and the PP-01 production verification pack.

## 4. Current State

- Model B has been selected in the decision record.
- Access is not provisioned.
- PP-01 production metadata evidence is not collected.
- Implementation is not authorized.
- Launch readiness is not claimed.

## 5. Runbook Goal

The goal is to enable future safe metadata-only PP-01 evidence collection without granting mutation capability or exposing private data.

This runbook defines the controlled state required before a verifier can collect production metadata evidence.

## 6. Non-Goals

- No source code change.
- No SQL included.
- No role creation by this document.
- No production connection by this document.
- No private row access.
- No storage object listing.
- No RPC/function invocation.
- No implementation approval.
- No legal/compliance approval.

## 7. Roles and Responsibilities

| Role | Responsibilities | Boundaries |
|---|---|---|
| Owner/operator | Approves exact production target, provisions access separately, confirms boundaries, revokes access. | Does not use this runbook as SQL or production command authorization. |
| Verifier | Collects metadata only, refuses unsafe access, writes sanitized PP-01 evidence report. | Does not inspect private rows, secrets, storage objects, or invoke application behavior. |
| Reviewer | Reviews diff/evidence report for secrets/private data before commit. | Does not treat evidence report as implementation authorization or launch approval. |

## 8. Pre-Provisioning Preconditions

- Decision record selecting Model B is committed.
- Exact production Supabase project is identified by operator.
- Owner approval exists for temporary read-only verifier access.
- Access expiry/revocation plan exists.
- Verifier understands no private rows and no mutation.
- Evidence output file is identified as `00_Status/PP01ProductionVerificationExecutionReport.md`.
- Stop conditions are understood.

## 9. Operator Checklist Before Provisioning

- Confirm production project.
- Confirm this is production, not staging/local.
- Confirm selected model B.
- Confirm no service_role key will be shared.
- Confirm no owner/admin dashboard access will be treated as read-only.
- Confirm no DML/DDL/grant/revoke/bypassrls/object ownership.
- Confirm no storage object listing.
- Confirm no secrets pages/files.
- Confirm access expiry time/date.
- Confirm revocation owner.
- Confirm allowed evidence categories.
- Confirm forbidden evidence categories.

## 10. Required Access Boundary for Model B

Model B access must be:

- Temporary.
- Non-owner.
- No service_role.
- No BYPASSRLS.
- No DML.
- No DDL.
- No grant/revoke.
- No deployment permissions.
- No storage object listing.
- Metadata-only access.
- No private row inspection.
- Revocable.

## 11. Explicitly Forbidden Capabilities

- INSERT.
- UPDATE.
- DELETE.
- UPSERT/MERGE.
- CREATE/ALTER/DROP.
- GRANT/REVOKE.
- BYPASSRLS.
- Object ownership.
- service_role semantics.
- App/admin RPC execution.
- Storage object listing/download/upload/delete.
- Secret access.
- Deployment permission.
- Provider/payment payload access.
- Auth user row inspection.
- Private support/report/message/ticket/order rows.

## 12. Allowed Metadata Evidence After Provisioning

- Project/environment identity if non-secret.
- Table/schema metadata.
- RLS enabled/disabled metadata.
- Policy metadata.
- Function metadata/security mode/grants/search_path metadata.
- Storage bucket metadata only.
- Storage policy metadata.
- Migration/provenance metadata.
- Realtime metadata.
- Edge Function deployment metadata only if separately exposed read-only.
- Notification/diagnostics/commerce/deletion/moderation/admin/media/messaging metadata only.

## 13. Forbidden Evidence After Provisioning

- User rows.
- Auth users rows.
- Tickets/orders/reservations/claims rows.
- Message bodies.
- Profile private data.
- Report evidence.
- Diagnostics payloads.
- Notification payloads.
- Support notes.
- Storage object names/paths where user content may be exposed.
- Signed URL generation.
- Provider/payment payloads.
- Secrets/tokens.
- Screenshots containing private data.

## 14. Provisioning Package Requirements

The operator must provide a sanitized access package containing:

- Access model confirmation.
- Production project confirmation.
- Allowed evidence categories.
- Forbidden evidence categories.
- Expiry/revocation plan.
- Contact/person responsible for revocation.
- Confirmation that no service_role or secrets are shared.

Credentials must not be included in this runbook.

## 15. Credential Handling Rules

- Credentials must not be committed.
- Credentials must not be pasted into ChatGPT/Codex.
- Verifier must not retain credentials after the evidence window.
- Accidental exposure requires immediate revoke/rotate.
- Evidence report must not include credentials.

## 16. Expiry / Revocation Plan

- Temporary access must expire or be revoked after PP-01 evidence collection.
- Revocation must be recorded by operator.
- If evidence collection is aborted, access must still be revoked.
- No lingering verifier access.

## 17. Incident Stop Conditions

Stop if:

- Access appears mutation-capable.
- service_role is exposed.
- Private rows are visible or required.
- Storage object listing is required.
- Project/environment target is uncertain.
- Credentials/secrets are requested by tool.
- SQL or UI path cannot be constrained to metadata-only.
- Any step would invoke application behavior.
- Any output would expose private data.

## 18. Verifier Intake Checklist

- Confirm production target from operator.
- Confirm access model B.
- Confirm access is temporary.
- Confirm no service_role.
- Confirm no mutation capability.
- Confirm no private row inspection.
- Confirm no storage object listing.
- Confirm evidence scope.
- Confirm stop conditions.
- Refuse access if unclear.

## 19. PP-01 Evidence Collection Handoff

After provisioning is confirmed, update or rerun `00_Status/PP01ProductionVerificationExecutionReport.md`.

The evidence report remains metadata-only.

No source patching, implementation, legal/compliance claim, or launch claim follows from evidence collection.

The evidence report commit requires diff review.

## 20. Implementation Prompt Release Gates

Surgical implementation prompts may begin only after these gates:

- Gate 1: Model B selection committed.
- Gate 2: Model B provisioning runbook committed.
- Gate 3: Operator confirms Model B access provisioned safely.
- Gate 4: PP-01 production metadata evidence collected and reviewed.
- Gate 5: Evidence gaps are classified by risk and implementation scope.
- Gate 6: Owner approves one scoped implementation patch at a time.

Implementation prompts must not be issued before Gate 4 is complete.

## 21. Definition of Controlled State

JoinFolk can be considered under controlled engineering process only when:

- Planning docs exist.
- Access model selected.
- Runbook exists.
- Actual production evidence collected.
- Evidence gaps known.
- No blind implementation.
- Changes proceed one scope at a time with diff/build/test/manual verification/rollback.

This is not the same as launch-ready.

## 22. Acceptance Criteria

This runbook is complete only when:

- Model B checklist is documented.
- Operator and verifier responsibilities are documented.
- Allowed/forbidden boundaries are documented.
- Expiry/revocation is documented.
- Incident stop conditions are documented.
- Implementation prompt gates are documented.
- No SQL or production commands are included.
- No production access was executed by this runbook.

## 23. Explicitly Blocked Claims

- Do not claim access was provisioned.
- Do not claim PP-01 evidence was collected.
- Do not claim production verified.
- Do not claim implementation authorized.
- Do not claim launch-ready.
- Do not claim legally compliant.
- Do not claim security hardened.
- Do not claim everything is fixed.

## 24. No-Modification Confirmation

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
- Only `00_Status/SupabaseModelBReadOnlyVerifierRoleProvisioningRunbook.md` was created/modified.

# Operator-Confirmed Read-Only Supabase Production Access Protocol

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook synthesis plus operator access planning only
- canonical: false
- Execution status: Not executed
- Access status: Not provisioned
- Production verification status: Not executed
- Implementation status: Not authorized
- Legal status: Engineering access planning only; not legal advice

## 2. Purpose

This protocol defines how an operator can provide a safe read-only production access path for PP-01 evidence collection.

It does not create access, execute queries, verify production, authorize implementation, approve launch, or provide legal advice.

## 3. Evidence Boundary

This protocol is based only on handbook synthesis and public platform role principles.

No production access, SQL, CLI, dashboard observation, private data inspection, source-code modification, build, test, dependency install, legal review, or implementation work was performed.

## 4. Why This Protocol Exists

The PP-01 Production Verification Execution Report found no operator-confirmed read-only production access path in the handbook workspace.

Production evidence remains needed before production-dependent implementation decisions can be made for RPC/RLS/grants/search path, storage, Edge Functions, realtime, migration provenance, notification, commerce, deletion, moderation, admin/support, media, and messaging surfaces.

This protocol exists to let an operator provide a controlled evidence path without granting unnecessary mutation capability or exposing private data.

## 5. Access Principles

- Least privilege.
- Read-only by default.
- No service_role.
- No private row inspection.
- No mutation-capable automation.
- No secrets in reports.
- No production writes.
- No implicit implementation authorization.
- Evidence must be reproducible, sanitized, and scoped.

## 6. Allowed Access Models

| Model | Description | Allowed use | Required operator confirmation | Risk | Recommended status |
|---|---|---|---|---|---|
| Supabase Read-Only dashboard role where plan supports it | Operator grants a read-only organization/project role if available. | Metadata observation only. | Project, environment, role, read-only limits, expiry. | Low to medium, depending on platform role limits. | Preferred if truly read-only. |
| Temporary database read-only verifier role | Operator creates a time-limited database role for metadata-only inspection. | Metadata queries only, no private row reads. | Role privileges, no RLS bypass, no object ownership, expiry/revocation. | Medium; depends on exact privileges. | Fallback with explicit owner approval. |
| Read replica / isolated read context | Operator provides isolated read context if available. | Metadata-only verification away from primary production. | Replica/source identity, staleness, privilege limits, no private rows. | Low to medium; data visibility still matters. | Optional if available. |
| Operator-provided sanitized screenshots/exports of metadata only | Operator collects metadata evidence and provides sanitized artifacts. | Evidence intake without direct verifier access. | Sanitization, source, date, production target, no private data. | Medium; reproducibility may be lower. | Acceptable for constrained evidence. |

## 7. Preferred Access Path A â€” Supabase Read-Only Dashboard Role

The preferred path is an operator-granted Supabase Read-Only organization/project role if the current Supabase plan supports it.

Boundary:

- Verifier uses Dashboard only for metadata observation.
- No table row browsing.
- No SQL mutation.
- No storage object listing.
- No secret pages.
- No project settings changes.
- No deploy operations.
- Evidence captured as sanitized notes or screenshots if allowed.

If a true Read-Only role is unavailable in the current plan, this path is Not available and must not be substituted with mutation-capable access unless the owner explicitly accepts that risk and defines supervision/constraints.

## 8. Fallback Access Path B â€” Temporary Database Read-Only Verifier Role

An operator may create a temporary database login role with narrowly scoped read-only metadata privileges.

Required constraints:

- The role must not own objects.
- The role must not bypass RLS.
- The role must not have service_role semantics.
- The role must not have insert, update, delete, DDL, grant, revoke, deploy, or storage mutation permissions.
- The role should be time-limited and revoked after evidence collection.
- Setup is a separate owner-approved production administration task, not part of PP-01 execution.

This protocol does not include executable SQL for creating that role.

## 9. Optional Access Path C â€” Read Replica / Isolated Read Context

A read replica or isolated read context may be used only if available and owner-approved.

Constraints still apply:

- No private rows.
- No mutation.
- No service_role.
- No object listing beyond metadata.
- No secret access.
- No implementation work.

This path may help isolate metadata reads from primary production, but this protocol does not claim such a replica or isolated context exists.

## 10. Explicitly Forbidden Access Methods

- service_role key.
- anon/authenticated user tokens for evidence.
- Owner/Admin/Developer dashboard access treated as read-only without constraint.
- SQL Editor unrestricted mutation access unless the operator explicitly constrains and supervises read-only metadata collection.
- Local env files or secrets.
- Production API calls that invoke application behavior.
- RPC/function invocation.
- Storage object listing.
- User-content table row SELECT.
- Auth user row inspection.
- Provider/payment payload inspection.
- Diagnostics/private payload inspection.

## 11. Operator Confirmation Checklist

- I identify the exact production project.
- I confirm environment is production.
- I confirm access model A/B/C or operator-provided sanitized metadata artifacts.
- I confirm access is read-only for the intended evidence path.
- I confirm no service_role key will be used.
- I confirm no private user data access is permitted.
- I confirm no mutation/deploy/storage operation is permitted.
- I confirm access expiry/revocation plan.
- I confirm evidence output path.

## 12. Verifier Confirmation Checklist

- I will not inspect secrets.
- I will not inspect private rows.
- I will not run mutation SQL.
- I will not invoke RPCs.
- I will not list storage objects.
- I will only collect metadata needed by PP-01.
- I will stop if access looks mutation-capable or unclear.
- I will mark items Not executed if read-only safety is uncertain.

## 13. Safe Evidence Collection Boundaries

Allowed evidence is metadata only:

- Project/environment id/name if non-secret.
- Table/schema existence.
- RLS enabled/disabled state.
- Policy names and definitions.
- Function metadata, security mode, grants, and search path.
- Storage bucket names and public flag only.
- Storage policy metadata.
- Edge Function deployment names/status only.
- Realtime publication/table metadata.
- Migration history metadata.

No row contents are allowed.

## 14. Allowed Evidence Categories

- Supabase project/environment.
- Migration/provenance.
- Database schema metadata.
- RLS enablement.
- RLS policies.
- RPC/function metadata.
- SECURITY DEFINER/search_path/grants.
- Storage bucket metadata.
- Storage policy metadata.
- Edge Function deployment list/status.
- Realtime metadata.
- Notification/diagnostics metadata only.
- Commerce/payment metadata only.
- Deletion/privacy implementation metadata only.
- Moderation/abuse metadata only.
- Ops/admin/support metadata only.
- Media/storage lifecycle metadata only.
- Messaging/realtime privacy metadata only.

## 15. Forbidden Evidence Categories

- Private profile rows.
- Auth users rows.
- Tickets/orders/reservations/claims rows.
- Payment provider payloads.
- Message bodies.
- Notification payloads.
- Diagnostics payloads.
- Report evidence.
- Support notes.
- Storage object names/paths where user content may be exposed.
- Signed URL generation.
- Tokens/secrets.
- Screenshots containing private user data.

## 16. Query Safety Rules

- Only metadata queries.
- Prefer transaction read-only mode if SQL is used.
- Stop if the tool cannot guarantee read-only behavior.
- No CALL/DO/function invocation.
- No DDL/DML.
- No analyze, vacuum, or materialized-view refresh.
- No copy/export of private data.
- No row-level content reads.

This protocol does not include ready-to-run SQL.

## 17. Storage Safety Rules

- Bucket metadata only.
- No object listing.
- No object download.
- No signed URL generation.
- No upload/delete/move/copy.
- No cache purge.
- No bucket policy changes.

## 18. RPC / Function Safety Rules

- Function metadata may be inspected.
- Function body may be summarized only if needed for gate verification and if it contains no secrets or private data.
- Do not invoke application functions.
- Do not test admin/refund/deletion/transfer/moderation/media/messaging functions.
- Do not paste large bodies into evidence reports.

## 19. Edge Function / Realtime Safety Rules

- Deployment list/status only.
- No deploy.
- No secrets.
- No invocation.
- Realtime metadata only.
- No channel subscription using real user context.

## 20. Private Data / Secrets Safety Rules

- No env files.
- No service_role.
- No JWT secrets.
- No API/provider/webhook secrets.
- No private user row data.
- No support/private evidence.

## 21. Evidence Artifact Rules

- Evidence report must use sanitized facts.
- Store source type for each fact.
- No raw secrets.
- No private row values.
- No screenshots with personal data.
- Use Unknown / Needs verification where uncertain.

## 22. Access Expiry / Revocation Rules

- Temporary access should expire or be revoked after evidence collection.
- Operator records revocation.
- Verifier does not retain credentials.
- Rotate affected credentials if accidental exposure occurs.
- Access should not persist beyond the approved evidence window.

## 23. Incident Stop Conditions

Stop evidence collection if:

- Access can mutate data.
- service_role is exposed.
- Private data is visible unexpectedly.
- Production/project target is uncertain.
- Query/tool cannot guarantee read-only behavior.
- Secret/env file is required.
- Storage objects would need listing.
- Any evidence collection would invoke app behavior.

## 24. Handoff to PP-01 Evidence Collection

After the operator confirms an access path, create or update `00_Status/PP01ProductionVerificationExecutionReport.md` with actual evidence.

Do not implement. Do not patch app, web, mobile, dashboard, backend, Supabase, RLS, RPC, storage, realtime, Edge Functions, legal copy, support processes, or production behavior.

Commit the evidence report only after diff review and confirmation that no secrets or private data are included.

## 25. Acceptance Criteria

This protocol is ready only when:

- Access model is selected.
- Operator checklist is completed.
- Verifier checklist is completed.
- Forbidden methods are documented.
- Stop conditions are documented.
- Evidence boundaries are documented.
- Revocation/expiry plan is documented.
- No production access was executed by this protocol.

## 26. Explicitly Blocked Claims

- Do not claim access was provisioned.
- Do not claim PP-01 production evidence was collected.
- Do not claim production verified.
- Do not claim launch-ready.
- Do not claim legally compliant.
- Do not claim security hardened.
- Do not claim implementation authorized.

## 27. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No read-only production access was provisioned or used by this protocol.
- No private data, secrets, storage objects, messages, tickets, orders, diagnostics, reports, support notes, or auth users were inspected.
- No implementation, production verification, admin/support action, storage/media action, messaging action, deletion/export action, refund/payment action, moderation action, RLS/RPC/storage/realtime action, Edge Function action, notification action, commerce action, or policy publication was executed.
- No files were staged or committed.
- Only `00_Status/OperatorConfirmedReadOnlySupabaseProductionAccessProtocol.md` was created/modified.

# Edge Function Deployment Inventory Decision

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: PP-01 metadata evidence and prior decision records
- canonical: false
- Decision status: Proposed; not implemented
- Inventory status: Proposed; incomplete until sanitized deployment inventory is reviewed
- Implementation status: Not authorized
- Production mutation status: Not executed
- Legal status: Engineering security decision only; not legal advice

## 2. Purpose

This record defines how Edge Function deployment status, endpoint exposure, auth posture, CORS behavior, secret/env boundary, webhook behavior, database/storage dependencies, and runtime risk should be inventoried and classified before any Edge Function change, source implementation, SQL, migration, dashboard action, or production change is authorized.

This is not implementation. It does not authorize Edge Function deployment changes, source-code changes, production access, secret inspection, verification queries, or runtime invocation.

## 3. Evidence Boundary

This document is based only on sanitized PP-01 metadata evidence and committed handbook reports/decisions.

No new production access, SQL, CLI, dashboard action, Edge Function inspection, secret inspection, source inspection, private data inspection, storage object listing, storage object download, build, test, dependency install, migration, deployment, or implementation was performed.

No credentials, hostnames, full project refs, connection strings, service_role keys, anon keys, JWT secrets, tokens, webhook secrets, API keys, environment variable values, private row values, storage object names, storage object paths, message bodies, auth user rows, tickets/orders/reservations/claims rows, diagnostics payloads, notification payloads, support notes, or payment payloads are included.

## 4. Decision Status

Decision status: Proposed / Draft.

Edge Function inventory model is defined.

Actual function-by-function classification is not complete until sanitized deployment inventory is reviewed.

Implementation is not authorized.

This document must be reviewed before any Edge Function patch plan or implementation prompt is issued.

## 5. PP-01 Evidence Summary

PP-01 metadata evidence and follow-up decision records reported:

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
- Edge Function deployment status not verified.
- Edge Function runtime behavior not verified.
- Edge Function auth/CORS/webhook exposure not verified.
- Edge Function secret/env configuration not inspected.
- No exploitability claim exists.
- No production safe/unsafe final claim exists.
- No implementation is authorized.

## 6. Problem Statement

Database RLS/grants and storage policy evidence do not fully prove Edge Function exposure.

Edge Functions may bypass direct client restrictions by using privileged secrets, service roles, RPCs, storage operations, external APIs, or webhook handlers.

A function can be safe in source intent but risky in deployment configuration.

A function can be deployed even if the handbook/source inventory is incomplete.

Unknown Edge Function deployment and runtime exposure blocks launch-ready claims.

Metadata evidence does not prove exploitability.

## 7. Decision Principles

- Classify before patching.
- Deployment inventory before implementation.
- Endpoint exposure must be explicit.
- Public endpoints require documented public-safe purpose.
- Authenticated endpoints must verify caller identity server-side.
- Webhook endpoints require signature/secret verification classification.
- CORS must match intended clients.
- service_role or privileged env usage must be treated as high-risk until classified.
- Edge Functions must be cross-referenced with RLS, RPC, storage, and external API dependencies.
- Secret values must not be copied into the handbook.
- No destructive action without owner review.
- No production-safe or launch-ready claim until verification.

## 8. Edge Function Deployment Classification Model

| Class | Description | Default decision posture |
|---|---|---|
| Deployed active app function | Function deployed for current user-facing app flows. | Must have endpoint, auth, dependency, and runtime operation classification before implementation claims. |
| Deployed internal/admin function | Function deployed for operational, support, admin, or maintenance use. | Requires strict authority, auditability, and least-privilege review. |
| Deployed webhook function | Function receiving provider or third-party callbacks. | Requires signature/secret verification, idempotency, and payload handling classification. |
| Deployed storage/media function | Function handling upload, signing, transformation, moderation, or storage object access. | Cross-reference storage exposure and media lifecycle decisions before accepting. |
| Deployed payment/commerce function | Function handling payment, order, refund, dispute, ticket, or provider operations. | Treat as high-risk; require commerce/payment owner and provider behavior verification. |
| Deployed notification/messaging function | Function sending or processing notifications, messages, realtime events, or delivery state. | Require payload minimization, auth, and privacy lifecycle classification. |
| Deployed diagnostic/support function | Function handling diagnostics, support, evidence, audit, or operational visibility. | Require private-data access and auditability decisions. |
| Legacy/deprecated deployed function | Function deployed but no longer expected to serve active product flows. | Must be retained, restricted, removed, or documented by owner decision; no removal authorized here. |
| Source-only not deployed function | Function present in docs/source inventory but not deployed in production. | Track as implementation provenance issue; no production exposure claim without deployment evidence. |
| Unknown deployment status function | Function whose deployment state is not verified. | Blocking classification item for P0/P1 Edge Function exposure before launch-ready claim. |

## 9. Endpoint Exposure Classification Model

| Exposure class | Meaning | Default decision posture |
|---|---|---|
| Public unauthenticated endpoint | Endpoint can be called without user authentication. | Accept only for documented public-safe purpose and safe payload behavior. |
| Authenticated user endpoint | Endpoint requires signed-in user identity. | Require server-side identity verification and negative tests. |
| Host/staff/admin-gated endpoint | Endpoint requires product authority beyond sign-in. | Require backend-enforced role/authority gate and auditability where privileged. |
| Webhook endpoint | Endpoint is called by provider or third party. | Require signature/secret verification, replay/idempotency handling, and owner review. |
| Internal/service-only endpoint | Endpoint intended for backend or trusted automation only. | Confirm no public/client path and classify privileged dependency. |
| Scheduled/background endpoint | Function runs on a schedule or background trigger. | Require deployment trigger, side-effect, and rollback classification. |
| CORS-limited browser endpoint | Endpoint intended for browser clients with CORS constraints. | Treat CORS as client boundary only; auth and payload safety still required. |
| No external endpoint / source-only | Function is not externally reachable or is not deployed. | No runtime exposure claim without deployment evidence. |
| Unknown endpoint exposure | Endpoint reachability or auth boundary is not classified. | Blocking item until sanitized inventory is reviewed. |

## 10. Auth and Authorization Classification Model

| Auth pattern | Description | Default decision posture |
|---|---|---|
| No auth / public | No caller identity or secret validation is required. | Accept only for public-safe, non-mutating or explicitly designed public behavior. |
| JWT authenticated | Caller identity is validated through a signed user token or equivalent. | Require server-side verification and actor/row dependency mapping. |
| Role/claim-gated | Caller must have specific claims or roles. | Require claim source, freshness, and negative tests. |
| App-level host/staff/admin gate | Function checks product authority such as host, staff, admin, or support. | Require backend-authoritative gate and auditability where privileged. |
| Webhook signature verified | Function validates provider signature or equivalent proof. | Require provider-specific verification and replay/idempotency classification. |
| Shared secret/header based | Function relies on shared secret or header. | Treat as privileged; secret handling and rotation plan required. |
| service_role/internal only | Function is called only from trusted server-side context. | Confirm no public/client path and classify privileged dependencies. |
| Mixed/conditional auth | Function has multiple auth branches or fallback paths. | Requires full branch-by-branch classification before acceptance. |
| Unknown auth posture | Auth behavior is not verified. | Blocking classification item for deployed P0/P1 functions. |

Actual auth behavior is not verified here.

## 11. Secret / Environment Boundary

Secret names may be classified only if approved and sanitized.

Secret values must never be written into the handbook.

Environment variable values must never be inspected or copied.

service_role usage must be classified as privileged.

External API keys, webhook secrets, payment provider secrets, notification credentials, and storage signing keys are sensitive.

Missing secret inventory blocks final Edge Function risk classification.

This decision authorizes no secret access.

## 12. Runtime Operation Classification

| Runtime operation | Exposure concern | Default posture |
|---|---|---|
| Database read | May expose private or sensitive data through function response. | Require data-class and caller-authority classification. |
| Database write | May mutate production state or bypass client RLS expectations. | Treat as high-risk until owner, auth, and rollback model are accepted. |
| SECURITY DEFINER RPC call | May bypass caller-level table permissions. | Cross-reference function grant hardening decision and body/gate behavior. |
| Storage read/download | May expose object content. | Cross-reference storage exposure and media lifecycle decisions. |
| Storage list | May expose object metadata or organization. | Minimize and classify as sensitive. |
| Storage upload/write | May create content, storage costs, or abuse vectors. | Require actor, file, path, and lifecycle controls. |
| Storage delete | May destroy content or evidence. | Require strict authority and rollback/retention decision. |
| Signed URL creation | May provide object access outside direct policy checks. | Require intended actor, lifetime, and logging classification. |
| Payment/commerce provider call | May move money or alter revenue state. | Require commerce owner and provider verification. |
| Messaging/notification send | May leak private content or spam users. | Require payload minimization and notification/privacy review. |
| External HTTP request | May create side effects or expose data to third parties. | Require endpoint purpose, payload, retry, and error logging classification. |
| Admin/support/moderation action | May alter authority, content visibility, or user state. | Require PP-07/PP-08 authority and auditability decisions. |
| Unknown runtime operation | Runtime behavior is not classified. | Blocking item for deployed functions before launch-ready claims. |

## 13. CORS / Browser Client Boundary

CORS is not authentication.

Allowed origins must match intended app/web/dashboard clients.

Broad CORS may be acceptable only when auth and payload behavior are safe.

Dashboard-only functions should not be treated as mobile/public functions unless intended.

Unknown CORS posture blocks endpoint safety claims.

No CORS change is authorized here.

## 14. Webhook Handling Boundary

Webhook endpoints must verify provider signature/secret or equivalent authority.

Webhook replay/idempotency handling must be classified.

Payment/commerce webhooks are high-risk.

Webhook payload contents must not be copied into the handbook.

No webhook invocation or secret inspection is authorized here.

## 15. Relationship to RLS / Grant Matrix

Edge Functions may use service_role or privileged RPCs and therefore bypass direct table grant assumptions.

RLSPolicyAndGrantMatrixClassification.md must be cross-referenced for database dependencies.

A table being RLS-protected does not prove Edge Function-mediated access is safe.

No RLS/grant change is authorized here.

## 16. Relationship to Storage Exposure

Edge Functions may create signed URLs, upload, delete, transform, or expose storage objects.

StorageBucketExposureDecision.md must be cross-referenced for media/storage dependencies.

No storage policy or bucket change is authorized here.

## 17. Relationship to SECURITY DEFINER / Function Grants

Edge Functions may invoke SECURITY DEFINER RPCs or depend on broad function EXECUTE grants.

SecurityDefinerAndFunctionGrantHardeningDecision.md must be cross-referenced before claiming Edge Function paths are safe.

No function grant or SQL change is authorized here.

## 18. Proposed Target State

- Every deployed Edge Function has deployment status classified.
- Every deployed Edge Function has endpoint exposure classified.
- Every deployed Edge Function has auth posture classified.
- Every deployed Edge Function has CORS/webhook posture classified where relevant.
- Every deployed Edge Function has secret/env usage classified without exposing values.
- Every deployed Edge Function has database/storage/RPC/external dependency classification.
- Every legacy/deprecated function has retain/remove/restrict decision.
- No Unknown P0/P1 Edge Function exposure remains before launch-ready claim.

## 19. Required Future Evidence

Required future evidence includes:

- Sanitized Edge Function deployment inventory.
- Function names only if approved.
- Deployment status.
- Endpoint exposure class.
- Auth posture class.
- CORS posture class.
- Webhook posture if relevant.
- Secret/env usage classification without values.
- Database/RPC dependency classification.
- Storage dependency classification.
- External API dependency classification.
- Source/deployment provenance.
- No secret values.
- No payload bodies.
- No private data.

## 20. Required Future Implementation Scope

No implementation is authorized yet.

Future implementation may include endpoint restriction, auth hardening, CORS adjustment, webhook signature enforcement, secret rotation plan, source-code patch, deployment removal, function redeploy, RPC/storage dependency adjustment, or documentation-only acceptance.

Exact action depends on classification and owner approval.

Destructive/deployment removal actions require separate explicit decision.

Implementation must be split into scoped patches.

## 21. Required Verification Scope

Future verification scope should include:

- Diff review.
- Deployment/config diff review if applicable.
- No secret value inspection.
- No private data inspection.
- Metadata-only verification where possible.
- Anonymous endpoint negative tests.
- Authenticated non-authorized negative tests.
- Intended actor positive tests.
- Webhook signature negative/positive tests where relevant.
- CORS behavior verification where relevant.
- Storage/RPC side-effect smoke tests only after explicit authorization.
- Rollback verification.

## 22. Rollback / Safety Requirements

- Every future source/deployment/config change must have rollback notes.
- Edge Function deployment changes must be reversible or staged.
- Auth/CORS/webhook changes should be patched in small scoped batches.
- No function deletion or deploy removal without separate decision.
- No secret rotation without separate owner-approved plan.
- Failed smoke tests block release-readiness claims.

## 23. Explicit Non-Goals

- No SQL.
- No migration.
- No source changes.
- No production mutation.
- No Edge Function deploy.
- No Edge Function delete.
- No Edge Function invocation.
- No secret inspection.
- No secret rotation.
- No CORS change.
- No auth change.
- No webhook call.
- No storage object action.
- No function/RPC change.
- No launch approval.
- No legal/compliance claim.

## 24. Risks and Open Questions

- Edge Functions may be deployed without current handbook inventory.
- Public endpoints may be intentional but undocumented.
- Auth may rely on client-side assumptions.
- CORS may be too broad.
- Webhook signature handling may be incomplete.
- service_role usage may create privileged bypass paths.
- Storage signed URL generation may bypass bucket assumptions.
- External API calls may have hidden side effects.
- Legacy functions may remain deployed after product flow changes.
- Runtime behavior is not verified by metadata alone.

## 25. Follow-Up Artifacts

- SupabaseMigrationSourceOfTruthDecision.md
- EdgeFunctionDeploymentPatchPlan.md only after deployment inventory classification and owner approval.
- StorageBucketExposurePatchPlan.md only after bucket/policy classification and owner approval.
- RLSGrantMatrixPatchPlan.md only after matrix classification and owner approval.
- JoinFolkReleaseCandidateReadinessReport.md only after implementation and verification gates are complete.

## 26. Implementation Authorization Status

Implementation remains not authorized.

No SQL, migration, source change, Edge Function deploy/delete/update/invocation, secret inspection, secret rotation, CORS change, auth change, webhook call, storage action, production mutation, or verification query is authorized by this decision.

## 27. Explicitly Blocked Claims

- Do not claim exploitability.
- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim legal compliance.
- Do not claim Edge Functions hardened.
- Do not claim Edge Function exposure resolved.
- Do not claim implementation authorized.
- Do not claim all runtime/API/webhook risk is resolved.

## 28. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No database role was created.
- No production connection was made.
- No production mutation was executed.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No Edge Function was deployed, updated, deleted, invoked, or inspected.
- No secret or environment variable value was inspected, copied, printed, rotated, or changed.
- No CORS/auth/webhook configuration was changed.
- No storage object was listed, downloaded, uploaded, modified, or deleted.
- No signed URL was generated.
- No credentials, hostnames, full project refs, service_role keys, database passwords, connection strings, webhook secrets, API keys, environment variable values, or secrets were included.
- No private rows, storage objects, object paths, messages, tickets, orders, diagnostics, reports, support notes, auth users, or payment payloads were inspected.
- No application RPC/function was invoked.
- No implementation/admin/support/storage/media/messaging/deletion/export/refund/payment/moderation/RLS/RPC/storage/realtime/Edge/notification/commerce action was executed.
- No files were staged or committed.
- Only 09_Decisions/EdgeFunctionDeploymentInventoryDecision.md was created/modified.

# Storage Bucket Exposure Decision

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: PP-01 metadata evidence and prior decision records
- canonical: false
- Decision status: Proposed; not implemented
- Implementation status: Not authorized
- Production mutation status: Not executed
- Legal status: Engineering security decision only; not legal advice

## 2. Purpose

This record defines how storage bucket exposure, bucket public/private state, storage object policies, media access, upload/delete/list behavior, signed URL behavior, and app media dependencies should be classified before any storage policy, SQL, migration, source implementation, dashboard action, or production change is authorized.

This is not implementation. It does not authorize storage policy changes, bucket configuration changes, source-code changes, production access, verification queries, or storage object actions.

## 3. Evidence Boundary

This document is based only on sanitized PP-01 metadata evidence and committed handbook reports/decisions.

No new production access, SQL, CLI, dashboard action, source inspection, private data inspection, storage object listing, storage object download, build, test, dependency install, migration, or implementation was performed.

No credentials, hostnames, full project refs, connection strings, service_role keys, anon keys, JWT secrets, tokens, private row values, storage object names, storage object paths, message bodies, auth user rows, tickets/orders/reservations/claims rows, diagnostics payloads, notification payloads, support notes, or payment payloads are included.

## 4. Decision Status

Decision status: Proposed / Draft.

Storage exposure model is defined.

Actual bucket-by-bucket classification is not complete until sanitized inventory is reviewed.

Implementation is not authorized.

This document must be reviewed before any storage policy patch plan or implementation prompt is issued.

## 5. PP-01 Evidence Summary

PP-01 metadata evidence and follow-up classification reported:

- Verifier direct table/storage access was closed.
- Verifier role membership rows = 0.
- Verifier effective table privilege rows for public/auth/storage/realtime = 0.
- storage_schema_usage = false.
- storage_objects_select = false.
- No storage objects listed.
- No private rows inspected.
- No functions invoked.
- No mutation executed.
- Verifier access revoked / NOLOGIN confirmed.
- Storage bucket public/private dashboard status not verified.
- Storage runtime behavior not verified.
- Storage object upload/read/list/delete behavior not verified.
- Storage policy relationship to app media flows not verified.
- No exploitability claim exists.
- No production safe/unsafe final claim exists.
- No implementation is authorized.

## 6. Problem Statement

Table/RLS evidence does not fully prove storage exposure.

Bucket public/private state is a separate exposure surface.

storage.objects RLS/policies, bucket public flags, signed URL behavior, CDN/public URL behavior, upload/delete/list rules, and app media usage must be evaluated together.

A verifier role having no storage direct access does not prove anon/authenticated/app media access is correctly scoped.

Public media may be intentional, but it must be explicitly classified.

Unknown storage exposure blocks launch-ready claims.

Metadata evidence does not prove exploitability.

## 7. Decision Principles

- Classify before patching.
- Public buckets must be intentionally public and public-safe.
- Private buckets must not rely on UI hiding as security.
- Upload permission must be narrower than read permission.
- Delete permission must be narrower than upload permission.
- List permission must be treated as sensitive.
- Signed URL behavior must be explicitly classified.
- Media visibility must align with event/profile/venue lifecycle.
- Storage policy must align with app RLS/RPC assumptions.
- No object listing or private object inspection is required for classification unless separately authorized.
- No destructive action without owner review.
- No production-safe or launch-ready claim until verification.

## 8. Storage Exposure Classification Model

| Class | Description | Default decision posture |
|---|---|---|
| Public-safe asset bucket | Bucket intended for static or public-safe assets with no private payloads. | May be public only with documented public-safe justification and owner approval. |
| Public event/media bucket | Bucket intended for event posters, public event media, or shareable public media. | Requires visibility lifecycle alignment and explicit public URL/cache expectations. |
| Authenticated upload bucket | Bucket accepting uploads from signed-in users. | Require narrow upload authority, ownership model, type/size controls, and list/delete restrictions. |
| Private user/media bucket | Bucket holding private or participant-scoped user/media content. | Must not be public; access must be policy-mediated or signed-URL-mediated by intended actor. |
| Host/venue-managed media bucket | Bucket for venue, business, host, or event owner media. | Require host/venue authority model and lifecycle decisions for transfer/deletion. |
| Moderation/quarantine bucket | Bucket for rejected, reported, or safety-review media. | Internal or least-privilege access only with retention and audit decisions. |
| Internal/admin/reporting bucket | Bucket for diagnostics, support, evidence, reporting, or admin artifacts. | Internal-only by default; support/admin access requires auditability decision. |
| Legacy/deprecated bucket | Bucket no longer expected to serve active product flows. | Must be classified, restricted, archived, or accepted by owner decision; no deletion authorized here. |
| Supabase-managed/platform storage relation | Platform-owned storage schema or relation used by Supabase internals. | Do not modify without separate explicit platform/schema decision. |
| Unknown bucket | Bucket purpose, sensitivity, or public state not yet classified. | Blocking classification item for P0/P1 storage exposure before launch-ready claim. |

## 9. Bucket Public Flag Classification

| Bucket public state | Meaning | Default decision posture |
|---|---|---|
| Public=true and intentionally public-safe | Objects may be reachable through public URL behavior by design. | Accept only with documented media class, lifecycle, cache, and public-copy alignment. |
| Public=true but content sensitivity unknown | Bucket is public but stored content has not been classified. | Treat as P1/P0 candidate until inventory and owner decision confirm safety. |
| Public=false with authenticated access policies | Bucket is private at bucket level but accessible through authenticated policies. | Require policy/role matrix and negative tests. |
| Public=false with signed URL access | Access occurs through generated time-bounded URLs or equivalent. | Require signed URL authority, lifetime, revocation, and logging decisions. |
| Public=false internal-only | Bucket intended for service/internal workflows only. | Confirm no client role, app route, or inherited exposure exists. |
| Unknown public/private state | Bucket public flag was not verified. | Needs dashboard or metadata verification before launch-ready claims. |

Bucket public/private status must be verified before launch-ready claims, but this document does not perform verification.

## 10. Storage Operation Classification

| Operation | Exposure concern | Default posture |
|---|---|---|
| Read/download | May expose object content. | Allow only for intended visibility class and actor model. |
| List | May expose metadata, object organization, or user activity patterns. | Minimize by default; avoid public or broad authenticated list unless justified. |
| Upload/insert | May allow unauthorized content placement or abuse. | Require actor authority, ownership, path policy, media constraints, and lifecycle review. |
| Update/replace | May overwrite legitimate content or bypass moderation. | Restrict to owner/host/internal authority with audit where privileged. |
| Delete | May destroy user or evidence content. | Restrict more narrowly than upload and require lifecycle/rollback decisions. |
| Signed URL create/use | May bypass direct bucket assumptions if generated broadly. | Treat as a distinct access path requiring authority and lifetime classification. |
| Public URL access | May persist through browser, CDN, crawler, or social preview behavior. | Accept only for public-safe media with visibility/cache expectations. |
| Transform/thumbnail access | May reveal derived content even if original is protected. | Classify with original media and cache lifecycle. |
| Metadata read | May reveal sensitive refs, ownership, names, timing, or paths. | Minimize and classify separately from content download. |
| Metadata write | May alter lifecycle, ownership, or visibility state. | Restrict to intended actors and audited privileged paths. |

List/delete/update are higher-risk than public-safe read.

## 11. Media Visibility Classes

| Media visibility class | Examples | Default storage posture |
|---|---|---|
| Public marketing/static assets | Product-safe public images or static media. | Public bucket/public URL may be acceptable if intentionally non-private. |
| Public event poster/cover media | Approved public event poster or cover image. | May be public only when event visibility and takedown lifecycle are accepted. |
| Public venue/business media | Approved public venue or business gallery media. | May be public with owner/venue lifecycle and deletion/transfer decisions. |
| Event participant media | Participant-uploaded event media. | Requires participant/host visibility and moderation lifecycle before public exposure. |
| Checked-in/live-event gallery media | Live event or check-in-linked gallery media. | Requires checked-in/user/host authority and retention decisions. |
| Host/private draft media | Draft media for events, venues, or host profiles. | Private by default; no public URL until publication decision. |
| User profile/avatar media | Profile, persona, or avatar images. | Public/private posture depends on profile visibility and deletion/redaction decisions. |
| Moderation/rejected media | Reported, rejected, quarantined, or takedown media. | Internal or least-privilege only with evidence retention decision. |
| Internal diagnostics/report media | Diagnostic, support, report, or operational evidence media. | Internal-only with auditability and retention classification. |
| Unknown media | Media purpose or sensitivity not yet classified. | Blocking item until owner and lifecycle classification exists. |

No actual object names or paths are included in this decision.

## 12. Storage Policy Classification Model

| Policy class | Description | Default decision posture |
|---|---|---|
| Public read policy | Allows public read/download for selected bucket or media class. | Accept only for public-safe media and documented visibility contract. |
| Authenticated read policy | Allows signed-in users to read selected objects. | Require role and lifecycle classification plus negative tests. |
| Owner/uploader read policy | Limits read to uploader or object owner. | Require stable ownership mapping and account deletion behavior. |
| Participant-scoped read policy | Limits read to event/group/conversation participants. | Require membership source, removal/block behavior, and lifecycle review. |
| Host/staff-scoped read policy | Limits read to host, staff, manager, or scanner authority. | Require staff/host authority boundary and transfer/delegation review. |
| Authenticated upload policy | Allows signed-in user upload. | Require path/ownership constraints, file controls, and abuse handling. |
| Owner/uploader update/delete policy | Allows owner/uploader mutation. | Require deletion/restoration expectations and evidence retention exceptions. |
| Host/staff moderation delete policy | Allows host/staff removal or moderation action. | Require PP-07/PP-08 authority and auditability decisions. |
| Internal/service-only policy | Intended for backend, service, or controlled automation. | Confirm no client role or inherited exposure. |
| Missing/disabled/unknown policy | Policy absent, disabled, or not classified. | Needs classification before launch-ready or implementation claims. |

## 13. Combined Storage Decision Rules

- Public=true is acceptable only for public-safe media with documented visibility contract.
- Public event poster/venue media can be acceptable if it contains no private payload and aligns with product visibility.
- Event participant media requires lifecycle and role boundary review.
- User/private media must not be public by default.
- List permission must be minimized even when read/download is public.
- Upload permission must validate actor authority, object ownership, media type, and lifecycle.
- Delete permission must be limited to owner/host/staff/internal authority.
- Signed URL behavior must be treated as an access path, not ignored.
- Storage metadata may reveal sensitive information even when files are not downloaded.
- Storage policy must be cross-referenced with RLS/RPC/media lifecycle decisions.
- No SQL or storage policy patch is authorized here.

## 14. Relationship to RLS / Grant Matrix

Storage bucket exposure is not fully covered by table RLS/grant classification.

storage.objects policies may depend on table rows, event roles, uploader identity, host identity, or RPC-mediated access.

RLSPolicyAndGrantMatrixClassification.md remains necessary for storage metadata dependencies.

A table being protected does not prove object access is protected.

An object being inaccessible does not prove metadata exposure is safe.

## 15. Relationship to SECURITY DEFINER / Function Grants

SECURITY DEFINER functions and RPCs may create signed URLs, write storage metadata, or mediate upload/delete flows.

Function grants must be cross-referenced before claiming storage is protected.

No function change is authorized here.

## 16. Relationship to Edge Functions / Realtime

Edge Functions may expose upload/signing/moderation/download endpoints.

Realtime may expose storage-related metadata if tables/channels are subscribed.

EdgeFunctionDeploymentInventoryDecision.md remains required.

Runtime realtime/storage behavior remains unresolved.

## 17. Proposed Target State

- Every bucket has a classified public/private state.
- Every bucket has a media/data visibility class.
- Every bucket has explicit read/list/upload/update/delete posture.
- Every public bucket has documented public-safe justification.
- Every private bucket has explicit access model.
- Every signed URL path has explicit intended actor model.
- Every app media flow has storage policy dependency classification.
- No Unknown P0/P1 storage exposure remains before launch-ready claim.

## 18. Required Future Evidence

Required future evidence includes:

- Sanitized bucket inventory.
- Bucket names only if approved; no object paths or object names.
- Public/private state.
- Sanitized storage policy inventory.
- Operation-level policy classification.
- App/dashboard media flow mapping.
- Upload/delete/list dependency mapping.
- Signed URL usage classification.
- Relation/RPC dependencies.
- No private object listing.
- No private object download.
- No storage object path disclosure.

## 19. Required Future Implementation Scope

No implementation is authorized yet.

Future implementation may include bucket public/private adjustment, storage policy adjustment, upload path restriction, delete/list restriction, signed URL flow adjustment, media lifecycle change, source-code change, or documentation-only acceptance.

Exact action depends on classification and owner approval.

Destructive actions require separate explicit decision.

Implementation must be split into scoped patches.

## 20. Required Verification Scope

Future verification scope should include:

- Diff review.
- Dashboard/storage config diff review if applicable.
- Migration diff review if SQL is later created.
- No private data inspection.
- No private object listing/download.
- Metadata-only verification where possible.
- Anonymous read/list negative tests.
- Authenticated non-owner negative tests.
- Uploader/owner positive tests.
- Participant/host/staff positive and negative tests where relevant.
- Upload/delete/list smoke tests only after explicit authorization.
- Signed URL behavior tests where relevant.
- Rollback verification.

## 21. Rollback / Safety Requirements

- Every future SQL/storage policy/config change must have rollback notes.
- Bucket public/private changes must be reversible or staged.
- Storage policies must be patched in small scoped batches.
- Delete/list restrictions should be staged carefully to avoid breaking app flows.
- No object deletion or bucket deletion without separate explicit decision.
- No Supabase-managed schema/storage internals change without separate explicit decision.
- Failed smoke tests block release-readiness claims.

## 22. Explicit Non-Goals

- No SQL.
- No migration.
- No source changes.
- No production mutation.
- No bucket config change.
- No storage policy change.
- No storage object listing.
- No storage object download.
- No storage object deletion.
- No signed URL generation.
- No function change.
- No Edge Function change.
- No launch approval.
- No legal/compliance claim.

## 23. Risks and Open Questions

- Public buckets may be intentional but undocumented.
- Private buckets may still have overly broad policies.
- List permission may expose metadata.
- Upload permission may allow unauthorized file placement if path/ownership checks are weak.
- Delete permission may allow destructive abuse if too broad.
- Signed URLs may bypass assumptions if generated too broadly.
- App media lifecycle may not match storage policy.
- Dashboard media tools may depend on broader access than mobile app flows.
- Legacy buckets may contain sensitive historical files.
- Storage runtime behavior is not verified by metadata alone.

## 24. Follow-Up Artifacts

- `EdgeFunctionDeploymentInventoryDecision.md`
- `SupabaseMigrationSourceOfTruthDecision.md`
- `StorageBucketExposurePatchPlan.md` only after bucket/policy classification and owner approval.
- `RLSGrantMatrixPatchPlan.md` only after matrix classification and owner approval.
- `JoinFolkReleaseCandidateReadinessReport.md` only after implementation and verification gates are complete.

## 25. Implementation Authorization Status

Implementation remains not authorized.

No SQL, migration, source change, bucket config change, storage policy change, function change, Edge Function change, storage object action, production mutation, or verification query is authorized by this decision.

## 26. Explicitly Blocked Claims

- Do not claim exploitability.
- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim legal compliance.
- Do not claim storage hardened.
- Do not claim bucket exposure resolved.
- Do not claim implementation authorized.
- Do not claim all media/storage risk is resolved.

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
- No bucket config was changed.
- No storage policy was changed.
- No storage object was listed, downloaded, uploaded, modified, or deleted.
- No signed URL was generated.
- No credentials, hostnames, full project refs, service_role keys, database passwords, connection strings, or secrets were included.
- No private rows, storage objects, object paths, messages, tickets, orders, diagnostics, reports, support notes, auth users, or payment payloads were inspected.
- No application RPC/function was invoked.
- No implementation/admin/support/storage/media/messaging/deletion/export/refund/payment/moderation/RLS/RPC/storage/realtime/Edge/notification/commerce action was executed.
- No files were staged or committed.
- Only 09_Decisions/StorageBucketExposureDecision.md was created/modified.

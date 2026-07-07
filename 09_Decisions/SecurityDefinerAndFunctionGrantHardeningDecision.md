# Security Definer and Function Grant Hardening Decision

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

This decision defines the intended hardening approach for:

- Broad function EXECUTE grants.
- SECURITY DEFINER functions.
- Missing proconfig/search_path candidates.
- Future Model B verifier role boundaries.

It does not implement anything.

## 3. Evidence Boundary

This decision is based only on sanitized PP-01 metadata evidence and handbook documents.

No new production access, SQL, CLI, dashboard action, source inspection, private data inspection, build, test, dependency install, migration, or implementation was performed.

No credentials, hostnames, full project refs, connection strings, service_role keys, anon keys, JWT secrets, tokens, private row values, storage object names, message bodies, auth user rows, tickets/orders/reservations/claims rows, diagnostics payloads, notification payloads, support notes, or payment payloads are included.

## 4. Decision Status

Decision status: Proposed / Draft.

Implementation is not authorized.

This record must be reviewed before any patch prompt is issued.

## 5. PP-01 Evidence Summary

PP-01 metadata evidence reported:

- anon public execute_count=372.
- authenticated public execute_count=397.
- service_role public execute_count=410.
- verifier public execute_count=350.
- verifier security_definer_execute_count=319.
- verifier table/storage direct access was closed.
- no functions were invoked.
- no private rows were inspected.
- no storage objects were listed.
- Model B verifier access was revoked / NOLOGIN confirmed.

## 6. Problem Statement

Broad EXECUTE privileges create an unclear authority boundary.

SECURITY DEFINER functions can bypass caller privileges by design.

Missing proconfig/search_path is a hardening candidate.

Metadata evidence does not prove exploitability, but it requires classification before implementation.

## 7. Decision Principles

- Default deny where feasible.
- Explicit grant over inherited/PUBLIC-style exposure.
- SECURITY DEFINER functions must have explicit search_path.
- Functions should be executable only by intended roles.
- App-facing RPCs must map to intended caller roles: anon, authenticated, service_role, or internal-only.
- Supabase-managed auth/storage/realtime internals must not be modified without separate explicit decision.
- No production SQL without separately approved implementation plan.
- No behavior claim until verified.

## 8. Function EXECUTE Grant Classification Model

| Class | Description | Intended grant posture |
|---|---|---|
| Public/anon-safe read RPC | Function intentionally callable without login for public-safe read behavior. | Explicit anon/authenticated grant only after owner acceptance; no accidental PUBLIC-style dependency. |
| Authenticated user RPC | Function intended for signed-in users acting on their own allowed data. | Explicit authenticated grant; no anon grant unless separately accepted. |
| Host/staff/admin-gated RPC | Function has internal gates for host, staff, scanner, support, or ops/admin actions. | Explicit grants only to caller roles required by the app, with body/gate review before changes. |
| Service-role/internal-only RPC | Function should be called only by backend/internal automation. | No anon/authenticated grant; service/internal path only after explicit decision. |
| Migration/maintenance-only RPC | Function is only for data repair, migration, backfill, or one-time maintenance. | No app-facing grant; remove/archive/restrict after owner decision. |
| Deprecated/unsafe/legacy RPC | Function is replaced, ambiguous, or not intended for active use. | Revoke from app-facing roles or remove only after dependency review. |
| Supabase-managed function | Function owned by platform-managed schemas such as auth/storage/realtime. | Do not alter without separate explicit vendor/schema decision. |

## 9. SECURITY DEFINER Hardening Standard

Proposed standard:

- All app-owned SECURITY DEFINER functions should have explicit search_path.
- Prefer `search_path=public, extensions` only when required.
- Avoid broad PUBLIC execute grants unless intentionally public.
- Review row_security settings intentionally.
- Keep function body changes out of this decision.
- Future implementation must be one scoped patch with rollback notes.

## 10. Missing proconfig Candidate List

The following are hardening candidates, not proven exploitable:

- `control_cancel_event(event_id uuid)`
- `control_end_event(event_id uuid)`
- `control_open_checkin(event_id uuid)`
- `delete_personal_reminder(p_id uuid)`
- `list_active_reminders()`
- `list_personal_reminders()`
- `publish_event(p_event_id uuid, p_visibility text)`
- `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])`
- `upsert_personal_reminder(p_id uuid, p_title text, p_note text, p_reminder_date date, p_color_theme text, p_remind_before_days integer)`

## 11. App-Facing RPC Exposure Classes

The next required classification artifact should map each app-owned RPC into an exposure class.

This decision does not attempt full RPC-by-RPC classification because the current evidence does not include app route/RPC usage mapping or behavior verification.

RPC families requiring careful review include:

- Admin/ops/support RPCs.
- Publish and event lifecycle RPCs.
- Check-in and staff/scanner RPCs.
- Payment/commerce/ticket/order RPCs.
- Messaging/DM RPCs.
- Storage/media/moderation RPCs.

## 12. Model B Verifier Role Implication

Decision:

- Future verifier role baseline must also constrain function EXECUTE privileges, not only table/storage privileges.
- Before any new production evidence window, create a hardened verifier role pattern that avoids accidental function invocation risk.
- Existing verifier role was revoked and should remain NOLOGIN.

## 13. Supabase-Managed Schema Boundary

Decision:

- Do not alter Supabase-managed auth/storage/realtime functions or grants without separate explicit vendor/schema decision.
- App-owned public schema functions are the primary target for future hardening.

## 14. Proposed Target State

- App-owned SECURITY DEFINER functions have explicit proconfig/search_path.
- App-owned RPC EXECUTE grants are explicit by intended caller role.
- Internal/admin/service functions are not executable by anon/public unless intentionally exposed.
- Deprecated/unsafe functions are revoked, archived, or removed only after separate owner decision.
- Model B verifier role cannot execute app-owned RPCs unless explicitly required for metadata evidence.

## 15. Required Future Implementation Scope

First possible implementation scope: function grant/proconfig hardening patch.

Status:

- Not authorized yet.
- Requires exact function list.
- Requires grant matrix.
- Requires rollback SQL/migration strategy.
- Requires owner approval.

## 16. Required Verification Scope

Future implementation verification must include:

- `git diff` review.
- Migration diff review if future SQL is created.
- No private data inspection.
- Build/test only for relevant app surfaces if source code is touched.
- Supabase metadata verification after patch, if approved.
- Manual smoke tests for auth/anon/authenticated RPC paths after patch.
- Rollback verification.

## 17. Rollback / Safety Requirements

- Every future SQL migration must have rollback notes.
- Grants must be reversible.
- No destructive drop/remove without separate decision.
- Prefer staged revoke/verify pattern.
- Avoid touching Supabase-managed schemas unless explicitly approved.

## 18. Explicit Non-Goals

- No implementation.
- No SQL.
- No migration.
- No source changes.
- No production mutation.
- No function body rewrite.
- No legal/compliance claim.
- No launch approval.

## 19. Risks and Open Questions

- Some RPCs may rely on current broad grants.
- Revoking too broadly could break app flows.
- Function body/gate behavior is not verified.
- Migration provenance is unresolved.
- Storage/Edge/realtime behavior is still unresolved.
- App route/RPC usage mapping is needed before a patch.

## 20. Follow-Up Artifacts

- RLSDisabledRelationTriageDecision.md
- RLSPolicyAndGrantMatrixClassification.md
- ModelBVerifierRoleHardeningDecision.md if future verifier access is needed.
- StorageBucketExposureDecision.md
- EdgeFunctionDeploymentInventoryDecision.md
- SupabaseMigrationSourceOfTruthDecision.md
- SecurityDefinerAndFunctionGrantHardeningPatchPlan.md only after classification artifacts are reviewed and owner approval is given.

The patch plan is not the next step by default; it remains gated behind classification, owner review, and explicit implementation authorization.
## 21. Implementation Authorization Status

Implementation remains not authorized.

No SQL, migration, source change, grant change, function change, or production mutation is authorized by this decision.

## 22. Explicitly Blocked Claims

- Do not claim exploitability.
- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim legal compliance.
- Do not claim security hardened.
- Do not claim implementation authorized.
- Do not claim everything fixed.

## 23. No-Modification Confirmation

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
- Only `09_Decisions/SecurityDefinerAndFunctionGrantHardeningDecision.md` was created/modified.

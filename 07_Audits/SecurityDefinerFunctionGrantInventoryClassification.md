# Security Definer Function Grant Inventory Classification

## 1. Metadata

- Status: Draft
- Version: 0.1
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: PP-01 metadata evidence, owner-review gate, committed decision records, and patch plan
- canonical: false
- Inventory status: Preliminary; incomplete until sanitized function inventory is reviewed
- Classification status: Preliminary
- Implementation status: Not authorized
- Production mutation status: Not executed
- Legal status: Engineering security audit/classification only; not legal advice

## 2. Purpose

This artifact classifies the known SECURITY DEFINER / function EXECUTE hardening candidates and defines the sanitized inventory model required before any implementation prompt.

- This is not implementation.
- This does not authorize SQL.
- This does not authorize migration creation.
- This does not authorize production access.
- This does not authorize Supabase CLI.
- This does not authorize source changes.
- This does not authorize function/RPC invocation.

## 3. Evidence Boundary

This artifact is based only on sanitized PP-01 metadata evidence, committed handbook decisions, committed patch plan, and existing audit material.

No new production access, SQL, CLI, dashboard action, source inspection, private data inspection, storage object listing, build, test, dependency install, migration, verification query, RPC/function invocation, or implementation was performed.

No secrets or private data are included.

## 4. Inventory Status

This is a preliminary inventory/classification only.

Complete function inventory is not available inside this artifact.

Current artifact classifies known candidate functions and missing evidence.

Exact function-by-function implementation action remains blocked.

Owner-approved implementation prompt is still required later.

## 5. Reviewed Inputs

- `00_Status/SecurityDefinerFunctionGrantHardeningOwnerReviewGate.md`
- `08_PatchPlans/SecurityDefinerAndFunctionGrantHardeningPatchPlan.md`
- `09_Decisions/SecurityDefinerAndFunctionGrantHardeningDecision.md`
- `09_Decisions/RLSPolicyAndGrantMatrixClassification.md`
- `09_Decisions/SupabaseMigrationSourceOfTruthDecision.md`
- `09_Decisions/RLSDisabledRelationTriageDecision.md`
- `09_Decisions/StorageBucketExposureDecision.md`
- `09_Decisions/EdgeFunctionDeploymentInventoryDecision.md`
- `07_Audits/SupabaseFocusedBackendFollowUpReport.md`
- `07_Audits/OpsAdminSupportToolsAuthorityAudit.md`
- `07_Audits/EventLifecycleContractAudit.md`
- `07_Audits/DirectDataAccessRlsRelianceAudit.md`

## 6. PP-01 Evidence Summary

Known PP-01 evidence and follow-up records reported:

- Verifier table/storage direct access closed.
- Verifier had 0 effective table privilege rows for public/auth/storage/realtime.
- Storage schema usage/select false.
- No private rows inspected.
- No storage objects listed.
- No functions invoked.
- No mutation executed.
- Verifier revoked / NOLOGIN confirmed.
- Broad function EXECUTE grants observed by aggregate count.
- Verifier had public function execute exposure and security_definer execute exposure.
- Missing proconfig candidate list exists.
- No exploitability claim.
- No implementation authorized.

## 7. Classification Model

| Classification dimension | Meaning | Required before implementation |
|---|---|---|
| ownership/source | Whether the function is app-owned, platform-managed, legacy, or unknown. | Owner/source classification for every candidate function. |
| schema | Schema containing the function. | Sanitized schema classification without private data. |
| security mode | SECURITY DEFINER, SECURITY INVOKER, or unknown. | Confirmed mode for each candidate. |
| proconfig/search_path state | Whether explicit proconfig/search_path is present. | Confirmed current state and target standard. |
| current EXECUTE exposure | Current anon/authenticated/service/internal/PUBLIC exposure. | Role-by-role exposure classification. |
| intended caller role | Intended caller class after hardening. | Target caller role and justification. |
| app/dashboard/mobile/web dependency | Product surface that calls or depends on the function. | Dependency classification before any revoke/restrict. |
| Edge Function dependency | Whether deployed Edge Functions call or depend on the function. | Cross-reference Edge inventory before implementation. |
| storage/RLS dependency | Whether the function depends on storage or table/RLS behavior. | Cross-reference storage and RLS/grant decisions. |
| authority gate / body behavior | Internal permission checks, ownership checks, or authority gates. | Body/gate review before mutation. |
| risk class | P0/P1/P2/Unknown classification. | Owner-accepted risk class. |
| proposed future action | Preserve, restrict, harden, defer, exclude, or accept. | Action classification with owner approval. |
| rollback requirement | Reversal needs for grants/proconfig changes. | Rollback notes before implementation prompt. |
| verification requirement | Metadata and smoke/negative test needs. | Verification plan before implementation prompt. |

## 8. Candidate Function List

- `control_cancel_event(event_id uuid)`
- `control_end_event(event_id uuid)`
- `control_open_checkin(event_id uuid)`
- `delete_personal_reminder(p_id uuid)`
- `list_active_reminders()`
- `list_personal_reminders()`
- `publish_event(p_event_id uuid, p_visibility text)`
- `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])`
- `upsert_personal_reminder(p_id uuid, p_title text, p_note text, p_reminder_date date, p_color_theme text, p_remind_before_days integer)`

These are hardening candidates only.

They are not proven exploitable.

They are not approved for mutation.

They require dependency and authority classification first.

## 9. Function-by-Function Preliminary Classification Table

| Function | Domain | Known concern | Preliminary risk | Dependency class | Missing evidence | Proposed future action | Rollback required | Verification required | Implementation authorized? |
|---|---|---|---|---|---|---|---|---|---|
| `control_cancel_event(event_id uuid)` | Event lifecycle / host authority | Missing proconfig/search_path candidate; possible lifecycle mutation authority. | P0-P1 candidate | Dependency classification required | Owner/source, exact exposure, body/gate behavior, app/dashboard call sites, rollback design. | Defer pending dependency and authority classification; possible future proconfig hardening. | Yes | Metadata and event lifecycle smoke/negative tests after approval. | No |
| `control_end_event(event_id uuid)` | Event lifecycle / host authority | Missing proconfig/search_path candidate; possible lifecycle mutation authority. | P0-P1 candidate | Dependency classification required | Owner/source, exact exposure, body/gate behavior, app/dashboard call sites, rollback design. | Defer pending dependency and authority classification; possible future proconfig hardening. | Yes | Metadata and event lifecycle smoke/negative tests after approval. | No |
| `control_open_checkin(event_id uuid)` | Event lifecycle / host authority | Missing proconfig/search_path candidate; possible check-in state mutation authority. | P0-P1 candidate | Dependency classification required | Owner/source, exact exposure, body/gate behavior, host/staff/check-in dependencies, rollback design. | Defer pending dependency and authority classification; possible future proconfig hardening. | Yes | Metadata and check-in smoke/negative tests after approval. | No |
| `delete_personal_reminder(p_id uuid)` | Notification/reminder/privacy | Missing proconfig/search_path candidate; possible user-owned reminder mutation. | P1 or Unknown candidate | Dependency classification required | Owner/source, exact exposure, body/gate behavior, notification/reminder dependencies, rollback design. | Defer pending dependency and authority classification; possible future proconfig hardening. | Yes | Metadata and reminder delete smoke/negative tests after approval. | No |
| `list_active_reminders()` | Notification/reminder/privacy | Missing proconfig/search_path candidate; possible reminder visibility/read path. | P1 or Unknown candidate | Dependency classification required | Owner/source, exact exposure, body/gate behavior, caller role, privacy boundary, rollback design. | Defer pending dependency and authority classification; possible future proconfig hardening. | Yes | Metadata and reminder list smoke/negative tests after approval. | No |
| `list_personal_reminders()` | Notification/reminder/privacy | Missing proconfig/search_path candidate; possible user-owned reminder read path. | P1 or Unknown candidate | Dependency classification required | Owner/source, exact exposure, body/gate behavior, caller role, privacy boundary, rollback design. | Defer pending dependency and authority classification; possible future proconfig hardening. | Yes | Metadata and reminder list smoke/negative tests after approval. | No |
| `publish_event(p_event_id uuid, p_visibility text)` | Event lifecycle / host authority / visibility | Missing proconfig/search_path candidate; possible public visibility mutation. | P0-P1 candidate | Dependency classification required | Owner/source, exact exposure, body/gate behavior, publish/visibility call sites, rollback design. | Defer pending dependency and authority classification; possible future proconfig hardening. | Yes | Metadata and publish/visibility smoke/negative tests after approval. | No |
| `publish_event_with_groups(p_event_id uuid, p_visibility text, p_group_ids uuid[])` | Event lifecycle / host authority / visibility | Missing proconfig/search_path candidate; possible group/public visibility mutation. | P0-P1 candidate | Dependency classification required | Owner/source, exact exposure, body/gate behavior, group visibility dependencies, rollback design. | Defer pending dependency and authority classification; possible future proconfig hardening. | Yes | Metadata and publish/group visibility smoke/negative tests after approval. | No |
| `upsert_personal_reminder(p_id uuid, p_title text, p_note text, p_reminder_date date, p_color_theme text, p_remind_before_days integer)` | Notification/reminder/privacy | Missing proconfig/search_path candidate; possible user-owned reminder mutation. | P1 or Unknown candidate | Dependency classification required | Owner/source, exact exposure, body/gate behavior, notification/reminder dependencies, rollback design. | Defer pending dependency and authority classification; possible future proconfig hardening. | Yes | Metadata and reminder upsert smoke/negative tests after approval. | No |

## 10. Domain Grouping

### Event lifecycle / host control functions

`control_cancel_event`, `control_end_event`, and `control_open_checkin` matter because they may affect event lifecycle mutation, host authority boundaries, staff/check-in behavior, and rollback/smoke test requirements.

### Reminder / personal notification functions

`delete_personal_reminder`, `list_active_reminders`, `list_personal_reminders`, and `upsert_personal_reminder` matter because they may touch privacy-sensitive user-owned reminder or notification data and require caller, ownership, rollback, and smoke test classification.

### Publish / visibility functions

`publish_event` and `publish_event_with_groups` matter because they may alter visibility state, public/group exposure, host authority boundaries, and release-critical lifecycle behavior.

## 11. Missing Evidence

- Exact function owner/source.
- Exact schema.
- Exact security mode confirmation.
- Exact proconfig/search_path state.
- Current anon EXECUTE exposure per function.
- Current authenticated EXECUTE exposure per function.
- Current service_role/internal exposure per function.
- PUBLIC/inherited exposure per function.
- Function body/gate behavior.
- App/dashboard/mobile/web call sites.
- Edge Function call sites.
- Storage/RLS dependencies.
- Intended target caller role.
- Rollback SQL design.
- Post-patch verification query design.
- Manual smoke test scope.

## 12. Risk Class

| Risk | Meaning | Current handling |
|---|---|---|
| P0 | Security-impacting or authority-boundary item that blocks safe implementation sequencing or launch-ready claims. | Resolve, patch, or explicitly accept/defer with owner approval before release-readiness claim. |
| P1 | Important hardening item that should be addressed before public launch or broad beta. | Resolve, patch, or explicitly accept/defer with owner approval. |
| P2 | Follow-up hardening, documentation, monitoring, or lower-risk verification item. | Track after P0/P1 decisions; do not bundle without reason. |
| Unknown | Evidence is insufficient to classify risk. | Classify before implementation; Unknown security-impacting function exposure blocks launch-ready claims. |

Unknown security-impacting function exposure blocks launch-ready claims.

P0/P1 must be resolved, patched, or explicitly accepted/deferred by owner before release-readiness claim.

## 13. Dependency Class

| Dependency class | Meaning | Handling |
|---|---|---|
| App critical path | Function is required for primary mobile/web app behavior. | Must map call sites and smoke tests before any grant restriction. |
| Dashboard critical path | Function is required for dashboard or operational UI behavior. | Must map dashboard dependency and authority expectations. |
| Host/admin lifecycle path | Function affects event lifecycle, host, staff, admin, or support operations. | Require authority gate and rollback classification. |
| User privacy path | Function reads or mutates user-owned/private data. | Require privacy boundary and negative tests. |
| Notification/reminder path | Function affects reminders, notifications, or delivery-adjacent state. | Require notification/privacy smoke tests and payload boundary review. |
| Edge/storage/RLS dependent path | Function is called by Edge, touches storage, or relies on RLS/table state. | Cross-reference Edge, storage, and RLS decisions. |
| Deprecated/legacy path | Function is no longer expected to serve active flows. | Confirm dependency before restriction/removal; no destructive action here. |
| Unknown dependency | Dependency is not yet classified. | Defer implementation and block launch-ready claim if security-impacting. |

## 14. Proposed Future Action

| Future action | Meaning | Authorization status |
|---|---|---|
| preserve explicit intended grant | Keep intended function execute access for an accepted caller role. | Not authorized yet. |
| restrict accidental anon execute | Remove or narrow unintended unauthenticated execute exposure. | Not authorized yet. |
| restrict accidental authenticated execute | Remove or narrow unintended authenticated execute exposure. | Not authorized yet. |
| restrict PUBLIC/inherited execute | Replace inherited broad exposure with explicit grants. | Not authorized yet. |
| add explicit proconfig/search_path | Harden app-owned SECURITY DEFINER function configuration. | Not authorized yet. |
| defer pending dependency review | Delay action until call sites, gates, and ownership are classified. | Not authorized yet. |
| exclude Supabase-managed/platform function | Exclude platform-managed functions from app-owned hardening scope. | Not authorized yet. |
| no-op / documented acceptance | Accept current posture with documented owner rationale. | Not authorized yet. |

## 15. Rollback Requirement

Every future grant revoke must have reversible grant restoration notes.

Every future proconfig/search_path change must have rollback notes.

Rollback must not expose secrets or private data.

No destructive function drop is allowed in the first patch.

Rollback design is required before implementation prompt.

## 16. Verification Requirement

- Metadata-only verification where possible.
- Verify intended grants after patch.
- Verify revoked grants after patch.
- Verify SECURITY DEFINER proconfig/search_path after patch.
- Anon negative tests.
- Authenticated positive tests.
- Authenticated non-owner/non-authorized negative tests.
- Host/staff/admin smoke tests where relevant.
- Event publish/control/check-in smoke tests if affected.
- Reminder list/upsert/delete smoke tests if affected.
- Rollback verification.

Verification is not authorized by this artifact.

## 17. Launch Readiness Impact

This classification does not make the platform launch-ready.

Unknown function grant/proconfig exposure remains a launch-readiness blocker until classified, patched, or explicitly accepted/deferred by owner.

No production safe/unsafe final claim is made.

## 18. Implementation Authorization Status

Implementation remains not authorized.

No SQL, migration, source change, grant change, function change, proconfig change, production mutation, Supabase CLI action, dashboard action, verification query, RPC invocation, or deployment action is authorized by this inventory/classification artifact.

A separate owner-approved implementation prompt is required after this artifact and rollback plan are reviewed.

## 19. Explicitly Blocked Claims

- Do not claim exploitability.
- Do not claim production safe.
- Do not claim production unsafe as final conclusion.
- Do not claim launch-ready.
- Do not claim legal compliance.
- Do not claim security hardened.
- Do not claim function grants fixed.
- Do not claim implementation authorized.
- Do not claim all RPC/function risk is resolved.

## 20. No-Modification Confirmation

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
- Only `07_Audits/SecurityDefinerFunctionGrantInventoryClassification.md` was created/modified.

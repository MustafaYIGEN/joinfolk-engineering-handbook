# Diagnostics / Observability / Audit Log Contract Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false

## 2. Purpose

This audit maps JoinFolk diagnostics, observability, telemetry, and audit-log surfaces. It separates debugging evidence from authority enforcement, client-written telemetry from backend audit evidence, and support/admin visibility from mutation authority.

This report is not a patch plan, cleanup plan, migration plan, or implementation plan. No backend, Supabase, application, dashboard, mobile, or web code changes are authorized by this audit.

## 3. Audit Scope

In scope:

- Client diagnostics and remote telemetry.
- `app_diagnostics` evidence and RLS reliance.
- Host identity transfer audit surfaces.
- Ops/admin action traceability.
- Manual override auditability.
- Notification, push, reminder, moderation, commerce, lifecycle, and messaging logging evidence.
- Support/admin diagnostic visibility.
- Edge Function logging versus deployment evidence.
- Payload privacy, retention, redaction, and audit boundary questions.

Read-only source inspection covered handbook reports and targeted local source references in `C:\dev\joinfolk-engineering-handbook`, `C:\dev\hostos`, `C:\dev\joinfolk-web`, and `C:\dev\hostos\apps\mobile`.

Out of scope:

- Creating migrations or backend patches.
- Verifying production state directly.
- Writing database statements.
- Running Supabase CLI, builds, tests, installs, or production connections.

## 4. Diagnostics / Observability / Audit Log Contract Summary

Observed diagnostics and auditability are partial and split across client diagnostics, transfer audit surfaces, local Edge Function source, and product records that can act as partial operational evidence.

Current high-confidence observations:

- Mobile remote diagnostics writes runtime diagnostic events to `app_diagnostics` from client-side code. These records are useful for debugging but low-trust for security or compliance conclusions unless paired with backend evidence.
- Transfer tooling references `host_transfer_audit_log`, transfer rows with audit-style fields, and prior production evidence for `_transfer_audit`.
- Ops transfer actions are RPC-mediated for sensitive mutations, while some transfer and audit display reads are direct table reads under assumed ops RLS.
- Manual tier/event override flows appear to involve external support/admin process guidance rather than in-app mutation authority.
- Notification and push logs exist as local/source and prior audit evidence, including `log_push_sent_v1`, but push dispatch remains tied to local Edge Function source without confirmed deployment.
- No dedicated diagnostics dashboard page was confirmed in focused ops route inspection.

Canonical contract expectation:

- Diagnostics support debugging and operational awareness, not permission checks.
- Audit logs support traceability, not authority enforcement.
- Client telemetry is low-trust.
- Server-side admin mutations should be attributable, scoped, and auditable.
- Private payloads must be minimized, access-controlled, and retention-defined.
- Edge Function local source is not production observability evidence unless deployment is verified.

## 5. Diagnostics and Audit Surface Inventory Matrix

| Surface / domain | Diagnostic/audit action or visibility exposed | Access path observed | Expected authority owner | Scope | Production evidence status | Risk class | Recommendation |
|---|---|---|---|---|---|---|---|
| Mobile remote diagnostics | Runtime diagnostic event capture | Direct table insert candidate | Backend/RPC/RLS/auth + mobile client | Authenticated / possible anon diagnostic path | Local source + prior handbook evidence; production RLS not fully covered | Privacy-sensitive; operational/admin-sensitive | Verify RLS/policies; verify payload minimization |
| `app_diagnostics` | Telemetry table for client diagnostics | Direct table insert | Backend/RPC/RLS/auth | Client write; support/admin read unknown | Not covered by supplied production evidence | Privacy-sensitive; compliance/audit-sensitive | Document contract; verify RLS/policies |
| Client console/log helpers | Local debug output | Console/client log | Mobile/Web UI | Client only | Local source only | Product correctness; UX-only | Preserve as non-authoritative |
| Host transfer audit | Transfer traceability | Direct read + backend audit helper evidence | Ops/admin RPC + RLS | Ops/admin | `_transfer_audit` known from prior production evidence; policy completeness unknown | Compliance/audit-sensitive; operational/admin-sensitive | Verify append-only semantics |
| `host_transfer_audit_log` | Transfer audit display | Direct table read candidate | Backend/RPC/RLS/auth | Ops/admin | Local source/report evidence; production policy not fully covered | Privacy-sensitive; compliance/audit-sensitive | Verify RLS/policies; document contract |
| `host_identity_transfers` | Transfer state plus audit-style fields | Direct read + RPC mutation | Ops/admin RPC | Ops/admin | Production transfer RPC evidence exists; table policy completeness unknown | Operational/admin-sensitive | Preserve; add auditability review later |
| Ops transfer mutations | Approve/reject/execute transfer | SECURITY DEFINER RPC candidates | Ops/admin RPC | Ops/admin | `admin_execute_host_identity_transfer_v1` production evidence exists | Security-sensitive; compliance/audit-sensitive | Verify grants/internal gates |
| Manual tier/event overrides | External support/admin process guidance | Manual process outside app | Support/admin process | Ops/admin | Local dashboard evidence only | Operational/admin-sensitive; compliance/audit-sensitive | Add process auditability later |
| Ops media drafts | Media draft review/state operations | Direct table access candidate | Backend/RPC/RLS/auth | Ops/admin | Local source evidence; production policy unknown | Privacy-sensitive; operational/admin-sensitive | Verify RLS/policies; add auditability later |
| Media moderation | Host moderation actions | RPC-mediated mutation candidate | Backend/RPC/RLS/auth | Host/staff/ops | Prior function/source evidence; policy correctness unknown | Privacy-sensitive; product correctness | Document moderation log contract |
| Notification records | Notification state and partial observability | RPC/direct mixed by domain | Backend/RPC/RLS/auth | User owner / system | `notifications_v2` RLS confirmed at high level | Privacy-sensitive | Verify payload policy |
| Push delivery logs | Sent/dead-token/throttle evidence | RPC + Edge Function local source | Backend/RPC/RLS/auth + Edge deployment | Internal/service | Local source; deployment unconfirmed | Privacy-sensitive; operational/admin-sensitive | Verify Edge Function deployment |
| Reminder records | Reminder listing/timing state | RPC-mediated candidate | Backend/RPC/RLS/auth | User owner | Production RLS not fully covered | Privacy-sensitive; product correctness | Verify RLS and RPC configuration |
| Commerce/order records | Order/payment/ticket operational evidence | RPC/direct mixed by domain | Backend/RPC/RLS/auth | User/host/ops | `commerce_orders` deny-all style policy evidence exists | Revenue-sensitive; compliance/audit-sensitive | Document audit contract |
| Ticket/reservation records | Acquisition and attendance evidence | RPC/direct mixed by domain | Backend/RPC/RLS/auth | User/host/staff/ops | Tickets/reservations RLS evidence exists; ticket direct policies zero in prior evidence | Revenue-sensitive; security-sensitive | Prefer RPC authority |
| Lifecycle/control actions | Event state transition traceability | RPC-mediated mutation expected | Backend/RPC/RLS/auth | Host/ops | Production body evidence incomplete | Operational/admin-sensitive | Add auditability review later |
| Messaging/private communication | Private message/report traceability | Unknown / Needs verification | Backend/RPC/RLS/auth | Conversation member/support | Production evidence not fully covered | Privacy-sensitive | Needs verification |
| Support diagnostic visibility | Reading diagnostics/audit records | Unknown / Needs verification | Support/admin process + RLS | Support/ops/admin | No dedicated diagnostics dashboard confirmed | Privacy-sensitive; operational/admin-sensitive | Needs product decision |
| Edge Function logs | Local operational logging in function source | Edge Function local source only | Edge Function deployment | Internal/service | No deployed Edge Functions visible in Dashboard based on manual confirmation | Operational/admin-sensitive | Do not treat as production evidence |

## 6. Role Vocabulary and Authority Boundary

- User/client: an app or web user whose device can produce client-side diagnostics. Client-supplied telemetry is low-trust.
- Host: product role that can manage owned events or host surfaces, but is not ops/admin.
- Staff: event-scoped operational role. Staff status does not imply host or ops authority.
- Ops/admin: internal authority role for sensitive operational actions such as host identity transfer or support operations.
- Support: possible read-oriented operational role. Support read visibility is not mutation authority.
- Service role: infrastructure authority used by backend or deployed functions. It is not ordinary user authority.
- Public/authenticated user: product access states, not operational audit-reader roles.
- Backend/RPC: expected authority owner for privileged writes, audit creation, and sensitive reads.
- Edge Function: deployment surface separate from local source folders.
- Audit reader: any support/ops/admin actor allowed to view audit or diagnostic records.

Diagnostics are not authority. Audit logs are evidence and traceability, not permission checks. Dashboard route guards and UI labels are not backend authority. Client logs can support debugging, but they cannot prove security-sensitive user action without server-side evidence.

## 7. Client Diagnostics Assessment

Mobile source evidence includes remote diagnostics helpers and calls from host-facing mobile screens. The observed pattern is client-side diagnostic capture with direct insertion into `app_diagnostics`.

Assessment:

- Client-written diagnostics are useful for runtime debugging, reproducing issues, and operational triage.
- Client diagnostics should not be treated as proof that a user performed a sensitive action.
- Client diagnostics should not include secrets, tokens, payment data, private message bodies, or unrestricted private event/media data.
- If anon or broad diagnostic insert paths exist, the canonical contract must define accepted payload shape, rate limits, abuse handling, and read access.

Current classification: Product-critical for observability, privacy-sensitive, backend authority unclear for policy correctness, and Unknown / Needs verification for production RLS behavior.

## 8. App Diagnostics Table / RLS Assessment

`app_diagnostics` is the central observed table candidate for client diagnostics. Prior audits flagged direct mobile inserts and local comments referencing an anon whitelist.

Known evidence:

- Direct mobile insert evidence exists from local source inspection.
- Prior source inventory referenced app diagnostics migrations and policy-related files.
- Supplied production RLS evidence did not fully cover `app_diagnostics`.

Contract expectation:

- Write authority should be explicitly scoped by auth state, payload type, and abuse controls.
- Read authority should be restricted to explicit support/ops/admin contracts.
- Payload content should be minimized and retention-defined.
- Diagnostics should be separate from canonical audit logs.

Status: Unknown / Needs verification for production RLS and policy correctness.

## 9. Transfer Audit Log Assessment

Transfer audit evidence is stronger than most diagnostics domains because transfer tooling and prior production reports reference dedicated audit surfaces.

Observed surfaces:

- `host_transfer_audit_log` as a dashboard transfer audit read surface.
- Transfer rows with audit-style JSON fields and lifecycle/status metadata.
- Prior production evidence for `_transfer_audit`.
- `admin_execute_host_identity_transfer_v1` production evidence with SECURITY DEFINER, `search_path=public`, and an `auth_is_ops()` internal gate.

Expected clean transfer audit contract:

- Every transfer mutation records actor, action, target, timestamp, outcome, and relevant non-secret details.
- Audit entries are append-only or tamper-resistant where product/compliance requires.
- Audit reads are ops/admin scoped and not public, host, staff, or support-visible unless explicitly accepted.
- Audit details distinguish source persona, target persona, and personal identity preservation.

Current risk: compliance/audit-sensitive and operational/admin-sensitive. Production function evidence exists, but full table policy, retention, and immutability evidence remains Unknown / Needs verification.

## 10. Ops/Admin Action Traceability Assessment

Ops/admin actions should be traceable when they affect identity, account tier, event lifecycle, tickets, reservations, media, private data, notifications, or diagnostic visibility.

Observed traceability posture:

- Host identity transfer has explicit audit surfaces.
- Ops media draft operations appear direct-table/RLS-dependent from prior ops audit evidence; dedicated audit traceability remains Unknown / Needs verification.
- Tier/event manual override guidance appears to point to external process rather than in-app mutation; auditability is therefore process-dependent.
- Support reads of private diagnostics or private data were not confirmed as audited.
- Notification/push logging exists as local/source evidence, but deployed delivery observability is not confirmed.

Recommendation: document an ops/admin action traceability contract before patching. Do not treat existing logs as complete without production verification.

## 11. Manual Override Auditability Assessment

Prior ops/admin audit evidence found dashboard tier/event surfaces that guide manual external override operations rather than executing in-app mutations.

Contract implications:

- External/manual operations are not app authority, but they still require process-level traceability.
- Manual overrides should identify actor, target, reason, approver if any, timestamp, before/after state, and rollback path where product requires.
- External process logs must not be confused with database audit logs unless explicitly integrated.

Status: Operational/admin-sensitive and compliance/audit-sensitive. Current evidence is local-source-only and process semantics are Unknown / Needs verification.

## 12. Notification / Push / Reminder Logging Assessment

Notification and push systems provide partial observability but are not a complete audit trail by default.

Observed evidence:

- `notifications_v2` RLS was confirmed at a high level in prior production evidence.
- `push_tokens_v1` RLS was confirmed at a high level.
- `log_push_sent_v1` appears in local/source and prior audit evidence.
- Local `push-dispatch` source was observed in prior audits, but no deployed Edge Functions were visible in Dashboard based on manual confirmation.
- Reminder RPCs had prior configuration/search-path concerns; reminder table production RLS was not fully covered.

Contract expectation:

- Notification records are product data, not full delivery proof unless explicitly designed that way.
- Push delivery logs should avoid push token leakage and private payload leakage.
- Reminder logs should separate scheduling/listing from notification or push dispatch.
- Push delivery observability depends on deployed runtime evidence, not local function folders.

Status: Mostly backend/RPC-oriented in concept, but deployment and payload correctness remain Unknown / Needs verification.

## 13. Media / Moderation Logging Assessment

Media moderation is privacy-sensitive because media can contain participants, private event context, uploader identity, and public/private exposure decisions.

Observed evidence:

- `host_moderate_media_v1` exists in local/source and prior audit evidence.
- `event_media` RLS was confirmed enabled at a high level in prior production evidence, but policy correctness needs deeper review.
- Ops media draft tooling appears to use direct table access with RLS assumptions.

Expected clean contract:

- Host/staff/ops moderation actions are attributable.
- Hide, unhide, delete, public-highlight, and rejection decisions preserve enough audit context for support review.
- Audit records avoid exposing raw private media URLs or sensitive metadata to unauthorized readers.

Status: Product-critical and privacy-sensitive. Dedicated moderation audit completeness is Unknown / Needs verification.

## 14. Commerce / Ticket / Reservation Logging Assessment

Commerce, ticket, claim, transfer, and reservation actions are revenue-sensitive and should be auditable through backend-authoritative mutations.

Known evidence from prior audits:

- `commerce_orders` had deny-all style authenticated policy evidence.
- `tickets` and `event_ticket_claims_v1` had RLS enabled with zero direct policies, making RPC/default-deny assumptions critical.
- Reservation and ticket RLS evidence exists at a high level, but policy correctness varies.
- Purchase/order/ticket/reservation RPC surfaces are split across versions and domains.

Expected clean contract:

- Order creation, payment confirmation, ticket issuance, ticket status mutation, claim/transfer, reservation approval/rejection/cancel, and stale order expiry should produce attributable backend evidence.
- Audit logging should distinguish product state from operational support state.
- Payment/refund/dispute logs should not be invented unless found and verified.

Status: Revenue-sensitive. Audit completeness remains Unknown / Needs verification.

## 15. Event Lifecycle / Control Logging Assessment

Lifecycle and control actions include publish, go-live, end, archive, cancel, check-in open/close, and related administrative state transitions.

Observed posture:

- Event lifecycle audits identified transition RPCs as expected authority.
- Dashboard controls are UI surfaces, not authority.
- Production body and audit completeness for lifecycle/control functions remain incomplete in supplied evidence.

Expected clean contract:

- Every lifecycle transition should record actor, event, previous state, next state, timestamp, and reason or source where relevant.
- Lifecycle audit records should remain separate from feed/search visibility and notification side effects.
- Logs should not replace backend transition checks.

Status: Operational/admin-sensitive and product correctness risk. Auditability is Unknown / Needs verification.

## 16. Messaging / Private Communication Logging Assessment

Messaging audit evidence did not confirm a support/admin private conversation viewer. Private communication remains high sensitivity if logs, reports, notifications, or support tooling reference message content.

Contract expectation:

- Private message bodies should not be logged unless explicitly accepted by product/security policy.
- Abuse/report logs, if present, should be privacy-scoped, support/admin-gated, and attributable.
- Notification previews and deep links should not leak message content to non-members.
- Support/admin visibility into private conversations, if ever introduced, must be explicit, gated, and auditable.

Status: Unknown / Needs verification. Do not claim support/admin DM visibility exists without direct evidence.

## 17. Support / Ops Diagnostic Visibility Assessment

Focused prior ops route inspection did not confirm a dedicated diagnostics dashboard page.

Known or likely visibility surfaces:

- Ops transfer audit display.
- Ops media draft management.
- Ops user/tier/event inspection pages.
- Potential diagnostic data in `app_diagnostics`, but read surface was not confirmed.
- Notification/push/reminder logs may be support-relevant but no complete support viewer was confirmed.

Contract expectation:

- Diagnostic/audit read access is itself sensitive.
- Support read access should be scoped, attributable, and separated from mutation authority.
- Ordinary host, staff, user, public, or social surfaces must not inherit diagnostic/audit visibility.

Status: Unknown / Needs verification for support diagnostic readers.

## 18. Edge Function Logging / Deployment Boundary Assessment

Local Edge Function source can include logging behavior, notification dispatch, email dispatch, transfer notifications, or operational traces. It is not production evidence by itself.

Known context:

- Local Edge Function folders exist in some Supabase trees.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Database Functions / RPC evidence is separate from Edge Function deployment evidence.

Contract expectation:

- Edge Function logs are active observability only if deployment is confirmed.
- Local logs, local dispatch code, and source comments should not be treated as production audit coverage.
- If Edge Functions later become deployed authority, logging, payload minimization, and service role use need separate verification.

Status: Unknown / Needs verification for production deployment.

## 19. Payload Privacy / PII / Sensitive Data Boundary

Sensitive payload categories include:

- User IDs and actor/target IDs.
- Profile and persona fields.
- Private event IDs, titles, venue/location details, or group/private metadata.
- Ticket, order, claim, reservation, and payment-like state.
- Push tokens and device identifiers.
- Message content or private conversation metadata.
- Media URLs, storage paths, signed URLs, and uploader identity.
- Device data, crash traces, app state, screen names, and route parameters.
- Location/geodata where present.
- IP address and user-agent where present.
- Auth/session traces and secret-adjacent values.

No secrets were inspected. Logs should never include service role keys, JWT secrets, API keys, private keys, tokens, credentials, or secret files. Audit payloads should be minimized to what is necessary for traceability.

## 20. Retention / Deletion / Redaction Assessment

Retention, deletion, redaction, and archival behavior for diagnostics and audit logs was not confirmed in the supplied evidence.

Contract expectation:

- Product data deletion and audit retention may have different requirements.
- Diagnostics retention should be time-bounded unless explicitly accepted.
- Audit retention should match compliance/support needs and be access-controlled.
- Redaction should preserve traceability while removing unnecessary private payload.

Status: Unknown / Needs verification.

## 21. Observability vs Authority Enforcement Boundary

Diagnostics and logs are supporting evidence only.

Canonical boundary:

- A diagnostic record does not grant access.
- An audit record does not authorize the underlying action.
- Client logs cannot prove server-side authority.
- Server-side audit logs are stronger evidence but still require access control, immutability, and retention verification.
- Dashboard observability cannot replace backend authorization checks.
- Product records such as notifications or orders can provide partial operational history, but only explicit audit design can define them as audit logs.

## 22. Dashboard / Ops Observability Surface Map

Observed or prior-audit dashboard/ops surfaces:

- `/ops/transfers`: transfer state, resolve/approve/reject/execute actions, and audit display evidence.
- `/ops/media-engine`: ops media draft management; audit completeness unknown.
- `/ops/users`: user/profile/tier support visibility; auditability unknown.
- `/ops/tier`: manual external tier override guidance; not confirmed as in-app mutation.
- `/ops/events`: event inspection/manual external override guidance; not confirmed as in-app mutation.
- Host/dashboard tools for event, ticket, reservation, media, and venue operations may expose operational state, but operational state views are not necessarily audit logs.

No dedicated diagnostics dashboard page was confirmed in focused ops route inspection.

## 23. Mobile / Web Diagnostics Surface Map

Mobile:

- Remote diagnostics helper writes client-side runtime events to `app_diagnostics`.
- Diagnostics helpers and console logs appear to support debugging.
- Release checklist references crash reports and analytics monitoring, but this is process evidence, not confirmed backend audit coverage.

Public web:

- No public diagnostics or audit visibility is expected.
- Public/share routes should not expose diagnostics, audit logs, or private support data.

Dashboard/web:

- Ops transfer audit display is the strongest observed web audit surface.
- Other ops pages provide operational inspection but not necessarily audit logging.

## 24. Backend RPC / RLS Authority Evidence Map

Prior handbook evidence only; no production connection was made.

- `app_diagnostics` production RLS/policy evidence was not fully covered.
- Transfer audit surfaces exist locally; production policy completeness for `host_transfer_audit_log` was not fully covered.
- `admin_execute_host_identity_transfer_v1` production evidence exists with SECURITY DEFINER, `search_path=public`, and an `auth_is_ops()` gate.
- `_transfer_audit` exists in prior production function evidence.
- `notifications_v2` and `push_tokens_v1` RLS were confirmed at a high level, but payload/logging correctness still needs review.
- Events, tickets, reservations, `commerce_orders`, `event_media`, venues, `venue_media`, and `event_staff_assignments` RLS evidence exists at a high level, but policy correctness varies by table.
- `tickets` and `event_ticket_claims_v1` had zero direct policies in prior evidence and likely depend on RPC/default-deny assumptions.
- `commerce_orders` had deny-all style authenticated policy evidence.
- DM/conversation/message production evidence was not fully covered.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Production SQL/RPC evidence remains stronger than local source assumptions.

Unreviewed diagnostic/audit tables, functions, and buckets must not be treated as safe.

## 25. Direct Data Access / RLS Reliance Map

| Data surface | Observed direct access or reliance | Authority concern | Evidence status | Recommendation |
|---|---|---|---|---|
| `app_diagnostics` | Mobile direct insert | Client write scope, payload minimization, support read scope | Local source + prior audit evidence; production policy unknown | Verify RLS/policies |
| `host_transfer_audit_log` | Direct read candidate in ops transfer UI | Audit log visibility and immutability | Local source/report evidence | Verify RLS and append-only semantics |
| `host_identity_transfers` | Direct read + RPC mutation | Transfer state visibility and mutation traceability | Local + production RPC evidence | Preserve RPC mutations; verify table policies |
| `ops_media_drafts` | Direct ops table access candidate | Ops-only visibility and moderation traceability | Prior ops audit evidence | Verify RLS/policies |
| `notifications_v2` | Product records and read/mark surfaces | Payload privacy and owner scope | RLS confirmed high level | Verify payload policy |
| `push_tokens_v1` | Token records and push eligibility | Token privacy and ownership | RLS confirmed high level | Verify owner scope and logging redaction |
| Reminders | RPC surfaces; table evidence incomplete | Privacy and timing | Production RLS not fully covered | Verify RPC/table authority |
| `event_media` | Media/moderation records | Private media metadata | RLS confirmed high level | Verify moderation/audit contract |
| `commerce_orders` | Order state | Revenue-sensitive support/audit evidence | Deny-all style policy evidence | Document audit expectations |
| `tickets` | Ticket ownership/status | Default-deny/RPC reliance | RLS enabled with zero direct policies in prior evidence | Preserve RPC authority |
| `reservations` | Reservation state | Revenue/product correctness | RLS confirmed high level | Verify auditability |
| `events` | Lifecycle/control state | Operational traceability | RLS confirmed high level | Verify transition logs |
| `profiles` / `user_profiles` | Identity/support visibility | Privacy-sensitive diagnostic context | Production evidence not fully covered | Verify RLS/policies |
| DM/conversation tables | Private communication logs if present | Highly privacy-sensitive | Production evidence not fully covered | Needs verification |

## 26. Duplicated / Split / Legacy Diagnostics Surfaces

| Surface / helper / RPC / table | Observed role | Current / legacy / unknown | Risk if still active or authoritative | Evidence type | Recommendation |
|---|---|---|---|---|---|
| Client console logs vs remote diagnostics | Local debugging versus remote telemetry | Current / unknown | Client logs may be mistaken for audit evidence | Local source | Document non-authority boundary |
| `app_diagnostics` local migrations/source references | Client diagnostics table | Current / unknown | Production RLS/payload assumptions may be unverified | Handbook + local source | Verify production policy |
| `host_transfer_audit_log` vs transfer row audit fields vs `_transfer_audit` | Transfer traceability | Split | Incomplete traceability or inconsistent audit readers | Handbook + local + prior production evidence | Reconcile audit contract |
| `notifications_v2` vs push sent logs | Product notification records versus delivery observability | Split | Notification records may be overread as delivery proof | Handbook + local source | Document semantics |
| `ops_media_drafts` state changes | Ops media workflow trace | Current / unknown | Status history may be insufficient for moderation audit | Local/prior audit evidence | Add auditability later |
| Manual override guidance | External process state changes | Current / unknown | App lacks full trace if external process is unaudited | Prior ops evidence | Define process audit |
| Local Edge Function logs | Potential dispatch/runtime logging | Unknown | Local source may be mistaken for production logging | Local source only | Verify deployment |
| Dashboard visual audit comments | Planned or conceptual audit logs | Unknown | Comments can be mistaken for implemented audit trail | Local source comments | Treat as non-authoritative |

## 27. Diagnostics-Audit-Critical Invariants

- Diagnostics are not authority enforcement.
- Audit logs are traceability evidence, not permission checks.
- Client-written diagnostics are low-trust.
- Server-side admin mutations should create attributable audit records.
- Audit logs should be append-only or tamper-resistant where product requires.
- Support read access to logs is itself sensitive.
- Logs must minimize private payloads and never include secrets.
- Manual operational overrides require external/process auditability.
- Revenue-sensitive actions require stronger auditability.
- Transfer execution is ops/admin-only and auditable.
- Push/notification logs do not replace delivery or authorization checks.
- Edge Function local source is not deployment evidence.
- Public, social, host, and staff surfaces do not inherit audit-log visibility.

## 28. Unknown / Needs Verification Surfaces

- Production RLS and policy correctness for `app_diagnostics`.
- Read visibility for diagnostics and audit logs.
- Retention, deletion, redaction, and archive behavior.
- Whether transfer audit logs are append-only or otherwise tamper-resistant.
- Whether all ops/admin mutations create audit records.
- Whether support reads of private data are audited.
- Whether manual external overrides have process-level audit trail.
- Whether lifecycle/control transitions record attributable audit entries.
- Whether commerce/ticket/reservation support actions have complete auditability.
- Whether media moderation status history is sufficient for audit needs.
- Whether private message reports or support workflows log sensitive content.
- Whether local Edge Function logging corresponds to deployed production behavior.

## 29. Diagnostics / Observability / Audit Log Gaps / Risk Register Seeds

| Gap ID | Domain | Current issue | Expected clean diagnostics/observability/audit-log contract | Risk | Priority candidate | Blocked by | Recommended next action |
|---|---|---|---|---|---|---|---|
| DOA-GAP-001 | App diagnostics | `app_diagnostics` direct client writes exist, but production RLS/payload/retention evidence is incomplete | Client diagnostics have scoped writes, restricted reads, minimized payloads, and retention rules | Privacy-sensitive; operational/admin-sensitive | Candidate P2 | Production policy verification | Document diagnostics table contract and verify policies |
| DOA-GAP-002 | Transfer audit | Transfer audit surfaces are split across audit table, row fields, and helper evidence | Transfer actions produce complete, attributable, tamper-resistant audit records | Compliance/audit-sensitive | Candidate P1 | Production audit table/policy review | Verify transfer audit completeness and append-only semantics |
| DOA-GAP-003 | Manual overrides | Tier/event manual override guidance implies external process outside app audit | Manual operational overrides have external/process audit with actor, target, reason, and outcome | Operational/admin-sensitive | Candidate P2 | Product/support process decision | Define manual override audit procedure |
| DOA-GAP-004 | Ops media drafts | Ops media draft state changes may lack dedicated audit trail | Moderation/admin media actions are attributable and reviewable | Privacy-sensitive; product correctness | Candidate P2 | Ops media source/policy review | Verify moderation traceability requirements |
| DOA-GAP-005 | Push delivery logs | `log_push_sent_v1` and local dispatch source exist, but deployment/log semantics are unclear | Push delivery observability is tied to confirmed deployed runtime and minimized payloads | Privacy-sensitive; operational/admin-sensitive | Candidate P2 | Edge Function deployment verification | Verify push dispatch deployment and log contract |
| DOA-GAP-006 | Commerce/tickets/reservations | Revenue-sensitive state changes have incomplete auditability evidence | Orders, tickets, claims, reservations, and payments have backend-authoritative audit records | Revenue-sensitive; compliance/audit-sensitive | Candidate P1 | Commerce RPC/body review | Audit revenue action traceability |
| DOA-GAP-007 | Lifecycle/control | Event lifecycle transition auditability is not fully proven | Lifecycle/control transitions record actor, previous state, next state, and timestamp | Operational/admin-sensitive | Candidate P2 | Lifecycle RPC review | Verify lifecycle transition logging |
| DOA-GAP-008 | Messaging/private communication | Private message/report logging behavior is unknown | Private communication logs avoid message bodies unless explicitly accepted and are support-gated | Privacy-sensitive | Unknown | Messaging schema/support review | Verify reporting and support log boundaries |
| DOA-GAP-009 | Edge Function observability | Local function logs may be mistaken for deployed production observability | Edge logs count only after deployment evidence exists | Operational/admin-sensitive | Unknown | Deployment confirmation | Separate local-source evidence from deployed runtime evidence |
| DOA-GAP-010 | Retention/redaction | Diagnostics and audit retention/redaction were not confirmed | Retention, deletion, and redaction are documented by data class | Privacy-sensitive; compliance/audit-sensitive | Candidate P2 | Product/privacy decision | Run privacy/data retention audit |

## 30. Product Decisions Required

- Which diagnostic payload fields are accepted from clients?
- Are anonymous diagnostic writes product-approved, and under what constraints?
- Who may read `app_diagnostics`?
- Which ops/admin support reads require audit logging?
- What transfer audit fields are mandatory?
- Are audit logs expected to be append-only or tamper-resistant?
- Which manual external overrides are allowed, and where is their audit trail kept?
- Which product records count as operational evidence versus formal audit logs?
- What is the retention period for diagnostics versus audit logs?
- Can logs include media URLs, private event metadata, ticket/order IDs, or message snippets?

## 31. Recommended Next Audits

1. Abuse / Reporting / Moderation Contract Audit.
2. Payments / Refunds / Disputes Operations Audit.
3. Privacy / Data Retention / Deletion Contract Audit.

These follow directly from unresolved moderation traceability, revenue auditability, and retention/redaction questions.

## 32. Non-Goals

- This audit does not authorize backend, RLS, RPC, storage, auth, or Edge Function changes.
- This audit does not create migrations or database statements.
- This audit does not verify production directly.
- This audit does not classify local Edge Function source as deployed production behavior.
- This audit does not claim diagnostics are unsafe solely because they exist.
- This audit does not claim RLS is correct solely because it is enabled.
- This audit does not claim audit logs are immutable unless evidence supports it.

## 33. Open Questions

- What exact `app_diagnostics` payload schema is accepted?
- Are diagnostic records readable by support, ops/admin only, user owner, or nobody through product UI?
- What retention policy applies to client diagnostics?
- Which admin/support reads should be audited?
- Is `host_transfer_audit_log` append-only in production?
- Are manual override workflows tracked outside the app?
- Do commerce/order/payment flows emit formal audit logs or only product state records?
- Do lifecycle transitions have attributable backend audit records?
- Are moderation actions logged separately from media state?
- Are private message reports logged without storing full private message bodies?

## 34. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `07_Audits/DiagnosticsObservabilityAuditLogContractAudit.md` was created/modified.

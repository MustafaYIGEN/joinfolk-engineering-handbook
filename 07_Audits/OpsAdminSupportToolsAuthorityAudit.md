# Ops / Admin / Support Tools Authority Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false

## 2. Purpose

This audit maps JoinFolk ops/admin/support authority across dashboard route guards, internal role checks, host identity transfer tooling, media engine drafts, tier/event lookup pages, support visibility, private-data boundaries, diagnostics, audit logs, SECURITY DEFINER RPCs, broad grants with internal gates, and local Edge Function deployment uncertainty.

This is an ops/admin/support tools authority audit only. It is not an implementation plan, patch plan, cleanup plan, migration plan, or authorization to modify backend/RPC/RLS/storage/auth behavior.

The audit separates:

- ops/admin authority from host authority
- support visibility from ops/admin mutation authority
- dashboard/admin UI guards from backend/RPC/RLS authority
- internal Database Functions / RPC authority from Edge Function deployment status
- private data visibility from public, social, host, and staff visibility
- read-only support access from mutation/admin actions
- operational tools from product user flows
- audit/log observability from authority enforcement
- production evidence from local-source-only evidence

## 3. Audit Scope

Read-only inspection covered handbook audit/architecture/decision documents and targeted local source searches under:

- `C:\dev\joinfolk-engineering-handbook`
- `C:\dev\hostos`
- `C:\dev\joinfolk-web`
- `C:\dev\hostos\apps\mobile`

Current system context preserved:

- Future accepted Supabase migration working target: `C:\dev\hostos\supabase\migrations`.
- This is not proof of historical sole canonical source.
- Split-source migration history remains unresolved.
- Production SQL/RPC evidence remains stronger than local source assumptions.
- Database Functions / RPC evidence is separate from Edge Function deployment evidence.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Local Edge Function source folders exist in some Supabase trees, but deployment status is not confirmed.
- No backend patch or migration is authorized by this audit.

Targeted inspection focused on:

- `joinfolk-web\dashboard` ops route guard and ops pages.
- Host identity transfer API wrapper and dashboard page.
- Ops media engine data access.
- User lookup, tier control, and event override pages.
- Auth profile loading for `is_ops`.
- Prior production/focused reports covering admin transfer RPCs, RLS evidence, SECURITY DEFINER functions, and broad grants.

## 4. Ops / Admin / Support Tools Authority Contract Summary

Observed ops/admin tooling is concentrated in `C:\dev\joinfolk-web\dashboard`:

- `/ops/users`
- `/ops/tier`
- `/ops/events`
- `/ops/media-engine`
- `/ops/transfers`

These routes are protected by `OpsGuard`, which checks an `is_ops` value loaded from the authenticated user's profile. This is a dashboard UI guard only. Backend authority must still be enforced by RPC gates and RLS policies.

Observed ops/admin actions split into two classes:

- Read-only or RLS-limited support visibility: user lookup, tier inspection, event inspection.
- Mutation/admin authority: host identity transfer RPCs and direct ops media draft CRUD guarded by RLS.

Production evidence confirms `admin_execute_host_identity_transfer_v1` exists, is SECURITY DEFINER, has `search_path=public`, includes an `auth_is_ops()` internal gate, and has broad execute grants. Broad grants are audit-significant but not exploitability proof when internal gates exist.

Clean contract expectation:

- Ops/admin authority is internally gated, backend-enforced, and auditable.
- Support read visibility does not imply mutation authority.
- Dashboard route guards are mirrors, not security controls.
- Private user, profile, event, ticket, message, media, diagnostic, and transfer data is visible only through explicit accepted support/admin contracts.

## 5. Ops/Admin Surface Inventory Matrix

| Surface / domain | Admin/support action or visibility exposed | Access path observed | Expected authority owner | Scope | Production evidence status | Risk class | Recommendation |
|---|---|---|---|---|---|---|---|
| Ops route guard | Allows `/ops/*` dashboard pages | UI guard reads profile `is_ops` | Backend/RPC/RLS must still enforce | Ops/admin | Local source only | Security-sensitive | Treat as UI mirror only |
| Auth profile ops flag | Loads tier and `is_ops` | Direct/profile API read | Backend/RLS/auth | Authenticated user | `profiles/user_profiles` policy not fully covered | Privacy/security-sensitive | Verify profile field exposure |
| Ops user lookup | Search by name and UUID; displays ID/name/tier | API/direct table reads with RLS limits | Backend/RLS/auth | Ops/support | Local source only | Privacy-sensitive | Document as best-effort support view |
| Ops tier control | Read-only tier inspection; manual external mutation guidance in UI | API/direct table reads | Support process, not app authority | Ops/support | Local source only | Operational/admin-sensitive | Do not treat as in-app mutation authority |
| Ops event override | Read-only event status/host/visibility inspection; manual external mutation guidance in UI | API/direct table reads | Support process, not app authority | Ops/support | Events RLS enabled; policy correctness incomplete | Operational/admin-sensitive | Keep lifecycle mutation backend-authoritative |
| Ops transfer page | List, resolve, approve, reject, execute host identity transfers | Direct transfer/audit reads plus ops/admin RPC mutations | Ops/admin RPC + RLS | Ops/admin | Production admin execute evidence exists | Privacy/admin-sensitive | Preserve RPC gate and auditability |
| Transfer audit log | Displays normalized audit entries per transfer | Direct `host_transfer_audit_log` read | Backend/RLS/auth | Ops/admin | Production existence partially evidenced via reports | Compliance/audit-sensitive | Verify audit completeness |
| Ops media engine | Editorial draft list/create/update/status workflow | Direct CRUD on `ops_media_drafts` with claimed ops-only RLS | Backend/RLS/auth | Ops/admin/editorial | Local/source-map evidence; production policy not covered | Operational/admin-sensitive | Verify RLS and audit semantics |
| Host media moderation | Host/dashboard moderation of event media | `host_moderate_media_v1` RPC | Backend/RPC/RLS/storage/auth | Host/staff/ops as accepted | `event_media` RLS exists; full body/policy review incomplete | Privacy-sensitive | Separate host moderation from ops moderation |
| Dashboard host messages | Host-persona DM inbox | DM RPCs | Backend/RPC/RLS/auth | Host conversation member | DM production evidence not fully covered | Privacy-sensitive | Not support/admin visibility |
| Public support page | Public support center/contact email | Static public web route | Public web UI | Public | Local source only | UX-only | Not ops/admin authority |
| Remote diagnostics | Runtime telemetry inserts | Direct `app_diagnostics` insert | Backend/RLS/auth or anon whitelist | Internal/support | Production evidence incomplete | Operational/privacy-sensitive | Dedicated diagnostics audit needed |
| Local Edge Functions | Transfer email, push, transactional email source | Local source only | Edge Function deployment if active | Internal/service | Not visible as deployed in Dashboard | Security/privacy-sensitive | Do not classify as active authority |

## 6. Role Vocabulary and Authority Boundary

- `ops/admin`: Internal operational administrator. Can access accepted ops tools only where backend/RPC/RLS confirms authority.
- `support`: Read-only or limited operational support role. Support visibility does not automatically grant mutation authority.
- `host`: Product role for event/venue/business operations. Host is not ops/admin.
- `staff`: Event-scoped operational role. Staff is not ops/admin.
- `scanner`: Staff subtype for check-in. Scanner is not support/admin.
- `manager`: Possible staff role. Manager is not ops/admin unless an explicit backend rule says otherwise.
- `user owner`: The authenticated user who owns a profile, notification, token, reminder, message, ticket, reservation, or media object.
- `service role`: Internal infrastructure authority. Service role usage is not user authority.
- `public/authenticated users`: Product users. Authentication alone is not support/admin authority.

Operational/admin roles are `ops/admin` and any explicitly defined `support` role. Product roles include host, staff, scanner, manager, user owner, participant, ticket holder, checked-in user, and public/authenticated users.

## 7. Dashboard Route Guard Assessment

Observed dashboard route evidence:

- `OpsGuard` requires `is_ops === true` and redirects non-ops users.
- `AuthContext` loads `is_ops` and tier from profile data after session resolution.
- `App.tsx` routes `/ops/users`, `/ops/tier`, `/ops/events`, `/ops/media-engine`, and `/ops/transfers` through `OpsGuard`.
- Sidebar contains an ops section visible only to `is_ops` users.

Interpretation:

- The route guard is useful UI gating.
- It is not backend authority.
- If a user can call a route's underlying RPC or table directly, the backend must still enforce ops/admin authority.
- `is_ops` profile exposure and update authority are security-sensitive and need profile/RLS verification.

Status: UI guard observed; backend authority varies by surface.

## 8. Ops/Admin RPC Authority Assessment

Observed RPC-mediated ops/admin surfaces:

- `ops_resolve_transfer_recipient_v1`
- `ops_approve_transfer_v1`
- `ops_reject_transfer_v1`
- `admin_execute_host_identity_transfer_v1`

Prior production evidence:

- `admin_execute_host_identity_transfer_v1` exists in production.
- It is SECURITY DEFINER.
- It has `search_path=public`.
- It includes an `auth_is_ops()` internal gate.
- Broad execute grants were reported.

Expected contract:

- Ops/admin RPCs must enforce internal authority, not rely on route guards.
- Broad grants are not automatically exploitable when internal gates exist, but they remain audit-significant.
- RPCs that mutate private identity, ownership, commerce, lifecycle, or support state should be auditable.

Status: Strongest production evidence exists for admin transfer execution. Other ops transfer RPC production parity remains Needs verification.

## 9. Host Identity Transfer Admin Assessment

Observed local dashboard transfer tooling:

- Lists transfer requests from `host_identity_transfers`.
- Enriches rows with `user_profiles` display names.
- Reads `host_transfer_audit_log`.
- Provides resolve, approve, reject, and execute actions.
- Uses RPCs for mutations.
- Displays audit details per transfer.

Prior handbook evidence:

- Host identity transfer affects organizer/persona identity and can move events, venues, layouts, followers, members, ratings, reviews, and invites depending on accepted transfer behavior.
- `joinfolk-web\supabase` and mobile migration history strongly implicate host identity transfer/ops provenance.
- `C:\dev\hostos\supabase\migrations` is the future working target, but not historical sole canonical proof.

Expected contract:

- Host identity transfer is ops/admin-only.
- Read visibility and mutation authority are distinct.
- Transfer execution must be internally gated and auditable.
- Personal identity preservation and organizer persona copy behavior must match Profile / Persona audit conclusions.

Status: Operational/admin-sensitive and privacy-sensitive. Audit log exists locally as a surface, but completeness needs verification.

## 10. User / Profile / Persona Support Visibility Assessment

Observed local support visibility:

- Ops user lookup searches users by name and UUID.
- It displays user ID, display name, tier, and created date when accessible.
- Page comments state results may be limited by RLS and missing results do not prove absence.
- Dashboard auth context reads profile tier and `is_ops`.
- Transfer tooling enriches transfer rows with `user_profiles` display names.

Expected contract:

- Support read visibility must be explicit, gated, and auditable if sensitive.
- Public profile visibility does not imply support/admin visibility.
- Tier and `is_ops` are authority/capability fields and should not be editable through ordinary client paths.
- Persona and profile fields exposed to ops/admin must be limited by accepted support purpose.

Status: RLS-limited read support surface observed. Production `profiles/user_profiles` policy evidence was not fully covered.

## 11. Event / Lifecycle Support Visibility Assessment

Observed local support visibility:

- Ops event override page fetches event title, status, visibility, host ID, event ID, and created date.
- Page comments state cross-host mutation is not possible via client-side auth because RLS blocks it.
- The page presents manual external override guidance, but the app itself does not perform event mutation in that surface.

Expected contract:

- Event support visibility is not lifecycle mutation authority.
- Lifecycle mutation remains backend/RPC-authoritative through accepted lifecycle functions.
- Manual support process, if used, needs separate operational controls and auditability outside app code.

Status: Read-only/RLS-limited support page observed; lifecycle mutation authority remains external/Unknown in this page.

## 12. Ticket / Commerce Support Authority Assessment

Observed evidence:

- No dedicated ops ticket/order/refund/dispute page was confirmed in focused ops route inspection.
- Prior commerce and direct-data audits flagged dashboard ticket status operations and revenue-sensitive RPC/direct boundaries.
- Production evidence found `tickets` and `event_ticket_claims_v1` RLS enabled with zero direct policies, making RPC/default-deny assumptions critical.
- `commerce_orders` had deny-all style authenticated policy evidence.

Expected contract:

- Ticket, order, claim, refund, cancellation, and dispute support actions are revenue-sensitive.
- Support read visibility must be separate from mutation/admin authority.
- Any support mutation must be backend/RPC-authoritative and auditable.

Status: Dedicated ops commerce support tooling not confirmed. Needs verification before patching.

## 13. Reservation Support Authority Assessment

Observed evidence:

- No dedicated ops reservation support page was confirmed in focused ops route inspection.
- Host/venue reservation pages exist outside ops routes.
- Prior reservation audits identified event and venue reservation RPCs and RLS evidence for reservations.

Expected contract:

- Reservation support visibility is separate from host/venue-owner authority.
- Reservation status mutation is revenue/product-sensitive and should be backend/RPC/RLS-authoritative.
- Support overrides, if accepted, need explicit audit logs.

Status: Unknown / Needs verification for ops/admin support tooling.

## 14. Venue / Media / Moderation Support Authority Assessment

Observed ops media support:

- `OpsMediaEnginePage` is an internal editorial CMS for media drafts.
- `mediaApi.ts` uses direct CRUD on `ops_media_drafts`.
- The source comments state access is restricted to `is_ops` users via RLS.
- The page supports draft, approved, scheduled, published, and rejected workflows.
- It stores editorial fields such as source URL/platform, organizer name, generated captions, scheduling, and linked event ID.

Observed host media moderation:

- Dashboard and hostos surfaces call `host_moderate_media_v1`.

Expected contract:

- Ops editorial drafts are operational/admin content, not public product data until explicitly published.
- Direct CRUD on `ops_media_drafts` must rely on verified RLS and accepted auditability.
- Host moderation and ops/admin moderation are separate authority concepts.
- Media/publication workflows must not expose private event/media/user data without accepted purpose.

Status: Ops media engine observed; production policy evidence for `ops_media_drafts` was not part of supplied production RLS reports.

## 15. Messaging / Private Conversation Support Visibility Assessment

Messaging audit evidence:

- No support/admin private conversation viewer was confirmed in targeted DM source.
- Dashboard `MessagesPage` is a host-persona inbox, not support/admin visibility.
- DM production evidence was not fully covered, and DM access is mostly RPC-mediated with one direct participant fallback read.

Expected contract:

- Support/admin private message visibility, if ever added, must be explicit, internally gated, purpose-limited, and auditable.
- Host/staff/ops route access must not automatically grant DM visibility.
- Public or host-persona messaging surfaces must not become support tooling accidentally.

Status: Support/admin DM visibility not confirmed.

## 16. Notification / Push / Reminder Support Visibility Assessment

Observed evidence:

- No dedicated ops notification, push token, reminder, or delivery log page was confirmed in focused ops route inspection.
- Prior notification audit found `notifications_v2`, `push_tokens_v1`, `user_notification_settings_v1`, reminder RPCs, and local Edge Function source ambiguity.
- `notifications_v2`, `push_tokens_v1`, and `user_notification_settings_v1` RLS were confirmed at a high level.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.

Expected contract:

- Notification records, push tokens, reminders, and delivery logs are privacy-sensitive.
- Support visibility must be owner/purpose scoped or explicit ops/admin.
- Push token support visibility should be minimal and auditable.

Status: Dedicated support/admin notification tooling not confirmed.

## 17. Diagnostics / Observability / Telemetry Assessment

Observed evidence:

- Mobile `remote-diagnostics.ts` inserts runtime state events into `app_diagnostics`.
- Comments reference an anon RLS policy whitelist.
- Prior direct-data audit flagged diagnostics as operational/admin-sensitive.
- No dedicated diagnostics dashboard page was confirmed in focused ops route inspection.

Expected contract:

- Telemetry visibility is operational/admin-sensitive and potentially privacy-sensitive.
- Diagnostics should not become authority enforcement.
- Diagnostic events should avoid sensitive user, profile, event, ticket, message, media, or location data unless explicitly accepted.
- Diagnostic read access should be gated and auditable.

Status: Diagnostics write surface observed; support/admin read surface unknown.

## 18. Audit Log / Admin Action Traceability Assessment

Observed evidence:

- Transfer tooling references `host_transfer_audit_log`.
- Transfer rows include an `audit_log` JSON-style field.
- Ops transfer page can load and display normalized audit entries per transfer.
- Prior production evidence mentioned `_transfer_audit` and transfer/admin functions.

Expected contract:

- Admin mutations should produce immutable or append-only audit evidence.
- Audit logs should include actor, action, target, timestamp, and relevant details without overexposing secrets/private payloads.
- Audit logs are observability, not authority enforcement.
- Read access to audit logs is itself ops/admin-sensitive.

Status: Transfer auditability surface observed. Coverage for other ops/admin actions is Unknown / Needs verification.

## 19. SECURITY DEFINER / Grants / Internal Gate Assessment

Prior production evidence:

- `admin_execute_host_identity_transfer_v1` exists, is SECURITY DEFINER, has `search_path=public`, has an `auth_is_ops()` body gate, and has broad execute grants.
- Some SECURITY DEFINER functions had `proconfig=null` or search_path concerns in prior reports.
- Broad execute grants were visible across admin, transfer, ticket, reservation, check-in, media, and notification categories.
- Broad grants alone are not enough to claim exploitability when an internal gate exists.

Expected contract:

- SECURITY DEFINER functions should have explicit internal gates.
- Search path posture should be stable and reviewable.
- Grants should be intentional and documented.
- Internal gates should be tested/reviewed separately from UI route guards.

Status: Candidate P1 review area from prior production reports, not a confirmed vulnerability list.

## 20. Edge Function Deployment Boundary Assessment

Observed evidence:

- Local Edge Function source exists in some Supabase trees, including notification/push and transfer email-related functions.
- Manual Supabase Dashboard evidence showed no deployed Edge Functions visible.
- `push-dispatch` was not visible/deployed in the production Edge Functions dashboard.

Expected contract:

- Edge Function source folders are not production deployment evidence.
- Edge Functions, if deployed later, need explicit deployment, auth, secret, signature, and caller-boundary verification.
- Internal service-role behavior is infrastructure authority, not user authority.

Status: Local-source-only unless future deployment evidence proves active production status.

## 21. Public / Private / Admin Boundary Assessment

Boundary rules:

- Public support pages and public verification/share pages are not ops/admin surfaces.
- Host, staff, scanner, manager, participant, ticket-holder, and checked-in roles do not inherit ops/admin visibility.
- Private user, profile, event, ticket, reservation, message, media, notification, push-token, diagnostic, and audit-log data must remain behind explicit backend authority.
- Support read access must be purpose-limited and separate from mutation.
- Ops/admin mutation must be internally gated and auditable.

Observed risk:

- Some ops pages display manual external mutation guidance because in-app RLS blocks mutation. This does not create in-app authority, but it signals an operational process that should be documented and audited outside this code path.

## 22. Dashboard Ops Surface Map

Observed dashboard ops surfaces:

- `/ops/users`: best-effort user lookup, RLS-limited.
- `/ops/tier`: read-only tier inspection with external/manual mutation guidance.
- `/ops/events`: read-only event inspection with external/manual override guidance.
- `/ops/media-engine`: direct CRUD editorial media draft workflow with claimed ops-only RLS.
- `/ops/transfers`: host identity transfer review, resolve, approve, reject, execute, and audit log display.

Dashboard route guard:

- `OpsGuard` checks `is_ops`.
- `is_ops` is loaded from profile data.
- Backend/RPC/RLS must enforce all data access and mutation.

## 23. Mobile / Web Ops Surface Map

Observed mobile/web surfaces:

- Public web support center exists as a public informational/contact route.
- Mobile remote diagnostics writes to `app_diagnostics`.
- Mobile host transfer screens exist for product transfer participant flows, not ops/admin dashboard authority.
- No mobile ops/admin support console was confirmed.
- No public web ops/admin tool was confirmed.

Interpretation:

- Mobile and public web surfaces are not ops/admin authority unless a future explicit route/tool proves otherwise.
- Diagnostics and transfer participant flows still intersect with ops/admin review and should be audited as supporting evidence.

## 24. Backend RPC / RLS Authority Evidence Map

Prior handbook evidence only:

- `admin_execute_host_identity_transfer_v1` production evidence exists.
- Events, tickets, reservations, `commerce_orders`, `event_media`, venues, `venue_media`, `notifications_v2`, `push_tokens_v1`, and `event_staff_assignments` RLS evidence exists at a high level, but policy correctness varies by table.
- `tickets` and `event_ticket_claims_v1` had zero direct policies in prior production evidence and likely depend on RPC/default-deny assumptions.
- `commerce_orders` had deny-all style authenticated policy evidence.
- `profiles/user_profiles` production RLS/policy evidence was not fully covered.
- DM/conversation/message production evidence was not fully covered.
- Social graph/block/mute table production evidence was not fully covered.
- Production evidence for `ops_media_drafts`, `host_identity_transfers`, and `host_transfer_audit_log` policy correctness was not fully covered in supplied reports.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Production SQL/RPC evidence remains stronger than local source assumptions.

Unreviewed ops/admin functions, tables, and policies must not be treated as safe.

## 25. Direct Data Access / RLS Reliance Map

| Data surface | Direct access observed | RPC-mediated access observed | RLS reliance status | Risk | Recommendation |
|---|---|---|---|---|---|
| `user_profiles` | Ops user lookup, profile/tier/is_ops load, transfer enrichment | Some profile RPCs elsewhere; not canonical here | Production policy not fully covered | Privacy/security-sensitive | Verify profile and ops flag policies |
| `events` | Ops event lookup | Lifecycle RPCs elsewhere | Events RLS enabled; correctness incomplete | Operational/admin-sensitive | Keep event mutation outside UI/direct path |
| `host_identity_transfers` | Transfer list/detail reads | Resolve/approve/reject/execute RPCs | Production policy incomplete | Privacy/admin-sensitive | Verify transfer RLS |
| `host_transfer_audit_log` | Transfer audit reads | `_transfer_audit` function evidence in reports | Production policy incomplete | Compliance/audit-sensitive | Verify audit log immutability/read access |
| `ops_media_drafts` | Direct list/create/update/delete | No RPCs observed | Local source claims ops-only RLS | Operational/admin-sensitive | Verify RLS and audit behavior |
| `tickets` | No ops support page confirmed | Ticket RPCs elsewhere | RLS zero direct policies | Revenue-sensitive | No support mutation without RPC |
| `commerce_orders` | No ops support page confirmed | Commerce RPCs elsewhere | Deny-all style policy evidence | Revenue-sensitive | Preserve RPC/support separation |
| `event_ticket_claims_v1` | No ops support page confirmed | Claim RPCs elsewhere | RLS zero direct policies | Revenue/privacy-sensitive | Preserve RPC authority |
| `reservations` | No ops support page confirmed | Reservation RPCs elsewhere | RLS enabled; policy review incomplete | Revenue/product-sensitive | Verify support visibility if added |
| `event_staff_assignments` | Not a focused ops page; direct staff paths elsewhere | Staff check-in RPCs | RLS enabled; policy review incomplete | Security-sensitive | Keep staff and ops roles separate |
| `event_media` / `venue_media` | Ops media drafts separate; host moderation elsewhere | `host_moderate_media_v1`, venue media RPCs | RLS evidence exists; correctness incomplete | Privacy-sensitive | Separate ops editorial from product media |
| Notifications / push / reminders | No ops support page confirmed | Notification/push/reminder RPCs elsewhere | Partial RLS evidence | Privacy-sensitive | Verify before support tooling |
| `app_diagnostics` | Mobile direct insert | None observed for ops read | Production evidence incomplete | Operational/privacy-sensitive | Dedicated diagnostics audit |
| DM/conversation tables | No support/admin viewer confirmed | DM RPCs for user/host messaging | Production evidence incomplete | Privacy-sensitive | Do not add support visibility without contract |

## 26. Duplicated / Split / Legacy Ops/Admin Surfaces

| Surface / helper / RPC / table | Observed role | Current / legacy / unknown | Risk if still active or authoritative | Evidence type | Recommendation |
|---|---|---|---|---|---|
| `OpsGuard` | Dashboard UI gate | Current/plausible | Mistaken for backend authority | Local source | Treat as UI-only guard |
| `is_ops` profile flag | Ops route gating signal | Current/plausible | Profile field exposure/update policy drift | Local source | Verify profile RLS |
| `admin_execute_host_identity_transfer_v1` | Transfer execution | Production-confirmed | Broad grants need internal gate review | Production report | Preserve gate; verify auditability |
| `ops_approve_transfer_v1` / `ops_reject_transfer_v1` / `ops_resolve_transfer_recipient_v1` | Transfer workflow | Local dashboard wrapper | Production parity incomplete | Local source | Verify production existence/gates |
| `host_identity_transfers` | Transfer request table | Current/plausible | Sensitive direct read surface | Local source | Verify RLS |
| `host_transfer_audit_log` and `_transfer_audit` | Transfer traceability | Current/plausible | Audit gaps or overexposed details | Local + production report | Verify completeness |
| `ops_media_drafts` | Internal editorial CMS | Current/plausible | Direct CRUD with unverified production policy | Local source | Verify RLS/audit |
| Manual external mutation guidance | Tier/event overrides outside app | Current/plausible support process | Untracked operational changes if no process/audit | Local source | Document process separately |
| Local Edge Function folders | Push/email/transactional source | Local-source-only | Mistaken as deployed authority | Manual dashboard evidence | Verify deployment before relying |

## 27. Ops-Admin-Critical Invariants

- Ops/admin authority is internally gated and auditable.
- Host, staff, scanner, and manager authority is not ops/admin authority.
- Support read visibility does not imply mutation authority.
- Dashboard route guards are not security controls.
- SECURITY DEFINER functions require explicit internal gates and stable search_path.
- Broad grants are not treated as exploitable when internal gates exist, but they remain audit-significant.
- Private user/profile/event/ticket/message/media data is visible only through explicit accepted support/admin contracts.
- Revenue-sensitive support actions are backend/RPC-authoritative and auditable.
- Host identity transfer is ops/admin-only and auditable.
- Edge Function local source is not deployment evidence.
- Public/social/host/staff surfaces do not inherit ops/admin visibility.
- Service role usage is internal infrastructure, not user authority.
- Audit logs support traceability; they do not replace authority checks.

## 28. Unknown / Needs Verification Surfaces

- Production policy correctness for `user_profiles.is_ops`.
- Production RLS/policy correctness for `host_identity_transfers`.
- Production RLS/policy correctness for `host_transfer_audit_log`.
- Production RLS/policy correctness for `ops_media_drafts`.
- Production parity for `ops_resolve_transfer_recipient_v1`, `ops_approve_transfer_v1`, and `ops_reject_transfer_v1`.
- Completeness of audit logging for transfer actions.
- Whether any support/admin user, event, ticket, reservation, notification, DM, media, or diagnostic views exist outside inspected dashboard routes.
- Operational controls around manual external tier/event overrides.
- Whether diagnostics can be read by support/admin tools.
- Whether local Edge Functions are deployed elsewhere outside the observed Dashboard state.

## 29. Ops / Admin / Support Gaps / Risk Register Seeds

### OAS-GAP-001

- Domain: Ops route guard and backend authority
- Current issue: Ops dashboard routes rely on `OpsGuard` and profile `is_ops` for UI gating.
- Expected clean ops/admin/support authority contract: Backend/RPC/RLS independently enforces every ops read and mutation.
- Risk: Security-sensitive.
- Priority candidate: Candidate P1.
- Blocked by: Profile/RLS verification and ops RPC/table policy review.
- Recommended next action: Verify `is_ops` field visibility/update authority and backend gates for each ops surface.

### OAS-GAP-002

- Domain: Host identity transfer
- Current issue: Transfer page mixes direct table reads, audit reads, and RPC mutations; production evidence is strongest only for execution RPC.
- Expected clean ops/admin/support authority contract: Transfer read, review, execute, and audit actions are ops-only, internally gated, and auditable.
- Risk: Operational/admin-sensitive and privacy-sensitive.
- Priority candidate: Candidate P1.
- Blocked by: Production RLS/RPC parity for transfer tables and related ops RPCs.
- Recommended next action: Verify transfer table policies and related ops RPC internal gates.

### OAS-GAP-003

- Domain: Transfer auditability
- Current issue: Transfer audit log surface exists, but completeness and immutability are not verified.
- Expected clean ops/admin/support authority contract: Admin mutations create complete, append-only, ops-readable audit records.
- Risk: Compliance/audit-sensitive.
- Priority candidate: Candidate P2.
- Blocked by: Audit table/function review.
- Recommended next action: Dedicated audit log traceability review.

### OAS-GAP-004

- Domain: Ops media engine
- Current issue: `ops_media_drafts` uses direct CRUD with claimed ops-only RLS, but supplied production policy evidence did not cover the table.
- Expected clean ops/admin/support authority contract: Ops editorial drafts are RLS-gated, auditable, and clearly separated from public/product media.
- Risk: Operational/admin-sensitive and privacy-sensitive.
- Priority candidate: Candidate P2.
- Blocked by: Production policy verification.
- Recommended next action: Verify `ops_media_drafts` RLS and publication/audit semantics.

### OAS-GAP-005

- Domain: Manual support overrides
- Current issue: Tier and event pages present manual external mutation guidance because client RLS blocks in-app mutation.
- Expected clean ops/admin/support authority contract: Manual operational changes are governed by an accepted, auditable support process.
- Risk: Operational/admin-sensitive and revenue-sensitive.
- Priority candidate: Candidate P2.
- Blocked by: Product/operations decision.
- Recommended next action: Document manual support process and audit expectations.

### OAS-GAP-006

- Domain: SECURITY DEFINER grants
- Current issue: Broad grants and missing search_path concerns exist across sensitive function categories, with internal gates on some functions.
- Expected clean ops/admin/support authority contract: SECURITY DEFINER functions have explicit gates, stable search_path, intentional grants, and reviewed reachability.
- Risk: Security-sensitive.
- Priority candidate: Candidate P1.
- Blocked by: Approved read-only function/grant review.
- Recommended next action: Keep as hardening review candidate; do not claim exploitability without evidence.

### OAS-GAP-007

- Domain: Diagnostics visibility
- Current issue: `app_diagnostics` write path exists, but support/admin read surface and retention/privacy contract are unclear.
- Expected clean ops/admin/support authority contract: Telemetry is privacy-scoped, support-readable only when accepted, and not authority enforcement.
- Risk: Privacy-sensitive and operational/admin-sensitive.
- Priority candidate: Candidate P2.
- Blocked by: Diagnostics/observability audit.
- Recommended next action: Run Diagnostics / Observability / Audit Log Contract Audit.

### OAS-GAP-008

- Domain: Private messaging support visibility
- Current issue: No support/admin DM viewer was confirmed, but future tooling would be highly sensitive.
- Expected clean ops/admin/support authority contract: Support/admin message visibility, if present, is explicit, internally gated, purpose-limited, and auditable.
- Risk: Privacy-sensitive.
- Priority candidate: Unknown / Needs verification.
- Blocked by: Product decision on support access.
- Recommended next action: Do not infer support DM visibility from host messaging.

### OAS-GAP-009

- Domain: Edge Function deployment boundary
- Current issue: Local Edge Function source exists, but Dashboard evidence did not show deployed functions.
- Expected clean ops/admin/support authority contract: Edge Function source is treated as inactive until deployment and auth/caller boundaries are verified.
- Risk: Security-sensitive and privacy-sensitive.
- Priority candidate: Unknown / Needs verification.
- Blocked by: Deployment evidence.
- Recommended next action: Verify deployment before classifying any Edge Function as active ops/admin authority.

## 30. Product Decisions Required

- What is the accepted source of truth for ops/admin role assignment?
- Is `is_ops` the only ops/admin role signal, or are support/admin tiers separate?
- Which support views are read-only and which are mutation-capable?
- Are manual external tier/event overrides part of accepted operations?
- What audit trail is required for manual support actions?
- Which transfer actions require normalized audit entries?
- Should ops media drafts be audited like admin mutations?
- Can support/admin ever view private conversations?
- Can support/admin view push tokens, notification payloads, reminders, or diagnostics?
- What retention and privacy rules apply to diagnostics and audit logs?

## 31. Recommended Next Audits

1. Diagnostics / Observability / Audit Log Contract Audit.
2. Abuse / Reporting / Moderation Contract Audit.
3. Payments / Refunds / Disputes Operations Audit.

These follow because ops/admin authority depends on traceability, support workflows, moderation/reporting, and revenue-sensitive operational controls.

## 32. Non-Goals

- This audit does not authorize application code changes.
- This audit does not authorize dashboard, mobile, web, or Supabase tree changes.
- This audit does not create SQL, migrations, or implementation instructions.
- This audit does not connect to production.
- This audit does not claim production vulnerability.
- This audit does not claim feature removal is safe.
- This audit does not claim support/admin access is unsafe solely because it exists.
- This audit does not claim broad execute grants are exploitable when internal gate evidence exists.
- This audit does not classify local Edge Function source as active production authority.

## 33. Open Questions

- Who can assign or revoke `is_ops`?
- Is there a separate support-only role with read-only access?
- Are tier and event manual overrides currently audited outside the app?
- Is `host_transfer_audit_log` complete for every transfer state transition?
- Do ops transfer RPCs all use the same internal gate model as `admin_execute_host_identity_transfer_v1`?
- Is `ops_media_drafts` production-active and RLS-gated as source comments indicate?
- Should ops media draft status changes create audit log entries?
- Is there any support/admin private message viewer outside the inspected dashboard?
- Can support view diagnostics, and what fields are permitted?
- Which Edge Functions, if any, are deployed outside the visible Supabase Dashboard evidence?

## 34. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `07_Audits/OpsAdminSupportToolsAuthorityAudit.md` was created/modified.

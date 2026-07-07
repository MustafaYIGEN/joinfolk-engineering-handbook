# Staff / Host Operations Authority Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false

## 2. Purpose

This audit maps JoinFolk host, staff, scanner, manager, venue-owner, and ops/admin authority boundaries across dashboard, mobile, backend RPC, RLS, storage, and public verification surfaces.

This is a staff, host operations, and ops/admin authority audit only. It is not an implementation plan, cleanup plan, patch plan, migration plan, or authorization to modify backend/RPC/RLS/storage/auth behavior.

The audit separates:

- host authority from staff authority
- staff scanner authority from staff manager authority
- host/staff operational authority from participant, ticket-holder, and checked-in entitlement
- dashboard/mobile UI guards from backend/RPC/RLS authority
- ops/admin authority from public, social, host, and participant visibility
- event host authority from venue/business owner authority
- staff assignment authority from staff action authority
- production SQL/RPC evidence from local-source-only evidence
- Database Functions / RPC evidence from Edge Function deployment evidence

## 3. Audit Scope

In scope:

- Host event operations, including creation, edit, publish, lifecycle controls, module setup, commerce setup, venue setup, and dashboard host actions.
- Staff assignment and role boundaries, including scanner and manager role evidence.
- Mobile and dashboard scanner/check-in surfaces.
- Ticket, reservation, venue, layout, session, media, notification, and transfer operations where host/staff/ops authority is relevant.
- Public check-in proof verification boundary.
- Direct table access and RLS reliance at a high level.
- Prior production SQL/RPC evidence documented in handbook reports.

Out of scope:

- Writing SQL, creating migrations, or changing application/backend behavior.
- Production verification, Supabase CLI usage, builds, tests, installs, commits, or staging.
- Final canonical source-of-truth claims.

Current source-path context:

- Supabase migration future working target is `C:\dev\hostos\supabase\migrations`.
- This is not historical sole canonical proof.
- Split-source migration history remains unresolved.
- Production SQL/RPC evidence remains stronger than local source assumptions.
- Database Functions / RPC evidence is separate from Edge Function deployment evidence.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Local Edge Function source folders exist in some Supabase trees, but deployment status is not confirmed.
- No backend patch or migration is authorized by this audit.

## 4. Staff / Host Operations Authority Contract Summary

JoinFolk has a multi-role operations model:

- Event hosts appear to own event-scoped operational actions.
- Staff assignments are stored separately and appear role-scoped by event.
- Scanner role is intended for check-in authority, not broad host authority.
- Manager role exists in local source, but its full product and backend authority contract remains Needs verification.
- Ops/admin tools exist for sensitive support flows such as host identity transfer.
- Venue/business ownership appears separate from event host authority and must remain separately enforced.

The clean authority contract should treat UI controls as guidance only. Any action that changes lifecycle, ticket state, reservation state, staff assignment, venue layout, media moderation, host identity, proof/check-in state, notification ownership, or operational visibility must be backend/RPC/RLS-authoritative.

Positive production evidence exists for core check-in RPCs. Unresolved evidence remains for proof helper functions, staff assignment policy correctness, direct table writes, manager role boundaries, and split-source operational surfaces.

## 5. Operations Authority Surface Inventory Matrix

| Surface / domain | Operational action or visibility exposed | Access path observed | Expected authority owner | Scope | Production evidence status | Risk class | Recommendation |
|---|---|---|---|---|---|---|---|
| Host event operations | Create, edit, publish, lifecycle controls | Mixed direct table access and RPCs | Backend/RPC/RLS/auth | Host | Events RLS enabled; lifecycle RPC evidence exists | Operational/admin-sensitive | Document contract and verify mutation boundaries |
| Staff assignment | Assign, list, remove event staff | Direct `event_staff_assignments` reads/upserts/deletes in local source | Backend/RLS or RPC | Host, staff | RLS enabled and policies observed; correctness needs deeper review | Security-sensitive | Verify policy contract or prefer RPC later |
| Scanner check-in | Scan ticket, check in by ID/code, undo | RPC-mediated check-in functions | Backend/RPC/auth | Host, scanner/staff | Positive production controls for key RPCs | Revenue-sensitive | Preserve and document scanner limits |
| Proof/check-in helpers | Create/read/remove proof material | RPC/helper evidence from prior reports | Backend/RPC/auth | Host, staff, proof owner | Prior keyword scan raised missing auth/staff signals | Security-sensitive | Harden later after explicit patch approval |
| Manager operations | Possible elevated staff actions | Staff role value observed | Backend/RPC/RLS/auth | Manager | Product/backend limits unclear | Operational/admin-sensitive | Needs product decision |
| Ticket operations | Ticket status, queue, check-in, sales stats | RPCs plus possible direct dashboard access from prior audits | Backend/RPC/RLS/auth | Host, staff, ticket owner | Tickets RLS enabled with zero direct policies | Revenue-sensitive | Prefer RPC authority and reconcile direct paths |
| Reservation operations | Approve, reject, cancel, check-in reservation-like flows | Dashboard/mobile surfaces and reservation RPC candidates | Backend/RPC/RLS/auth | Host, venue owner, user | Reservations RLS enabled; full policy correctness not complete | Revenue-sensitive | Document host/staff/venue-owner authority |
| Venue/layout/session operations | Venue edit, media, layout save, session selection | RPCs and direct reads | Backend/RPC/RLS/storage/auth | Host, venue owner | Venues/venue_media RLS evidence exists; layout/session coverage incomplete | Product correctness | Verify venue-owner and host boundaries |
| Event lifecycle operations | Publish, go live, end, archive, cancel, open check-in | RPCs such as `transition_event_status_v2` and publish/control functions | Backend/RPC/auth | Host | RPC evidence exists; some function hardening concerns remain | Operational/admin-sensitive | Keep backend-authoritative |
| Media moderation | Host moderation, hide/unhide/delete, public highlight controls | `host_moderate_media_v1` and media surfaces | Backend/RPC/RLS/storage/auth | Host, staff, ops, uploader | `event_media` RLS enabled; policy correctness needs review | Privacy-sensitive | Document moderation contract |
| Notification / host inbox | Operational notifications, host unread counts, mark read | RPC/direct surfaces from prior audits | Backend/RPC/RLS/auth | Owner, host, ops | `notifications_v2` RLS enabled; v1/reminder coverage incomplete | Privacy-sensitive | Verify owner/host scoping |
| Host identity transfer | Admin/ops transfer and persona copy | Ops/admin RPCs | Ops/admin RPC with internal gate | Ops/admin | Production evidence for `admin_execute_host_identity_transfer_v1` with `auth_is_ops()` gate | Operational/admin-sensitive | Preserve internal gate; verify auditability |
| Dashboard route guards | Host/staff/ops UI routing | UI guards and page-level checks | UI mirror only | Dashboard users | Local-source-only | Security-sensitive | Do not treat as authority |
| Mobile staff tools | Staff event list and scanner entry | Direct assignment reads plus scanner RPC | Backend/RPC/RLS/auth | Staff, scanner, manager | Staff RLS evidence exists | Operational/admin-sensitive | Keep mobile UI as mirror |
| Public verification | Public proof/check-in verification | Public RPC/function evidence | Backend/RPC/RLS | Public read only | `public_verify_checkin` provenance unresolved locally | Privacy-sensitive | Verify public-safe output contract |

## 6. Role Vocabulary and Authority Boundary

- `host`: Event-scoped operational authority, likely derived from event ownership such as `events.host_id`. Host authority must be backend-enforced and cannot be inferred from dashboard routing alone.
- `staff`: Event-scoped assigned operational role, observed through `event_staff_assignments`. Staff is not automatically host.
- `scanner`: Staff role intended for scanning/check-in. Scanner authority should be limited to check-in actions and should not imply manager, host, venue owner, or ops/admin authority.
- `manager`: Staff role observed in local source. Its allowed actions remain Unknown / Needs verification.
- `ops/admin`: Internal operational authority for support/admin actions such as host identity transfer. Ops/admin authority is not public, social, host, participant, or ticket-holder visibility.
- `support/admin`: Possible dashboard/support authority layer. Needs explicit product and backend contract.
- `venue owner`: Venue/business authority for venue, media, layout, and related operations. This must remain separate from event host authority unless product explicitly bridges them.
- `participant`: Event participation/viewer role, not operational authority.
- `ticket_holder`: Commerce entitlement/viewer role, not staff or host authority.
- `checked_in`: Attendance/viewer state, not staff or host authority.

Authority roles are `host`, `staff`, `scanner`, `manager`, `venue owner`, and `ops/admin`. Entitlement/viewer roles are `participant`, `ticket_holder`, and `checked_in`.

## 7. Host Event Operations Assessment

Host event operations include event creation, editing, publish/readiness, lifecycle transitions, module setup, commerce setup, venue binding, staff management, check-in controls, media moderation, and dashboard host tools.

Observed evidence:

- Handbook and local docs reference `publish_event_with_groups_and_snapshot_v2`, `publish_event`, and `transition_event_status_v2`.
- Dashboard and mobile surfaces include event editing, ticket setup, venue setup, staff/check-in, and host action workstations.
- Prior lifecycle and direct access audits identified mixed direct event access and RPC-mediated lifecycle control.

Expected contract:

- Host authority should be event-scoped and backend-authoritative.
- Lifecycle transitions should be controlled by RPCs, not by direct client-side status mutation.
- Dashboard action availability can mirror backend rules but cannot be the final authority.
- Event creation/edit direct writes are acceptable only with documented RLS policies and accepted product semantics.

Status: Mostly deterministic for intended host control; backend authority completeness remains Needs verification for all direct mutations.

## 8. Staff Assignment Contract Assessment

Observed local source includes staff assignment flows around `event_staff_assignments`:

- Direct reads for staff role lookup and staff event listing.
- Direct staff list reads for host/dashboard views.
- Direct upsert/delete style staff assignment management in mobile/local helper evidence.
- Dashboard staff pages read assigned staff events and expose scanner entry guidance.
- Role values include `scanner` and `manager`.

Prior production evidence confirms `event_staff_assignments` has RLS enabled and policy surface for select/insert/update/delete. Policy correctness still needs deeper review.

Expected contract:

- Only an event host or explicitly authorized manager/ops role should create or remove staff assignments.
- Staff assignment is event-scoped and role-scoped.
- Staff role lookup should be owner-scoped to the authenticated assigned user unless host/ops view is explicitly intended.
- Assignment read/write may remain direct only if RLS policy correctness is documented.

Risk: Security-sensitive and operational/admin-sensitive because assignment creates operational capability.

Recommendation: Verify staff assignment RLS policy semantics, define manager role powers, and consider RPC-mediated assignment later if direct policy complexity remains high.

## 9. Scanner Check-In Authority Assessment

Observed surfaces:

- Mobile scanner and staff event surfaces.
- Dashboard staff scanner page that guides staff toward mobile scanning.
- RPC candidates include `staff_checkin_ticket_v1`, `checkin_ticket_by_id_v2`, `checkin_ticket_v2`, and `undo_checkin_ticket_v2`.

Prior production evidence:

- `checkin_ticket_by_id_v2` had positive controls including `auth.uid`, event host checks, live status checks, ticket `event_id` scope, code match, and ticket status state-machine checks.
- `staff_checkin_ticket_v1` had positive controls including `auth.uid`, host/staff checks, `event_staff_assignments`, ticket owner/user ID, status, checked-in state, and `AUTH_REQUIRED` signals.
- Proof helper functions such as `ensure_ticket_checkin_proof_v1`, `record_checkin_proof_v1`, and `remove_ticket_checkin_proof_v1` had concerning prior scan evidence because obvious auth/host/staff checks were not found in keyword search.

Expected contract:

- Scanner check-in is backend/RPC-authoritative.
- Mobile scanner UI cannot authorize check-in by itself.
- Scanner role should not imply host, manager, reservation, media moderation, or ops authority.
- Undo check-in and proof mutation require the same or stronger authority checks as positive check-in.

Status: Core check-in path appears strongest where RPC controls are documented. Proof-helper authority remains unresolved.

## 10. Manager / Staff Operations Assessment

`manager` appears as a staff role value in local source. The current product and backend authority limits are not fully established.

Potential manager powers needing decision:

- Can manager scan tickets?
- Can manager assign/remove scanner staff?
- Can manager handle ticket queues or reservations?
- Can manager moderate media?
- Can manager view host-only operational dashboards?
- Can manager trigger lifecycle transitions?
- Can manager manage venue/session/layout details?

Expected contract:

- Manager must be explicitly defined and backend-enforced.
- Manager should not be treated as host unless product explicitly says so.
- Manager UI controls should mirror backend authority only.

Status: Unknown / Needs verification.

## 11. Ticket / Commerce Operations Authority Assessment

Ticket and commerce operations include ticket product setup, ticket queue, status changes, sales stats, order/payment state, check-in eligibility, and staff/host operational ticket actions.

Observed evidence:

- Web/mobile ticket helpers call RPCs such as `checkin_ticket_v2`, `checkin_ticket_by_id_v2`, and `undo_checkin_ticket_v2`.
- Prior commerce audit identified multiple ticket purchase/order/claim/guard RPCs and revenue-sensitive invariants.
- Prior focused production evidence found `tickets` RLS enabled with zero direct policies, making RPC/default-deny assumptions critical.
- Prior evidence found `commerce_orders` had deny-all style authenticated policy evidence.

Expected contract:

- Ticket purchase, issuance, status mutation, check-in, cancellation, transfer, claim, and order/payment authority must be backend/RPC-authoritative.
- Dashboard ticket controls must not write revenue-sensitive status directly unless RLS policy correctness is explicit and accepted.
- Host/staff ticket visibility must be event-scoped.

Status: Product-critical and revenue-sensitive. RPC authority appears preferred; direct access boundaries need continued verification.

## 12. Reservation Operations Authority Assessment

Reservation operations include event reservations, venue reservations, approval/rejection, cancellation, status updates, and venue-owner/host operational views.

Observed evidence:

- Prior audits identified event reservation and venue reservation RPC candidates such as `create_reservation_v1/v2`, `cancel_reservation_v1`, `update_reservation_status_v1`, `create_venue_reservation_v1/v2`, `decide_venue_reservation_v2`, and `update_venue_reservation_status_v1`.
- Dashboard reservation pages exist for host or semi-pro style operations.
- Reservations RLS was confirmed enabled in prior production evidence, but full policy correctness remains Needs verification.

Expected contract:

- Reservation approval, rejection, cancellation, and status mutation must be backend/RPC/RLS-authoritative.
- Event host authority and venue-owner authority must be separated where venue reservations differ from event reservations.
- Staff scanner authority should not imply reservation management authority.

Status: Mostly deterministic at product level; enforcement owner remains Mixed / Needs verification.

## 13. Venue / Layout / Session Operations Authority Assessment

Venue and layout operations include venue creation/update, venue media, visual venue layouts, saved layouts, event seating config, sessions, seat/standing areas, product-section mapping, and buyer preview.

Observed evidence:

- Dashboard venue pages use RPC candidates such as `create_visual_venue_layout_v1` and `save_venue_layout_v1`.
- Prior audits identified `create_venue_layout_v1`, `update_event_seating_config_v1`, `upsert_event_ticket_product_v2`, and session/seat availability RPCs.
- Mobile session picker evidence includes direct reads from `event_sessions_v1`.
- Venue and `venue_media` RLS evidence exists at a high level; layout/session table coverage remains incomplete.

Expected contract:

- Venue owner/business authority must remain separate from event host authority unless explicitly bridged.
- Layout/session/product-section mutations should be RPC-mediated or protected by verified RLS.
- Buyer availability and purchase authority must not rely on dashboard preview geometry alone.

Status: Product-critical for venue buyer flow and revenue correctness. Needs production policy verification for direct layout/session reads or writes.

## 14. Event Lifecycle Operations Authority Assessment

Lifecycle operations include draft save, publish, go live, end, archive, cancel, delete/remove, check-in open/close, and revert behavior.

Observed evidence:

- Local docs reference `transition_event_status_v2(p_event_id, p_action)`.
- Publish flows reference `publish_event_with_groups_and_snapshot_v2`.
- Control functions such as `control_open_checkin` and `control_cancel_event` appear in prior lifecycle and operations evidence.
- Prior production/focused reports flagged some SECURITY DEFINER/search path concerns across functions.

Expected contract:

- Lifecycle state mutation must be backend/RPC-authoritative.
- UI status normalization and dashboard buttons are not authority.
- Lifecycle controls should enforce host/ops authority, event ownership, allowed transition graph, and side effects such as ticket/check-in/media availability.

Status: Product-critical and operational/admin-sensitive. Use canonical lifecycle RPCs where possible and verify legacy/control surfaces.

## 15. Media Moderation Authority Assessment

Media moderation includes host moderation, owner hide/delete, staff/ops moderation, public highlight selection/removal, comment moderation, and storage object consistency.

Observed evidence:

- Prior audits identified `host_moderate_media_v1`.
- Mobile media access helpers distinguish checked-in, host, and staff access for gallery behavior.
- `event_media` RLS was confirmed enabled, but policy correctness still needs deeper review.
- Storage policy evidence exists for some venue/avatar buckets, but event media bucket production evidence remains incomplete unless separately covered.

Expected contract:

- Host/staff/ops media moderation must be backend/RPC/RLS/storage-authoritative.
- Uploader hide/delete authority must not grant unrelated media control.
- Moderation must keep media records and storage objects consistent.
- Public highlights should expose only approved public media.

Status: Privacy-sensitive and product-critical. Needs documented moderation role contract.

## 16. Notification / Host Inbox Operations Assessment

Notification and host inbox operations include host operational notification reads, unread counts, mark-read behavior, transfer/reservation/ticket notifications, and dashboard host inbox surfaces.

Observed evidence:

- Prior notification audit identified `notifications_v2`, `push_tokens_v1`, `user_notification_settings_v1`, reminder RPCs, and local Edge Function source ambiguity.
- `notifications_v2` RLS was confirmed enabled in prior production evidence.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.

Expected contract:

- Notification read/mark-read must be owner-scoped or host/ops scoped by explicit backend rule.
- Operational notifications should not leak private ticket, transfer, event, media, profile, or group details.
- Push/notification dispatch authority is separate from in-app notification record authority.

Status: Privacy-sensitive. Host inbox scope remains Needs verification.

## 17. Host Identity Transfer / Ops Admin Authority Assessment

Host identity transfer and ops/admin tools are operational/admin-sensitive.

Observed evidence:

- Production evidence exists for `admin_execute_host_identity_transfer_v1`.
- The function was reported as SECURITY DEFINER with `search_path=public`.
- Body evidence includes an `auth_is_ops()` internal gate.
- Execute grants were broad, but broad grants alone are not enough to claim exploitability because internal ops gate evidence exists.
- Related transfer names from prior reports and targeted terms include `admin_approve_transfer_v1`, `admin_reject_transfer_v1`, `accept_host_transfer_v1`, `execute_host_transfer_v1`, `request_host_transfer_v1`, `initiate_host_transfer_v1`, `ops_approve_transfer_v1`, `ops_reject_transfer_v1`, and `ops_resolve_transfer_recipient_v1`.

Expected contract:

- Ops/admin transfer actions must be internally gated, audited, and isolated from host/staff/public authority.
- Host identity transfer must preserve personal identity and copy only accepted host persona fields.
- Dashboard ops route guards are not sufficient authority.

Status: Operational/admin-sensitive. Preserve internal gate assumption and verify auditability before any future patch.

## 18. Dashboard / Ops Surface Map

Dashboard/ops surfaces observed or referenced by prior audits include:

- Host event dashboard and action workstations.
- Tickets page and ticket status/check-in controls.
- Reservation pages.
- Venue page, venue layout editor, visual layout save/create flows.
- Staff events page and scanner guidance page.
- Media moderation and host gallery controls.
- Transfer/admin tools.
- Ops/admin guarded routes.

Authority classification:

- Dashboard route guards and buttons are UI mirrors only.
- Dashboard operations that mutate events, tickets, reservations, staff assignments, venues, layouts, sessions, media, transfers, or notifications must be backend/RPC/RLS-authoritative.
- Dashboard-only visibility must not leak into public, social, feed, profile, or share surfaces.

## 19. Mobile Staff / Scanner Surface Map

Mobile staff/scanner surfaces observed or referenced include:

- Staff role helper and staff event listing through `event_staff_assignments`.
- Scanner entry through event detail render model and staff tools.
- RPC-mediated staff check-in via `staff_checkin_ticket_v1`.
- Ticket/check-in RPC helper usage.
- Media access helpers distinguishing host/staff/checked-in behavior.

Authority classification:

- Mobile staff UI can guide eligible users.
- Mobile staff UI cannot grant check-in authority.
- Direct staff assignment reads rely on RLS correctness.
- Scanner actions should remain RPC-mediated.

## 20. Public Verification Boundary Assessment

Public verification surfaces include public check-in proof verification and proof readback behavior.

Observed evidence:

- Prior docs reference check-in proofs and public lookup/verification behavior.
- `public_verify_checkin` was known from production RPC evidence but was not found in local provenance comparison.
- Proof helper provenance was split and unresolved, with mobile migration history strongly implicated for proof-specific helpers.

Expected contract:

- Public verification is public-safe read authority only.
- Public verification must not create, mutate, revoke, or issue proof.
- Public verification must expose only accepted fields and must not leak private ticket, user, event, staff, host, or claim data.
- Public verification is not staff authority.

Status: Privacy-sensitive and proof/security-sensitive. Needs production function and output contract review.

## 21. ViewerRole / Entitlement / Staff Role Interaction Map

Role separation:

- `guest`: no operational authority.
- `authenticated_non_participant`: no operational authority.
- `participant`: viewer/participation state, no operational authority.
- `ticket_holder`: commerce entitlement, no operational authority.
- `checked_in`: attendance state, no operational authority.
- `host`: event-scoped operational authority.
- `staff`: assigned operational role, event-scoped.
- `scanner`: staff role limited to scanner/check-in operations.
- `manager`: staff role requiring explicit backend contract.
- `ops/admin`: internal operational authority, separate from host/staff/social visibility.

Important interaction rules:

- Staff authority should not be inferred from ticket/participant entitlement.
- Checked-in status should not grant staff or host authority.
- Staff role should not automatically inherit all checked-in social/gallery privileges unless product explicitly defines it.
- Host/staff/ops operational access should not make private content public.

## 22. Backend RPC / RLS Authority Evidence Map

Prior handbook production evidence only:

- `event_staff_assignments` RLS was confirmed enabled at a high level, with policy surface evidence, but policy correctness still needs deeper review.
- Events, tickets, reservations, `commerce_orders`, `event_media`, venues, and `venue_media` RLS evidence exists at a high level, but policy correctness varies by table.
- `tickets` and `event_ticket_claims_v1` had zero direct policies in prior production evidence and likely depend on RPC/default-deny assumptions.
- `commerce_orders` had deny-all style authenticated policy evidence.
- `checkin_ticket_by_id_v2` had positive production control evidence around auth, host, live status, ticket scope, code match, and status state machine.
- `staff_checkin_ticket_v1` had positive production control evidence around auth, host/staff, assignment, ticket owner, status, checked-in, and auth-required checks.
- Proof helper functions had prior scan concerns because obvious auth/staff/host checks were not found by keyword search.
- `admin_execute_host_identity_transfer_v1` production evidence exists with SECURITY DEFINER, `search_path`, and `auth_is_ops()` gate.
- Some SECURITY DEFINER functions had `proconfig=null` or search path concerns in prior production/focused reports.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Local Edge Function source folders exist, but local source folders are not deployment proof.
- Production SQL/RPC evidence remains stronger than local source assumptions.

Unreviewed functions, policies, and tables must not be treated as safe.

## 23. Direct Data Access / RLS Reliance Map

| Surface / table | Direct access evidence | RPC-mediated evidence | RLS reliance status | Risk | Recommendation |
|---|---|---|---|---|---|
| `event_staff_assignments` | Direct reads/upserts/deletes in staff helper evidence | None clearly canonical for assignment mutation | RLS confirmed enabled; correctness needs review | Security-sensitive | Document staff RLS contract or prefer RPC later |
| `events` | Direct event reads/writes across app surfaces from prior audits | Publish/lifecycle RPCs | RLS enabled; policy correctness incomplete | Operational/admin-sensitive | Keep lifecycle mutation RPC-authoritative |
| `tickets` | Prior direct access concerns; RPC helpers also observed | Check-in and ticket RPCs | RLS enabled with zero direct policies | Revenue-sensitive | Treat RPC/default-deny as critical |
| `reservations` | Dashboard/mobile surfaces from prior audits | Reservation RPC candidates | RLS enabled; correctness incomplete | Revenue-sensitive | Verify host/venue-owner operations |
| `commerce_orders` | Order state surfaces | Commerce RPCs | Deny-all style policy evidence | Revenue-sensitive | Preserve RPC authority |
| `venues` / `venue_media` | Venue/media reads and dashboard operations | Venue/update/media RPCs | RLS enabled; storage evidence partial | Privacy/product correctness | Verify venue-owner authority |
| `venue_layouts` / layout tables | Dashboard layout operations | Layout save/create RPCs | Production coverage incomplete | Revenue/product correctness | Prefer RPC or verify RLS |
| `event_sessions_v1` | Mobile session picker direct read observed | Session authority unclear | Not covered by supplied production evidence | Product correctness | Verify RLS and lifecycle/host scope |
| `event_media` | Gallery/media surfaces | `host_moderate_media_v1` | RLS enabled; correctness incomplete | Privacy-sensitive | Document moderation/read contract |
| `notifications_v2` | Notification surfaces from prior audits | Notification RPCs | RLS enabled | Privacy-sensitive | Verify owner/host mark-read rules |
| `profiles` / `user_profiles` | Identity and transfer surfaces | Transfer RPCs | Full production policy coverage incomplete | Privacy-sensitive | Verify ops/admin and public field split |
| Transfer/admin rows | Ops/admin dashboard evidence | Transfer RPCs | Unknown / Needs verification | Operational/admin-sensitive | Keep behind ops RPCs |
| `app_diagnostics` | Mobile direct insert evidence | Unknown | Local evidence references anon whitelist | Operational/admin-sensitive | Keep telemetry separate from authority |

## 24. Duplicated / Split / Legacy Operations Surfaces

| Surface / helper / RPC / table | Observed role | Current / legacy / unknown | Risk if still active or authoritative | Evidence type | Recommendation |
|---|---|---|---|---|---|
| `checkin_ticket_by_id_v2` / `checkin_ticket_v2` / older check-in names | Ticket check-in | Current plus legacy/unknown variants | Inconsistent scanner authority or state machine | Production and local docs | Reconcile canonical check-in contract |
| `staff_checkin_ticket_v1` | Staff scanner check-in | Current/plausible | Scanner role boundary drift | Production evidence | Preserve and document role limits |
| `ensure_ticket_checkin_proof_v1` / `record_checkin_proof_v1` / `remove_ticket_checkin_proof_v1` | Proof mutation helpers | Unknown external reachability | Proof mutation without clear authority if callable | Prior production keyword scan | Harden later after explicit approval |
| Direct `event_staff_assignments` access | Staff assignment and staff visibility | Current/plausible | Policy complexity creates hidden authority assumptions | Local source + production RLS | Verify or wrap later |
| Dashboard scanner page vs mobile scanner | Scanner UX split | Current/plausible | Operators may assume dashboard scanning exists | Local source | Document dashboard/mobile split |
| `transition_event_status_v2` vs `control_*` functions | Lifecycle control | Split/legacy possible | Inconsistent lifecycle authority | Local docs + prior reports | Name canonical lifecycle surface |
| Ticket status dashboard helpers vs ticket RPC hooks | Ticket operations | Split/unknown | Direct revenue mutation if still active | Prior audits + local source | Reconcile ticket mutation paths |
| Ops/admin transfer RPC family | Host identity transfer | Split/unknown names | Wrong function called or missing internal gate | Prior production evidence | Document accepted ops transfer path |
| `hostos` dashboard vs `joinfolk-web` dashboard | Dashboard source ambiguity | Split-source | Audits may inspect wrong operational surface | Repository source-map docs | Name source path in future audits |

## 25. Staff-Host-Ops-Critical Invariants

- Host authority is event-scoped and cannot be inferred from UI route access alone.
- Staff assignment is event-scoped and role-scoped.
- Scanner authority is limited to check-in scanning and does not imply host, manager, venue-owner, or ops authority.
- Manager authority, if present, is explicitly documented and backend-enforced.
- Participant, ticket-holder, and checked-in are entitlement/viewer states, not operational authority roles.
- Ticket and revenue operations are backend/RPC-authoritative.
- Reservation operations are backend/RPC/RLS-authoritative.
- Venue, layout, and session operations require host, venue-owner, or backend authority.
- Lifecycle transitions are backend/RPC-authoritative.
- Media moderation authority is host/staff/ops scoped and does not expose private media publicly.
- Ops/admin actions are internally gated and auditable.
- Dashboard and mobile UI guards are not security controls.
- Public verification surfaces expose only public-safe proof information.
- Database Functions / RPC evidence is separate from Edge Function deployment evidence.
- Production SQL/RPC evidence remains stronger than local source assumptions.

## 26. Unknown / Needs Verification Surfaces

- Exact backend authority contract for `manager` staff role.
- Whether staff assignment mutation should remain direct RLS-based or move behind RPC later.
- External reachability and body-level authority of proof helper functions.
- Canonical check-in RPC surface across all dashboard/mobile paths.
- Whether any dashboard ticket or reservation status mutation bypasses canonical RPCs.
- Venue owner versus event host authority for venue/layout/session operations.
- Event session table RLS and mutation contract.
- Host notification inbox ownership and mark-read authority.
- Ops/admin route guard and RPC gate parity.
- Public verification output field contract.
- Source-path split between `hostos`, `joinfolk-web`, and mobile operational surfaces.

## 27. Staff / Host Operations Gaps / Risk Register Seeds

### SHO-GAP-001

- Domain: Staff assignment
- Current issue: Staff assignment is observed through direct table reads/upserts/deletes and relies on RLS policy correctness.
- Expected clean staff/host/ops authority contract: Staff assignment creation/removal is host/ops-authorized, event-scoped, role-scoped, and either RPC-mediated or backed by documented RLS.
- Risk: Security-sensitive and operational/admin-sensitive.
- Priority candidate: Candidate P1.
- Blocked by: Production policy review and accepted manager/scanner role contract.
- Recommended next action: Verify `event_staff_assignments` policies and document canonical assignment flow.

### SHO-GAP-002

- Domain: Proof/check-in helpers
- Current issue: Prior production keyword scan did not find obvious auth/host/staff checks in proof helper functions.
- Expected clean staff/host/ops authority contract: Proof mutation is reachable only through backend-authorized check-in/proof flows.
- Risk: Security-sensitive and privacy-sensitive.
- Priority candidate: Candidate P1; Candidate P0 only if direct unauthenticated or unauthorized reachability is proven.
- Blocked by: Production function body review and explicit patch approval.
- Recommended next action: Keep implementation blocked; verify helper reachability and grants in a dedicated proof authority audit.

### SHO-GAP-003

- Domain: Manager role
- Current issue: `manager` role is observed but action limits are not clearly documented.
- Expected clean staff/host/ops authority contract: Manager permissions are explicitly defined and backend-enforced.
- Risk: Operational/admin-sensitive.
- Priority candidate: Candidate P2.
- Blocked by: Product decision on manager powers.
- Recommended next action: Define scanner versus manager permission matrix.

### SHO-GAP-004

- Domain: Lifecycle operations
- Current issue: Canonical lifecycle RPCs and legacy/control functions may coexist.
- Expected clean staff/host/ops authority contract: One accepted lifecycle transition contract enforces host/ops authority and allowed transitions.
- Risk: Operational/admin-sensitive and product correctness.
- Priority candidate: Candidate P1.
- Blocked by: Production RPC body review and lifecycle decision.
- Recommended next action: Reconcile lifecycle RPC surface before patching lifecycle-dependent features.

### SHO-GAP-005

- Domain: Ticket and reservation operations
- Current issue: RPC-mediated paths exist, but prior audits flagged possible direct dashboard status operations and RLS reliance.
- Expected clean staff/host/ops authority contract: Revenue-sensitive ticket/reservation mutation is backend/RPC-authoritative.
- Risk: Revenue-sensitive.
- Priority candidate: Candidate P1.
- Blocked by: Direct access inventory and production policy verification.
- Recommended next action: Map ticket/reservation mutation callsites to canonical RPCs.

### SHO-GAP-006

- Domain: Venue/layout/session operations
- Current issue: Venue/layout/session operations span RPCs and direct reads, with incomplete production policy coverage for some tables.
- Expected clean staff/host/ops authority contract: Venue owner and event host boundaries are explicit and backend-enforced.
- Risk: Product correctness and revenue-sensitive.
- Priority candidate: Candidate P2.
- Blocked by: Venue/layout/session policy review.
- Recommended next action: Verify layout/session RLS and venue-owner authority.

### SHO-GAP-007

- Domain: Ops/admin transfer
- Current issue: `admin_execute_host_identity_transfer_v1` has broad grants but internal `auth_is_ops()` gate evidence.
- Expected clean staff/host/ops authority contract: Ops transfer actions are internally gated, audited, and not exposed to host/staff/public roles.
- Risk: Operational/admin-sensitive and privacy-sensitive.
- Priority candidate: Candidate P1.
- Blocked by: Auditability and related transfer RPC review.
- Recommended next action: Document accepted ops transfer path and review grants without claiming exploitability.

### SHO-GAP-008

- Domain: Public verification boundary
- Current issue: `public_verify_checkin` provenance/output contract remains unresolved.
- Expected clean staff/host/ops authority contract: Public verification is read-only and exposes only public-safe proof information.
- Risk: Privacy-sensitive.
- Priority candidate: Candidate P2.
- Blocked by: Production RPC output review.
- Recommended next action: Verify public verification fields and lifecycle/status behavior.

### SHO-GAP-009

- Domain: Dashboard/mobile UI guards
- Current issue: Host/staff/ops UI guards may be mistaken for authority.
- Expected clean staff/host/ops authority contract: UI guards mirror backend authority only.
- Risk: Security-sensitive and product correctness.
- Priority candidate: Candidate P2.
- Blocked by: Route guard inventory and backend call mapping.
- Recommended next action: Add canonical UI-mirror wording to dashboard/mobile operational docs.

## 28. Product Decisions Required

- What exact actions may `scanner` perform?
- What exact actions may `manager` perform?
- Should staff assignment management remain direct RLS-based or become RPC-mediated later?
- Which check-in RPC is canonical for host, staff, and scanner paths?
- Which proof helper functions are intended public/internal/private?
- Which dashboard path is the active host operations source for future audits and patches?
- Which ticket and reservation operations may staff perform, if any?
- Which venue/layout/session operations belong to event host versus venue owner?
- What public fields may `public_verify_checkin` return?
- What audit/logging expectations apply to ops/admin transfer actions?

## 29. Recommended Next Audits

1. Messaging / Direct Conversation Contract Audit.
2. Ops / Admin / Support Tools Authority Audit.
3. Diagnostics / Observability / Audit Log Contract Audit.

These follow because staff/host/ops boundaries intersect with private messaging, support/admin routes, operational telemetry, and auditability.

## 30. Non-Goals

- This audit does not authorize application code changes.
- This audit does not authorize dashboard, mobile, web, or Supabase tree changes.
- This audit does not create SQL, migrations, or implementation instructions.
- This audit does not connect to production.
- This audit does not prove a production vulnerability.
- This audit does not claim any feature can be removed safely.
- This audit does not make `C:\dev\hostos\supabase\migrations` a historical sole canonical source.
- This audit does not treat local Edge Function source folders as deployed production Edge Functions.

## 31. Open Questions

- Is `manager` a current product role or a planned/legacy role?
- Are staff assignments intended to be managed by hosts only, ops/admin only, or both?
- Should scanner staff be able to undo check-ins?
- Are proof helper functions intended to be callable directly by authenticated clients?
- Which dashboard repository/path is the accepted operational dashboard source for future patch work?
- Are venue owners allowed to manage event sessions and layouts independently of event hosts?
- Do host inbox notifications include staff-only or ops-only operational data?
- What public-safe field set is accepted for check-in proof verification?
- Which operational actions require audit logs?
- How should support/admin authority differ from ops/admin authority?

## 32. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `07_Audits/StaffHostOperationsAuthorityAudit.md` was created/modified.

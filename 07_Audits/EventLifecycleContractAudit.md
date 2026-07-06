# Event Lifecycle Contract Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false

## 2. Purpose

This document is an event lifecycle contract audit for JoinFolk. It maps observed lifecycle state vocabulary, publish/readiness behavior, transition authority, visibility, commerce availability, venue buyer availability, check-in timing, media/gallery/voting availability, and frontend status normalization.

This is not an implementation plan, patch plan, cleanup plan, migration plan, accepted vulnerability list, or release readiness report. It does not authorize adding, removing, or changing features. It does not authorize modifying backend/RPC/RLS/storage/auth, any Supabase tree, dashboard code, mobile code, web code, or application code.

## 3. Audit Scope

Read-only evidence was drawn from:

- Handbook repository: `C:\dev\joinfolk-engineering-handbook`
- Platform/backend candidate repository: `C:\dev\hostos`
- Web/dashboard repository: `C:\dev\joinfolk-web`
- Mobile repository: `C:\dev\hostos\apps\mobile`

The audit focused on:

- Event status vocabulary and aliases across mobile, dashboard, web/public, and backend migrations.
- Publish/readiness, lifecycle transition, check-in timing, feed visibility, event detail rendering, module access, commerce availability, and venue buyer lifecycle dependencies.
- Backend Database Functions / RPC candidates and table/RLS authority at a high level.
- Prior production SQL/RPC evidence supplied in handbook reports.

Current system context preserved:

- Future accepted Supabase migration target: `C:\dev\hostos\supabase\migrations`.
- This target is not proof of historical sole canonical source.
- Split-source migration history remains unresolved.
- Production SQL/RPC evidence remains stronger than local source assumptions.
- Database Functions / RPC evidence is separate from Edge Function deployment evidence.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- No backend patch or migration is authorized by this audit.
- Product Contract Audit identified lifecycle/status normalization as split-source / duplicated.
- Authority Boundary Audit identified lifecycle transitions and visibility as authority-sensitive.
- Commerce + Ticketing Contract Audit linked commerce availability to lifecycle.
- Venue Buyer Flow Contract Audit linked seat-selection mode, venue-layout binding, commerce mode legality, and buyer interactivity to lifecycle.

## 4. Event Lifecycle Contract Summary

JoinFolk appears to use a five-state canonical event lifecycle for active product surfaces:

- `draft`
- `published`
- `live`
- `ended`
- `archived`

Observed legacy or alias states include:

- `active` as a live-like alias.
- `closed` as an ended-like alias.
- `cancelled` as an archived-like alias in mobile and dashboard normalization.
- `canceled` in reservation and venue reservation status vocabularies, not clearly as event lifecycle state.
- `deleted` as a terminal or hidden state in frontend normalization.

The desired clean lifecycle contract is:

- Backend/RPC/RLS/auth is authoritative for lifecycle mutation.
- Frontend status helpers normalize for display and navigation only.
- Lifecycle state is separate from visibility state.
- Lifecycle state is separate from module configuration.
- Lifecycle state is separate from commerce mode and commerce availability.
- Commerce, venue buyer, check-in, media, voting, reminders, notifications, and public/share behavior are defined as lifecycle-dependent contracts, not ad hoc UI conditions.

Observed implementation reality is mostly backend-oriented but not fully deterministic. Positive evidence exists for `publish_event_with_groups_and_snapshot_v2`, `_event_publish_readiness_guard_v1`, and `transition_event_status_v2`. Main risks are repeated lifecycle function definitions, `status` vs `event_state` dual-read/dual-write behavior, frontend alias normalization in multiple places, incomplete production verification of live function bodies, and inconsistent lifecycle assumptions across feed, modules, commerce, check-in, public share, and memory surfaces.

## 5. Lifecycle Vocabulary and Alias Map

| Observed status / alias | Observed surface | Likely canonical meaning | Risk if treated incorrectly | Recommendation |
| --- | --- | --- | --- | --- |
| `draft` | Mobile lifecycle helper, dashboard types, create flow, publish RPC guard | Draft/editable, not public, publishable after readiness | Draft could leak into feeds or allow purchase if visibility and commerce are confused | Preserve as canonical state |
| `published` | Mobile/dashboard feed, publish RPC, transition RPC, commerce guards | Public or shared upcoming event, can be started, can allow purchase/reservation depending contract | Published may be treated as live for scanner/media if not separated | Preserve as canonical state |
| `live` | Mobile/dashboard lifecycle, transition RPC, check-in, feed sort | Event is actively running; check-in and live interactions can be available | Live may trigger check-in/media/voting too broadly if backend guards differ | Preserve as canonical state |
| `ended` | Mobile/dashboard lifecycle, transition RPC, memory layer, archive action | Event naturally completed; memory/summary/review surfaces may be available | Ended could still allow commerce or check-in if rules are incomplete | Preserve as canonical state |
| `archived` | Mobile/dashboard lifecycle, archive transition, profile/archive surfaces | Terminal archived summary state | Archived could be treated as ended memory even when cancelled | Preserve as canonical state, define memory behavior |
| `active` | Legacy feed/backend migrations, dashboard staff events, mobile status type | Live-like legacy alias | If interpreted as published, scanner and commerce behavior can drift | Reconcile as legacy alias |
| `closed` | Mobile lifecycle helper, dashboard normalizer, older backend status RPC | Ended-like legacy alias | Closed may fall through to draft if helper misses alias | Reconcile as legacy alias |
| `cancelled` | Mobile lifecycle and dashboard normalizers | Archived-like cancellation state; event did not happen | If treated as ended, memory/review surfaces can appear incorrectly | Document cancellation contract |
| `canceled` | Dashboard reservation/venue reservation status; i18n strings | Reservation/booking cancellation; event lifecycle use unconfirmed | Event and reservation cancellation vocabularies may be conflated | Keep distinct from event status unless verified |
| `deleted` | Mobile lifecycle fallback, dashboard normalizer | Terminal hidden/deleted state | Deleted event may be rendered as archived or draft inconsistently | Needs verification |
| `event_state` | Mobile `resolveEventState`; hostos later migrations | Server-authoritative lifecycle shadow/read model | `status` and `event_state` drift can create split truth | Candidate P1 review |

## 6. Active Lifecycle Flow Map

| Flow | Observed surface | Backend/RPC candidates | Contract status | Notes |
| --- | --- | --- | --- | --- |
| Draft creation | Dashboard create API and mobile create/publish flow | Direct `events` insert plus RLS; publish RPC later | Mostly deterministic | Dashboard creates `status = draft`; backend insert authority depends on RLS/policies. |
| Publish | Mobile `publishEventV2`, dashboard `publishEventDraft` | `publish_event_with_groups_and_snapshot_v2`, `_event_publish_readiness_guard_v1` | Mostly deterministic | Positive local evidence; production active body still depends on supplied production verification. |
| Start / go live | Mobile control and dashboard API | `transition_event_status_v2` with action `start` | Mostly deterministic | Later migration evidence dual-writes `status` and `event_state`, opens check-in, and sets `live_started_at`. |
| End | Mobile control and dashboard action model | `transition_event_status_v2` with action `end` | Mostly deterministic | Later migration evidence sets `ended_at`, closes check-in, and materializes winner. |
| Archive | Mobile control and dashboard action model | `transition_event_status_v2` with action `archive` | Mostly deterministic | Archive expected from ended only; mobile treats archive-from-archived as noop. |
| Unpublish | Mobile control and dashboard API | `transition_event_status_v2` with action `unpublish` | Backend authority unclear | Later migration evidence blocks unpublish when tickets exist; production active body needs verification. |
| Cancel | Legacy/control functions and aliases | `control_cancel_event`, older `control_unpublish_event`; exact canonical path unclear | Split / Needs verification | Cancelled maps to archived in UI but is not in the main transition action enum. |
| Revert live | Mobile control route language and handbook context | Unknown / Needs verification | Split / Needs verification | UI mentions live revert grace; accepted backend path not established in this audit. |
| Delete/remove | Mobile lifecycle includes deleted fallback; dashboard delete/remove not canonical | Unknown / Needs verification | Unknown | Must not be inferred from UI alone. |

## 7. Lifecycle Authority Matrix

| Domain | Flow / action | Current observed surface | Expected authority owner | Active RPC / data path candidates | Determinism status | Risk class | Evidence source | Recommendation |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Draft save | Create/edit draft event fields | Dashboard create/edit, mobile create/edit | Backend/RPC/RLS/auth | `events` insert/update, publish RPC later | Mostly deterministic | Product correctness | Dashboard API/source scan | Document canonical contract |
| Publish | Publish draft with visibility/groups/snapshot/readiness | Mobile and dashboard call publish RPC | Backend/RPC/RLS/auth | `publish_event_with_groups_and_snapshot_v2`, `_event_publish_readiness_guard_v1` | Mostly deterministic | Security-sensitive / privacy-sensitive | Mobile/dashboard API; hostos migrations | Preserve and verify production body |
| Go live | Start published event | Mobile control, dashboard API, workstation | Backend/RPC/RLS/auth | `transition_event_status_v2` action `start` | Mostly deterministic | Product correctness / security-sensitive | Mobile control; dashboard API; hostos migrations | Preserve, document transition matrix |
| End event | End live event | Mobile control, dashboard cards | Backend/RPC/RLS/auth | `transition_event_status_v2` action `end` | Mostly deterministic | Product correctness | Hostos migrations; dashboard/mobile helpers | Preserve, verify final body |
| Archive event | Archive ended event | Mobile control, dashboard cards | Backend/RPC/RLS/auth | `transition_event_status_v2` action `archive` | Mostly deterministic | Product correctness / privacy-sensitive | Hostos migrations; UI helpers | Preserve |
| Unpublish | Move published event back to draft | Mobile control, dashboard API | Backend/RPC/RLS/auth | `transition_event_status_v2` action `unpublish` | Backend authority unclear | Revenue-sensitive / privacy-sensitive | Hostos migrations; mobile control | Verify ticket/reservation blockers |
| Cancel event | Cancel without natural completion | UI alias handling; older functions | Backend/RPC/RLS/auth | `control_cancel_event`, older control functions | Split / duplicated | Product correctness / privacy-sensitive | Focused backend report; source search | Needs decision |
| Event visibility | Feed/public/group/private visibility | Feed RPCs and client filters | Backend/RPC/RLS/auth | `get_home_feed_events_v2`, `get_discover_events_v2`, `get_rising_events_v1`, event policies | Split / duplicated | Privacy-sensitive | Mobile feed; production RLS evidence | Reconcile |
| Module availability | Gallery/voting/poll/checklist/tickets/reservations | Mobile module access, dashboard workstation | Backend for actions; UI mirror for display | Module RPCs, event module tables, action RPCs | Split / duplicated | Product correctness | Mobile moduleAccess; dashboard eventWorkstation | Document canonical contract |
| Commerce availability | Purchase/order/reservation lifecycle | Mobile buyer helpers, backend purchase guards | Backend/RPC/RLS/auth | `create_commerce_order_v1`, `purchase_event_ticket_v5/v4`, reservation RPCs | Mostly deterministic but incomplete | Revenue-sensitive | Commerce audit; hostos migrations | Verify active paths |
| Venue buyer availability | Seat/standing/session selection | Mobile area/seat/session picker, dashboard readiness | Backend/RPC/RLS/auth | `get_event_seat_availability_v1`, product/seat/order RPCs | Split / UI-heavy | Revenue-sensitive | Venue Buyer Flow Audit | Reconcile with lifecycle |
| Check-in | Open/check in/close scanner window | Mobile control/scanner, dashboard attendee pages | Backend/RPC/RLS/auth | `transition_event_status_v2`, `checkin_ticket_by_id_v2`, `staff_checkin_ticket_v1`, legacy functions | Mostly deterministic current path | Security-sensitive | Focused backend report; source scan | Preserve current scanner path, review legacy |
| Media/gallery | Upload/read memory wall | Mobile module access and media helpers | Backend/RPC/RLS/storage/auth | Event media RPCs, storage policies | Backend authority unclear | Privacy-sensitive | Authority audit; storage evidence | Harden later / ADR |
| Voting/poll/winner | Vote while live, show results/winner | Mobile module access, transition winner materialization | Backend/RPC/RLS/auth | Poll/voting RPCs, `resolve_event_winning_media_v1` candidate | Mostly deterministic | Product correctness | Mobile moduleAccess; hostos migrations | Verify finalization contract |
| Notifications | Publish/invite/reminder lifecycle events | Publish RPC creates notifications; reminders local/RPC | Backend/RPC/auth | Notification DB functions; no Edge Functions deployed visibly | Split / duplicated | Privacy-sensitive | Hostos publish migration; production reports | Notification audit |
| Public share | Public event share/deep link | Web/dashboard function and public app route | Backend/RPC/RLS/auth | `get_event_share`, event policies | Backend authority unclear | Privacy-sensitive | Web source scan; production reports | Public share audit |

## 8. Transition Contract Assessment

| Transition | Expected actor | Expected backend authority | Current observed evidence | Determinism status | Risk | Recommendation |
| --- | --- | --- | --- | --- | --- | --- |
| `draft -> published` | Event host, maybe ops/admin | Publish RPC with readiness, visibility, group ownership, tier checks | Mobile/dashboard call `publish_event_with_groups_and_snapshot_v2`; hostos guard checks draft, host, readiness, tier for public visibility, group ownership | Mostly deterministic | Privacy/product correctness | Preserve and verify live function body |
| `published -> draft` | Event host, maybe ops/admin | Transition RPC action `unpublish` with blockers | Hostos later migration blocks unpublish if tickets exist; mobile/dashboard expose unpublish action | Backend authority unclear | Revenue/privacy risk if tickets/reservations exist | Candidate P1 verification |
| `published -> live` | Event host, maybe staff/ops by decision | Transition RPC action `start` | Hostos migration requires published, sets live state and check-in fields; mobile/dashboard action guards mirror | Mostly deterministic | Check-in/media/commercial availability drift | Preserve and document |
| `live -> ended` | Event host, maybe ops/admin | Transition RPC action `end` | Hostos migration sets ended fields, closes check-in, resolves winner candidate | Mostly deterministic | Check-in and memory/winner correctness | Preserve and verify finalization contract |
| `ended -> archived` | Event host, maybe ops/admin | Transition RPC action `archive` | Hostos migration requires ended and sets archived fields; mobile treats archived repeat as noop | Mostly deterministic | Public/profile/memory visibility drift | Preserve |
| `published/live -> cancelled` | Event host, maybe ops/admin | Dedicated cancel RPC or transition action, not clearly canonical | UI aliases map cancelled to archived; focused report found live `control_cancel_event` missing search_path and broad grants | Split / Needs verification | Cancelled may enable or suppress wrong surfaces | Needs decision |
| `live -> published` revert | Event host within strict grace if accepted | Backend RPC with grace, no check-ins/tickets blockers | UI language mentions live revert grace; local archived SQL mentions migration concept; accepted active backend path not confirmed | Unknown / Needs verification | Event state rollback can affect tickets/check-ins/feed | Needs verification |
| `any -> deleted` | Host or ops/admin by decision | Backend/RPC/RLS/auth | Deleted appears in frontend normalizers; no accepted backend path established here | Unknown / Needs verification | Deletion can affect privacy, records, wallet, and audit history | Needs decision |

## 9. Publish / Readiness Contract Assessment

Expected clean contract:

- Publish is a backend-authoritative transition from draft to published.
- Publish validates host ownership, draft status, visibility, group audience authority, poster snapshot requirements, tier/public visibility constraints, and venue/ticket readiness where relevant.
- UI readiness and workstation helpers are mirrors only.

Observed evidence:

- Mobile publish path uses `publish_event_with_groups_and_snapshot_v2` and documents no direct status update outside the RPC.
- Dashboard publish path uses the same RPC and requires a poster snapshot URL before publishing.
- Hostos publish readiness guard checks commerce mode, active ticket products, reserved seating layout, sections, seats, and product-section mapping.
- Hostos publish RPC patch updates `status` and `event_state` to `published` and handles share group and notification side effects.
- Dashboard workstation derives readiness from title, start time, description, commerce mode, ticket products, admission model, layout, sessions, and buyer preview.

Assessment:

- Lifecycle status: Product-critical.
- Determinism status: Mostly deterministic, with frontend readiness mirrors and backend guard evidence.
- Authority owner: Backend/RPC/RLS/auth.
- Recommendation: Preserve the publish RPC pattern, document readiness as backend-authoritative, and verify live production function body before treating local migration details as production truth.

## 10. Visibility Contract Assessment

Expected clean contract:

- Lifecycle state decides whether an event is draft/upcoming/live/post-event/archived.
- Visibility decides who may see it: public, friends/group, invite-only/private, host, staff, participant, ticket holder, or ops/admin.
- Feed/public share visibility must be backend-authoritative.

Observed evidence:

- Mobile feed comments state server-side feed RPCs should filter `published` and `live`, with client-side defense-in-depth filters.
- Mobile feed excludes some system event types from default feeds and treats ended-tail as deferred until server RPC changes.
- Mobile lifecycle helper has `canViewerSeeEventInFeed()` for public/invite/private visibility, but it is frontend logic.
- Dashboard/public web share functions reference public share behavior and `get_event_share` style constraints.
- Production evidence confirms events table RLS is enabled and event visibility policies exist, but full policy correctness needs deeper review.

Assessment:

- Lifecycle status: Product-critical.
- Determinism status: Split / duplicated.
- Authority owner: Backend/RPC/RLS/auth.
- Recommendation: Treat frontend feed filters as mirrors/defense-in-depth only. Create a dedicated visibility/public share contract audit.

## 11. Commerce Availability by Lifecycle

| Question | Current observed evidence | Assessment |
| --- | --- | --- |
| Can tickets be configured in draft? | Dashboard workstation/product setup supports ticket readiness while draft; publish guard validates products/layout before publish. | Yes as configuration, backend mutation authority still depends on product RPC/RLS. |
| Can tickets be purchased in draft? | Backend commerce guards in hostos migrations reject event status outside `published` or `live`; mobile ticketSales module hides ended/archived but not draft by itself. | Expected no; backend evidence positive but production body verification needed. |
| Can tickets be purchased in published? | Commerce/order guards allow `published`; mobile feed/buyer surfaces consider published active. | Expected yes if product/sale windows/capacity/entitlement allow. |
| Can tickets be purchased in live? | Commerce/order guards allow `live`; mobile module access keeps ticketSales active until ended/archived. | Expected yes unless product decision says sales close at live. Needs explicit contract. |
| Can tickets be purchased in ended/archived/cancelled? | Mobile module access hides ticketSales for ended/archived; commerce guards reject non-published/live; cancelled maps to archived in UI. | Expected no. Backend production parity of all purchase paths still needs review. |
| How do reservations behave? | Mobile reservation module hides ended/archived and allows authenticated interaction while active; backend reservation evidence allows `published`/`live` in older/later migrations. | Expected available published/live; lifecycle contract needs final product decision. |
| How do stale orders interact with lifecycle? | Order functions include pending/expiration concepts; active lifecycle/order interaction not fully reviewed. | Unknown / Needs verification. |
| How do sale windows interact? | Product `sale_starts_at` and `sale_ends_at` exist; backend commerce guards check sale windows. | Backend evidence positive for purchase path candidates; coverage incomplete. |

## 12. Venue Buyer Availability by Lifecycle

Expected clean contract:

- Hosts may configure venue layout, seating mode, products, sections, and sessions while draft.
- Published/live buyer surfaces may show venue buyer flows when product, layout, seat/standing, session, and availability contracts are satisfied.
- Ended/archived/cancelled events should not allow new buyer acquisition.
- UI preview/geometry remains presentation only; final seat/standing/product availability must be backend-authoritative.

Observed evidence:

- Venue Buyer Flow Audit found dashboard readiness and product-section mapping tied to publish readiness.
- Hostos publish readiness guard blocks exact-seat publication when layout/sections/seats/products/mappings are missing.
- Hostos transition start path can re-check venue readiness for reserved ticket sales before moving to live.
- Mobile buyer flows rely on `get_event_seat_availability_v1`, product reads, area/seat/session routes, and purchase/order RPCs.

Assessment:

- Lifecycle status: Product-critical.
- Determinism status: Split / duplicated.
- Authority owner: Backend/RPC/RLS/auth for final availability and purchase; dashboard/mobile UI as mirrors.
- Recommendation: Link lifecycle decisions to VBF-GAP-003, VBF-GAP-004, VBF-GAP-007, and CTC purchase source-of-truth review.

## 13. Check-in / Staff Scanner Availability by Lifecycle

Expected clean contract:

- Check-in mutation is allowed only for live events when check-in is opened and not closed.
- Host/staff/scanner authority, event/ticket scoping, ticket status, and state machine checks must be backend-enforced.
- UI check-in buttons are mirrors only.

Observed evidence:

- Mobile lifecycle helper derives check-in state as open only when status is `live`, starts/ends exist, event has not ended/archived/deleted, `checkin_opened_at` exists, and `checkin_closed_at` is absent.
- Hostos `transition_event_status_v2` later migration evidence opens check-in on `start` and closes check-in on `end`.
- Focused backend report supports positive control for `checkin_ticket_by_id_v2`, including constrained anon/public grants and body checks for event host/status/ticket/code/status state machine.
- Dashboard attendee/ticket pages show check-in actions only when event status is `live`.
- Legacy/proof-related check-in functions remain a separate concern.

Assessment:

- Lifecycle status: Product-critical.
- Determinism status: Mostly deterministic for current scanner path.
- Authority owner: Backend/RPC/RLS/auth.
- Recommendation: Preserve current scanner positive controls; review legacy/proof functions separately.

## 14. Media / Gallery / Voting Availability by Lifecycle

Expected clean contract:

- Gallery/media upload, voting, polls, winners, and memory wall behavior must have lifecycle-specific backend authority.
- UI module access can mirror display and interaction state.
- Ended/archived events may show memory/winner/read-only summaries if product-approved.
- Cancelled events should not automatically get memory semantics unless explicitly decided.

Observed evidence:

- Mobile module access allows gallery viewing based on participant/pro context and upload only while live with operational or checked-in privilege.
- Mobile voting can remain viewable after ended/archived for tallies/winners, but interaction is live-only.
- Mobile poll and checklist hide on ended/archived.
- Mobile render model shows memory layer for ended/archived but suppresses memory for raw cancelled events.
- Hostos later transition function evidence resolves or stores winning media on end/archive.
- Production event_media RLS/policy surface exists, but correctness and final lifecycle semantics need review.

Assessment:

- Lifecycle status: Optional to Product-critical depending module.
- Determinism status: Mostly deterministic in mobile, backend authority incomplete.
- Authority owner: Backend/RPC/RLS/storage/auth.
- Recommendation: Document lifecycle-specific module contract and verify media/voting RPC authority.

## 15. Dashboard Host Action Map

Observed dashboard host action behavior:

- Dashboard API creates events with `status = draft`.
- Dashboard API publishes through `publish_event_with_groups_and_snapshot_v2`.
- Dashboard API transitions via `transition_event_status_v2` with actions `unpublish`, `start`, `end`, and `archive`.
- Dashboard event workstation derives primary/secondary actions and readiness from commerce mode, tier, product count, admission model, layout status, sessions, and event status.
- Dashboard normalizes `active -> live`, `closed -> ended`, `archive -> archived`, and `cancelled -> archived`.
- Dashboard check-in actions are displayed for approved tickets/reservations only when event status is `live`.
- Dashboard ops page includes a visible SQL-like status override snippet in UI text; this audit does not treat that as approved production authority or implementation guidance.

Assessment:

- Dashboard is mostly aligned with backend lifecycle RPCs for publish and transition.
- Dashboard readiness is useful as UI guidance but cannot be final authority.
- Some dashboard status normalization and direct event/ticket data paths remain part of broader direct access/RLS audit scope.

## 16. Mobile Event State / Viewer State Map

Observed mobile lifecycle behavior:

- `deriveEventPhase()` maps raw status and fallback timestamps into `DRAFT`, `UPCOMING`, `LIVE`, `ENDED`, `ARCHIVED`, `DELETED`, or `INVALID`.
- `resolveEventState()` prefers `event_state` if present and valid, then falls back to raw status normalization.
- Mobile control uses `transitionEventStatusV2()` for `unpublish`, `start`, `end`, and `archive`.
- Mobile publish uses `publish_event_with_groups_and_snapshot_v2` and explicitly avoids direct status updates.
- Mobile feed filters visible events to `published` and `live` as defense-in-depth.
- Mobile event detail render model derives CTAs, modules, memory, host tools, and staff tools from viewer role, event state, persona, event type, and commerce.
- Mobile check-in state requires live status and explicit check-in-open signal.

Assessment:

- Mobile has the richest lifecycle render model.
- Mobile frontend model should be treated as UI and UX contract evidence, not backend authority.
- `status` vs `event_state` fallback is useful during migration but creates determinism risk until canonical server field ownership is documented.

## 17. Backend RPC / RLS Authority Evidence Map

Production evidence from prior reports:

- `events` exists in production and has RLS enabled.
- Production event policies exist, including host insert/update/delete policies and public/authenticated select visibility policies.
- Production policy correctness and redundancy still need deeper review.
- Target sensitive tables tied to lifecycle-dependent flows, including tickets, reservations, event_media, event_staff_assignments, and notification tables, have RLS enabled.
- Production SQL/RPC evidence remains stronger than local source assumptions.

Local-source-only backend evidence:

- `publish_event_with_groups_and_snapshot_v2` is the observed publish authority in mobile and dashboard source.
- `_event_publish_readiness_guard_v1` exists in hostos migration evidence and validates exact-seat/ticket readiness before publish.
- `transition_event_status_v2` is repeatedly defined in hostos migrations and is the observed transition authority for `unpublish`, `start`, `end`, and `archive`.
- Later hostos migration evidence dual-writes `status` and `event_state`.
- Purchase/order and reservation RPC candidates reject event statuses outside `published` and `live` in observed hostos migrations.
- Feed RPCs are expected to return `published` and `live` events, with client-side filters as defense-in-depth.

Unknown / Needs verification:

- Active production body of `transition_event_status_v2`.
- Active production body of `publish_event_with_groups_and_snapshot_v2`.
- Whether production currently stores and consistently uses `event_state`.
- Whether all direct event mutation paths are blocked or policy-controlled.
- Whether cancel/delete/revert-live have active backend authority paths.
- Whether all lifecycle-dependent commerce/order/reservation/media/voting paths consistently enforce lifecycle.

## 18. Direct Data Access / RLS Reliance Map

| Data surface | Observed direct access relevance | Prior production authority evidence | Current classification | Recommendation |
| --- | --- | --- | --- | --- |
| `events` | Mobile/dashboard/public reads; dashboard create/edit; lifecycle fields | RLS enabled; policies exist | RLS authority likely, policy correctness separate | Direct Data Access / RLS Reliance Audit |
| `event_modules` | Module availability and commerce mode derivation | Production evidence incomplete in supplied summaries | Unknown / Needs verification | Verify module RLS/RPC authority |
| `event_ticket_products_v1` | Publish readiness, ticket setup, purchase availability | Production evidence not fully summarized | Unknown / Needs verification | Verify product lifecycle/publish authority |
| `tickets` | Unpublish blocker, check-in, wallet, purchase state | RLS enabled; zero direct policies in focused evidence | RPC-only reliance likely | Verify all RPC lifecycle guards |
| `reservations` | Reservation availability and status | RLS enabled; policy surface exists | RLS/RPC authority likely | Review lifecycle guard coverage |
| `event_media` | Memory/gallery lifecycle | RLS enabled; policy surface exists | RLS authority likely; correctness separate | Media lifecycle audit |
| `event_staff_assignments` | Staff scanner and staff event lists | RLS enabled; policies exist | RLS authority likely | Staff scanner authority review |
| `venue_layouts` and layout tables | Publish readiness and venue buyer lifecycle | Not covered in supplied production RLS summary | Unknown / Needs verification | Direct Data Access / RLS Reliance Audit |
| `event_sessions_v1` | Session buyer availability | Not covered in supplied production RLS summary | Unknown / Needs verification | Venue buyer/session audit |
| `notifications_v1/v2` | Publish notifications, reminders | v2 RLS confirmed; v1 usage appears in local migration evidence | Split / Needs verification | Notification contract audit |

## 19. Frontend UI Gate and Helper Map

| UI/helper surface | Observed role | Acceptable as UI guidance? | Risk if treated as authority | Recommendation |
| --- | --- | --- | --- | --- |
| Mobile `deriveEventPhase()` | Maps raw status/timestamps to phase | Yes | Can drift from backend transition truth | Keep as display helper |
| Mobile `resolveEventState()` | Prefers `event_state`, falls back to status | Yes | `status`/`event_state` mismatch can cause inconsistent UI | Document canonical server field |
| Mobile `deriveCheckinState()` | Derives check-in open/closed for UI | Yes | Scanner mutation must still use backend checks | Preserve as mirror |
| Mobile `moduleAccess` | Maps lifecycle to module view/interact | Yes | Module interactions may appear allowed without backend authority | Verify module RPC guards |
| Mobile feed filters | Defense-in-depth filter for published/live | Yes | Client filter cannot protect privacy if RPC/RLS returns too much | Backend visibility audit |
| Mobile render model | CTA/memory/host/staff tool display | Yes | UI CTAs cannot define lifecycle permission | Preserve as presentation contract |
| Dashboard `normalizeEventStatus()` | Normalizes raw statuses for display/actions | Yes | Multiple normalizers can disagree on aliases | Reconcile |
| Dashboard `eventWorkstation` | Readiness and host action IA | Yes | Readiness can diverge from backend publish guard | Keep as mirror |
| Dashboard check-in button gating | Shows check-in only when event live | Yes | Backend scanner RPC must enforce | Preserve |
| Public event share gate | Allows published/live or share-eligible surfaces | Yes as presentation | Public share requires backend/RLS/RPC authority | Public share audit |

## 20. Duplicated / Split / Legacy Status Surfaces

| Surface / helper / RPC / status alias | Observed role | Current/legacy/unknown | Risk if still active or authoritative | Evidence type | Recommendation |
| --- | --- | --- | --- | --- | --- |
| `status` column | Main raw lifecycle/status field | Current | Mixed semantics if `event_state` also exists | Production/local evidence | Keep but document canonical relation |
| `event_state` field | Server-authoritative lifecycle shadow/read model in local evidence | Current candidate | Drift from `status` creates split truth | Local hostos/mobile evidence | Candidate P1 verification |
| `transition_event_status_v2` | Lifecycle transition authority | Current candidate; repeated definitions | Active body unclear without production verification | Local migrations + app calls | Verify production body |
| `publish_event_with_groups_and_snapshot_v2` | Publish authority | Current candidate; repeated definitions | Active body and guard inclusion must be verified | Local migrations + app calls | Verify production body |
| `publish_event` / `publish_event_with_groups` | Older publish functions | Legacy / Unknown | Legacy callable surfaces may bypass current guard | Backend reports | Review grants/usage |
| `update_event_status` | Older generic status RPC | Legacy / Unknown | Could allow old statuses such as active/closed | Local migration evidence | Candidate cleanup later if unused |
| `control_unpublish_event` | Older unpublish/cancel behavior | Legacy / Unknown | May encode cancel/archive differently | Local migration evidence | Review reachability |
| `control_cancel_event` | Cancel event function | Unknown / live function missing search_path evidence | Cancellation contract unclear | Focused backend report | Candidate P1 hardening/review |
| `control_end_event` | End event function | Unknown / legacy | May overlap with transition RPC | Focused backend report | Review reachability |
| `active` status alias | Live-like status in older feed/control code | Legacy alias | Feed/check-in rules can diverge | Source scan | Reconcile |
| `closed` status alias | Ended-like status | Legacy alias | Can fall through differently across helpers | Source scan | Reconcile |
| `cancelled` alias | Archived-like event cancellation | Current alias / contract unclear | Memory/share/commerce suppression may differ | Mobile/dashboard helpers | Needs decision |
| `deleted` alias | Hidden terminal state | Unknown | Public/profile/feed behavior unclear | Frontend normalizers | Needs verification |

## 21. Lifecycle-Critical Invariants

The following invariants should be backend-authoritative:

- Draft events are not public/feed visible except to authorized host/ops contexts.
- Publish requires host authority, draft status, accepted visibility, allowed group/share targets, required poster snapshot, tier rules, and readiness checks.
- Lifecycle transition mutation uses one accepted state machine.
- Start/go-live only happens from published.
- End only happens from live.
- Archive only happens from ended unless a separate cancel/archive contract explicitly allows another path.
- Unpublish cannot invalidate ticket, reservation, order, claim, check-in, or public/share state.
- Cancelled events do not receive ended/memory semantics unless product explicitly accepts that.
- Deleted/removed events are not visible except through approved audit/admin paths.
- Ticket purchase, reservation, venue buyer flow, and order creation are unavailable in draft, ended, archived, cancelled, and deleted states unless product explicitly defines exceptions.
- Check-in mutation is live-only and requires backend host/staff/scanner authority plus ticket state checks.
- Media upload, voting, poll interaction, checklist interaction, and winner finalization follow one lifecycle contract across mobile, dashboard, and backend.
- Feed/public share visibility must be backend-authoritative and not dependent on client filtering.
- `status` and `event_state` cannot diverge without a documented reconciliation rule.

## 22. Missing / Incomplete Lifecycle Feature Candidates

These are candidates requiring product and authority decisions, not approved feature work:

- Canonical lifecycle state machine including cancel/delete/revert-live behavior.
- Canonical relation between `status`, `event_state`, and timestamp fields.
- Accepted cancellation semantics, especially memory wall, winner, feed, share, ticket, and reservation behavior.
- Accepted unpublish blockers for tickets, reservations, orders, claims, check-ins, media, and notifications.
- Accepted live revert contract, if the feature remains intended.
- Accepted ended-tail behavior for feeds, if desired.
- Public share contract for ended/archived/cancelled events.
- Lifecycle-specific media/gallery/voting/poll/checklist contract.
- Lifecycle-specific commerce availability for published vs live events.
- Notification/reminder lifecycle trigger contract.

## 23. Lifecycle Gaps / Risk Register Seeds

### ELC-GAP-001

- Gap ID: ELC-GAP-001
- Domain: Lifecycle state machine
- Current issue: Canonical states exist in practice, but aliases and transition exceptions are spread across mobile, dashboard, and backend migrations.
- Expected clean lifecycle contract: One accepted state machine defines states, aliases, allowed transitions, actors, blockers, and side effects.
- Risk: Product correctness and security-sensitive authority drift.
- Priority candidate: Candidate P1.
- Blocked by: Production body verification for lifecycle RPCs and product decision for cancel/revert/delete.
- Recommended next action: Create a lifecycle state machine decision record.

### ELC-GAP-002

- Gap ID: ELC-GAP-002
- Domain: `status` vs `event_state`
- Current issue: Mobile prefers `event_state` when present, while dashboard still mainly selects/normalizes raw `status`; local backend evidence dual-writes both.
- Expected clean lifecycle contract: One server-authoritative lifecycle field or a documented dual-field reconciliation rule.
- Risk: Split truth across feed, detail, commerce, and scanner surfaces.
- Priority candidate: Candidate P1.
- Blocked by: Production schema/function verification and migration-source history review.
- Recommended next action: Verify production `events` lifecycle columns and active RPC body behavior.

### ELC-GAP-003

- Gap ID: ELC-GAP-003
- Domain: Publish readiness
- Current issue: Backend readiness guard exists locally, while dashboard/mobile also derive readiness for UI.
- Expected clean lifecycle contract: Backend publish guard is authoritative; UI readiness mirrors exact backend codes.
- Risk: Host sees publish-ready UI while backend rejects, or UI misses backend blockers.
- Priority candidate: Candidate P1.
- Blocked by: Production publish RPC body verification and readiness-code mapping.
- Recommended next action: Publish readiness parity audit.

### ELC-GAP-004

- Gap ID: ELC-GAP-004
- Domain: Unpublish and rollback
- Current issue: Unpublish exists as transition action and later local evidence blocks unpublish with tickets, but reservation/order/media blockers are not fully mapped.
- Expected clean lifecycle contract: Unpublish blockers cover all persisted attendance, commerce, reservation, media, notification, and public share consequences.
- Risk: Revenue-sensitive and privacy-sensitive state rollback.
- Priority candidate: Candidate P1.
- Blocked by: Production transition body review and product decision.
- Recommended next action: Unpublish blocker matrix.

### ELC-GAP-005

- Gap ID: ELC-GAP-005
- Domain: Cancelled/canceled semantics
- Current issue: `cancelled` maps to archived in UI; `canceled` appears in reservation vocabularies; canonical event cancellation path is unclear.
- Expected clean lifecycle contract: Event cancellation is distinct from reservation cancellation and defines visibility, commerce, memory, notifications, and audit behavior.
- Risk: Product correctness and privacy-sensitive ambiguity.
- Priority candidate: Candidate P1 / Unknown.
- Blocked by: Product decision and production RPC review.
- Recommended next action: Cancellation semantics ADR.

### ELC-GAP-006

- Gap ID: ELC-GAP-006
- Domain: Feed and public visibility
- Current issue: Feed RPCs and client filters both enforce published/live visibility, but full production policy correctness and public share lifecycle behavior remain incomplete.
- Expected clean lifecycle contract: Feed, discover, rising, public share, and profile archive visibility are backend-authoritative.
- Risk: Privacy-sensitive.
- Priority candidate: Candidate P1.
- Blocked by: Event policy/feed RPC/public share review.
- Recommended next action: Public Web / Share Surface Audit.

### ELC-GAP-007

- Gap ID: ELC-GAP-007
- Domain: Commerce availability
- Current issue: Purchase/order guards appear to allow published/live only, but multiple purchase/order paths and overloads remain.
- Expected clean lifecycle contract: Every active commerce/reservation/acquisition path enforces the same lifecycle availability rule.
- Risk: Revenue-sensitive.
- Priority candidate: Candidate P1.
- Blocked by: Commerce purchase/order source-of-truth review.
- Recommended next action: Link to CTC-GAP-002 and CTC-GAP-003.

### ELC-GAP-008

- Gap ID: ELC-GAP-008
- Domain: Venue buyer availability
- Current issue: Publish/start readiness touches reserved layouts and product-section mappings, but full buyer lifecycle parity is not proven.
- Expected clean lifecycle contract: Venue buyer setup, publish readiness, live readiness, and purchase availability share one backend contract.
- Risk: Revenue-sensitive product drift.
- Priority candidate: Candidate P1.
- Blocked by: Venue buyer backend/RLS/purchase review.
- Recommended next action: Link to VBF-GAP-001 through VBF-GAP-004.

### ELC-GAP-009

- Gap ID: ELC-GAP-009
- Domain: Check-in timing
- Current issue: Current scanner path has positive control evidence, but legacy/proof-related check-in functions and lifecycle coupling remain in review.
- Expected clean lifecycle contract: Check-in and proof mutation are live-only or explicitly lifecycle-scoped by backend authority.
- Risk: Security-sensitive.
- Priority candidate: Candidate P1 / Unknown.
- Blocked by: Legacy/proof reachability and body review.
- Recommended next action: Keep proof hardening plan blocked until explicit patch approval and reachability verification.

### ELC-GAP-010

- Gap ID: ELC-GAP-010
- Domain: Memory, media, voting, winners
- Current issue: Mobile has detailed lifecycle UI logic, and backend local evidence resolves winners on end/archive, but final backend authority and product semantics are incomplete.
- Expected clean lifecycle contract: Post-event memory, gallery, voting result, poll, winner, and cancelled-event behavior are accepted and backend-authoritative.
- Risk: Product correctness and privacy-sensitive media exposure.
- Priority candidate: Candidate P2.
- Blocked by: Media/voting/winner contract decision and RPC/RLS review.
- Recommended next action: Media / Voting / Memory Wall Contract Audit.

### ELC-GAP-011

- Gap ID: ELC-GAP-011
- Domain: Notifications/reminders
- Current issue: Publish RPC creates notification side effects locally, while reminders and Edge Function deployment are separately unclear.
- Expected clean lifecycle contract: Publish/start/end/cancel/archive notification and reminder behavior is documented separately from Edge Function deployment.
- Risk: Privacy-sensitive / product correctness.
- Priority candidate: Candidate P2 / Unknown.
- Blocked by: Notification DB function review and Edge Function deployment confirmation.
- Recommended next action: Notification / Push / Reminder Contract Audit.

### ELC-GAP-012

- Gap ID: ELC-GAP-012
- Domain: Direct event mutation
- Current issue: Event direct reads/writes exist in frontend/admin surfaces, while lifecycle authority is expected to be RPC-first.
- Expected clean lifecycle contract: Lifecycle-changing event fields are mutated only through accepted RPCs or documented RLS policies.
- Risk: Security-sensitive and product correctness.
- Priority candidate: Candidate P1 / Unknown.
- Blocked by: Direct data access inventory and production policy review.
- Recommended next action: Direct Data Access / RLS Reliance Audit.

## 24. Product Decisions Required

- What is the accepted lifecycle state machine and transition table?
- Is `event_state` the canonical lifecycle field, a shadow field, or a migration compatibility field?
- Which status aliases must remain supported: `active`, `closed`, `cancelled`, `canceled`, `deleted`, `archive`?
- What is the accepted cancellation path and product meaning?
- Does live revert remain an intended product feature?
- What blockers prevent unpublish or archive?
- Should ticket purchase remain available while live?
- Should reservation creation remain available while live?
- Should ended events ever appear in feeds as an ended-tail?
- Which post-event surfaces should appear for ended versus archived versus cancelled events?
- Which lifecycle transitions trigger notifications, reminders, winner finalization, and media/memory state changes?
- Which direct event field writes are accepted under RLS, and which must be RPC-only?

## 25. Recommended Next Audits

1. Direct Data Access / RLS Reliance Audit

   Focus on direct `events`, `event_modules`, tickets, reservations, media, venue layout, and session access paths that can affect lifecycle-sensitive behavior.

2. Public Web / Share Surface Audit

   Focus on public event share, claim handoff, profile/archive visibility, public proof verification, and lifecycle visibility rules for guest/anon contexts.

3. Notification / Push / Reminder Contract Audit

   Focus on lifecycle-triggered notifications, reminders, push-token ownership, publish/start/end/cancel/archive semantics, and separation between Database Functions / RPCs and Edge Function deployment.

## 26. Non-Goals

- This audit does not provide SQL, migrations, implementation code, patch instructions, cleanup instructions, or production commands.
- This audit does not authorize modifying any application, dashboard, mobile, web, backend, or Supabase code.
- This audit does not authorize changing lifecycle, ticketing, reservation, media, storage, RLS, or auth behavior.
- This audit does not connect to production or inspect production directly.
- This audit does not run Supabase CLI, builds, tests, installs, or deployment commands.
- This audit does not claim final exploitability or production vulnerability.
- This audit does not claim feature removal is safe.

## 27. Open Questions

- What is the live production definition of `transition_event_status_v2`?
- What is the live production definition of `publish_event_with_groups_and_snapshot_v2`?
- Does production currently have `event_state`, and is it always consistent with `status`?
- Are `control_cancel_event`, `control_end_event`, `publish_event`, `publish_event_with_groups`, `update_event_status`, or older lifecycle RPCs externally executable?
- What is the accepted lifecycle behavior for cancelled events?
- What is the accepted lifecycle behavior for deleted events?
- Is live revert supported, deprecated, or only a historical concept?
- What should happen to tickets, reservations, orders, claims, staff assignments, media, notifications, and public shares when an event is unpublished, cancelled, ended, or archived?
- Which frontend root is authoritative for public event share behavior?
- Do all feed RPCs and event policies enforce the same lifecycle and visibility contract?
- Are ended/archived profile archive and memory wall surfaces product-critical or optional?

## 28. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds, tests, or installs were run.
- No files were staged or committed.
- Only `07_Audits/EventLifecycleContractAudit.md` was created/modified.

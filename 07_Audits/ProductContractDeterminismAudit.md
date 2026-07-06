# Product Contract Determinism Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false

## 2. Purpose

This audit maps JoinFolk's observed product surfaces into a deterministic product contract. It separates product intent from implementation reality, frontend behavior from backend authority, and production evidence from local source assumptions.

This is a product/system audit only. It is not a patch plan, cleanup plan, migration plan, accepted vulnerability list, or authorization to change code, remove features, add features, modify backend/RPC/RLS/storage/auth, or modify any Supabase tree.

## 3. Audit Scope

Read-only sources used:

- Handbook repository: `C:\dev\joinfolk-engineering-handbook`
- Platform/backend candidate repository: `C:\dev\hostos`
- Web/dashboard repository: `C:\dev\joinfolk-web`
- Mobile repository: `C:\dev\hostos\apps\mobile`

Primary context preserved from existing handbook reports:

- Future accepted Supabase migration working target: `C:\dev\hostos\supabase\migrations`.
- This is not proof that `C:\dev\hostos\supabase` is the historical sole canonical source.
- Split-source migration history remains unresolved.
- Production SQL/RPC evidence remains stronger than local source assumptions.
- Database Functions / RPC evidence is separate from Edge Function deployment evidence.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- No backend patch or migration is authorized by this audit.

## 4. Product Promise Summary

JoinFolk appears to be a mobile-first event and community platform for discovering, creating, sharing, attending, monetizing, staffing, checking in, and remembering social events. The platform spans:

- Mobile attendee, host, and social workflows.
- Dashboard host, venue, staff, and ops workflows.
- Public web/share/deep-link surfaces.
- Supabase-backed database functions, RLS, storage, and auth authority.
- Venue layout, ticketing, reservation, gallery, notification, and social graph subsystems.

The clean product contract should make the backend authority explicit for business-critical rules while keeping frontend surfaces responsible for presentation, navigation, previews, and user guidance.

## 5. Core Product Domains

Core domains observed:

- Event discovery, feeds, and public sharing.
- Event creation, draft management, publish readiness, and lifecycle transitions.
- Host persona, personal persona, and tier-based access.
- Tickets, commerce orders, claims, transfers, reservations, and ownership state.
- Venue tooling, layout editing, seat/standing buyer flows, and venue reservations.
- Check-in, staff scanner, check-in proof, and public proof verification.
- Media/gallery/memory wall, voting, polls, winners, and moderation.
- Notifications, push tokens, notification settings, and reminders.
- Social graph: friends, following, host followers, groups, and direct messages.
- Dashboard ops/admin tools, host identity transfer, tier controls, and media ops.
- Storage/media ownership and public/private exposure semantics.
- Settings, localization, account, profile, auth, and safety/account controls.

## 6. Product Contract Matrix

| Domain | Feature / flow | Current observed surface | Expected product contract | Product status | Determinism status | Authority owner | Evidence source | Recommendation |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Event discovery / feeds | Home, discover, explore, feed visibility | Mobile routes and feed helpers; public web event pages; multiple discover/feed RPCs in backend reports | A single viewer-role-aware discovery contract controls which events appear to each viewer | Core MVP | Mostly deterministic | Mixed | Mobile routes/libs; backend gap report | Document canonical contract |
| Event creation / publish | Create event, draft save, publish readiness | Mobile and web create routes; dashboard create/detail pages; publish RPCs and readiness guards | Draft/publish rules are backend-authoritative, with UI as preview/guidance only | Core MVP | Mostly deterministic | Backend/RPC/RLS/storage/auth | Dashboard/mobile routes; production/audit reports | Preserve and document canonical contract |
| Event lifecycle | Draft, published, live, ended, archived, reverted | Mobile lifecycle helper includes aliases; dashboard normalizes statuses differently; revert/transition RPCs exist | Canonical lifecycle states and allowed transitions are defined once and enforced by backend | Product-critical | Split-source / duplicated | Mixed | Mobile lifecycle helper; dashboard types; RPC inventory | Reconcile |
| Host/personal persona | Personal vs host actions and profile context | Mobile host routes; host followers/members/transfer; dashboard host tools | Viewer role, host authority, and persona switching must be explicit and backend-authoritative for privileged actions | Product-critical | Backend authority unclear | Mixed | Mobile routes/libs; dashboard pages; audit reports | Needs decision |
| Tier system | user, semi_pro, pro | Mobile and dashboard tier parsers; tier controls; handbook product docs | Tier grants and feature gates should be backend-authoritative, with UI mirroring only | Product-critical | Mostly deterministic | Mixed | Mobile capabilities; dashboard types; ops tier page | Document canonical contract |
| Tickets / commerce / orders | Ticket products, orders, purchase, wallet | Multiple ticket/order RPCs; mobile and dashboard ticket flows; production RPC evidence | Commerce acquisition, ownership, order state, and capacity are backend-authoritative | Product-critical | Split-source / duplicated | Backend/RPC/RLS/storage/auth | Mobile/dashboard RPC scans; production reports | Reconcile and harden later |
| Reservations | Event and venue reservations | Mobile and dashboard reservation flows; reservation RPCs; RLS enabled in production | Reservation capacity, ownership, conflicts, and status transitions are backend-authoritative | Product-critical | Mostly deterministic | Backend/RPC/RLS/storage/auth | Mobile/dashboard routes; production reports | Preserve and review |
| Gift ticket / claims / transfers | Gift claims, pending claims, host transfer | Mobile claim/transfer routes; production host transfer RPCs; split provenance | Gift/claim/transfer authority, recipient state, and pending ownership must be backend-authoritative | Product-critical | Split-source / duplicated | Backend/RPC/RLS/storage/auth | Mobile routes; production/provenance reports | Reconcile |
| Venue tooling | Venue create/edit/offering/media/reservations | Dashboard venue pages; mobile venue host panel/edit; hostos dashboard venue routes | Venue ownership, offering mutation, reservation decisions, and media mutation are backend-authoritative | Product-critical | Mostly deterministic | Mixed | Dashboard/mobile route inventory; backend reports | Document canonical contract |
| Venue layout / buyer flow | Seat picker, standing areas, product sections | Dashboard visual editor; mobile seat/area/session picker; shared venue package; split migration provenance | Layout geometry, seat/standing availability, and buyer product mapping should have one canonical contract | Product-critical | Split-source / duplicated | Mixed | Dashboard venue editor; mobile routes; provenance report | Reconcile |
| Check-in / staff scanner | Scanner, ticket lookup, staff assignment, unsafe paths | Dashboard staff scanner; mobile scan route; positive control on `checkin_ticket_by_id_v2`; legacy/proof RPC concerns | Check-in mutation requires host/staff authority, event/ticket scoping, status checks, and backend state machine | Product-critical | Mostly deterministic | Backend/RPC/RLS/storage/auth | Focused backend follow-up; dashboard/mobile routes | Preserve current scanner path and harden later |
| Check-in proof | Proof generation/readback/removal/public verification | Proof-related RPCs; public proof route in hostos dashboard; public verify page deferred in web | Proof mutation and verification semantics must be explicit, bounded, and backend-authoritative | Product-critical | Backend authority unclear | Backend/RPC/RLS/storage/auth | Focused follow-up; patch-plan draft; public route inventory | Needs decision |
| Media / gallery / memory wall | Event media, comments, moderation, public highlights | Mobile gallery/photos/share-photo; dashboard gallery/moderation; public storage policies | Media visibility, ownership, moderation, and public exposure require documented backend/storage authority | Optional | Split-source / duplicated | Mixed | Mobile/dashboard RPC scans; storage reports | Reconcile |
| Voting / polls / winners | Poll edit, votes, winners, public highlights | Mobile poll/edit and winner RPCs; dashboard poll page | Voting eligibility, one-photo rule, winner finalization, and poll state must be backend-authoritative | Optional | Mostly deterministic | Backend/RPC/RLS/storage/auth | Mobile/dashboard RPC scans | Document canonical contract |
| Notifications / push / reminders | Notifications, push tokens, settings, reminders | Mobile notification/reminder routes; DB functions; local Edge functions not deployed visibly | Notification data and token ownership are backend-authoritative; Edge dispatch deployment status must be explicit | Product-critical | Split-source / duplicated | Mixed | Production reports; mobile route/RPC scan | Reconcile |
| Social graph | Friends, following, host followers, groups | Mobile social routes; web/mobile group routes; social RPCs and direct table reads | Relationship state and group membership should be backend-authoritative; UI should not define visibility authority | Optional | Backend authority unclear | Mixed | Mobile/web route scans; RPC scans | Document canonical contract |
| Direct messages | Conversations, messages, unread count | Mobile and dashboard DM RPCs; earlier hostos backend report did not confirm messaging in allowed files | DM participation, read state, deletion/archive, and visibility must be backend-authoritative | Optional | Split-source / duplicated | Backend/RPC/RLS/storage/auth | Mobile/dashboard RPC scans; backend gap report | Needs verification |
| Dashboard ops/admin | Transfer approval, tier control, user lookup, media ops | Dashboard ops pages; production admin transfer RPC exists; broad grants visible with internal gate | Ops authority model must be explicit, least-privilege, and backend-gated | Product-critical | Backend authority unclear | Mixed | Dashboard pages; production reports | Harden later |
| Storage/media ownership | Avatars, venue-media, venue-posters, event media | Public buckets confirmed; upload/update/delete appears constrained; ADR missing | Public/private storage semantics and ownership paths should be accepted by product/security decision | Product-critical | Mostly deterministic | Backend/RPC/RLS/storage/auth | Production parity report | Needs decision |
| Public web / share | Landing, event share, claim handoff, public verify | Public web routes; direct event reads; claim handoff; verification deferred | Public share and verification surfaces need a canonical public contract and backend authority | Core MVP | UI-dependent | Mixed | `joinfolk-web\web`; hostos public routes | Feature-complete later |
| Settings / localization / account | Auth, profile, privacy, terms, settings, localization | Mobile auth/settings/locales; hostos web localized public pages | Account settings and localization are UI-owned except auth/profile/privacy authority | Core MVP | Mostly deterministic | Mixed | Mobile routes; hostos web routes | Preserve |

## 7. Determinism Assessment

Deterministic or mostly deterministic areas:

- The product domains are consistently visible across handbook docs, mobile routes, dashboard pages, and backend/RPC audit reports.
- Sensitive production tables previously targeted by backend audits have production RLS enabled according to supplied production SQL evidence.
- Current scanner path `checkin_ticket_by_id_v2` has positive control evidence and constrained anon/public execute grants in production evidence.
- `C:\dev\hostos\supabase\migrations` has been accepted as the future working migration target, with explicit caveats.

Split, duplicated, or unclear areas:

- Commerce mode is represented by multiple client-side contracts. Mobile uses `none`, `reservation_only`, `ticket_sales_only`, and `illegal_dual_mode`; dashboard uses `none`, `reservation_only`, `ticket_sales_only`, and `conflict`; handbook/module language also refers to `ticketSales`, `reservation`, and `conflict`.
- Event lifecycle/status normalization is duplicated. Mobile and dashboard both normalize aliases such as `active`, `live`, `closed`, `ended`, `cancelled`, and `archived`, but the canonical status contract is not centralized in product documentation.
- Multiple frontend roots expose overlapping event, ticket, scan, claim, and dashboard surfaces. Observed roots include mobile Expo routes, `joinfolk-web\app`, `joinfolk-web\web`, Vite dashboard, hostos Next web, and hostos Next dashboard.
- Ticketing, reservations, entitlement, and gift claim logic has many RPC versions and overloads. Production evidence is stronger than local source assumptions, but active contract ownership still needs focused review.
- Venue layout and buyer flow logic is spread across dashboard geometry/editor code, mobile buyer routes, hostos shared packages, and split migration provenance.
- Direct messages exist in mobile/dashboard RPC usage and mobile migration provenance, while earlier backend gap reporting did not find a clear messaging implementation in the initially allowed backend tree.
- Local Edge Function folders exist, but no deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.

## 8. Authority Boundary Assessment

Backend/RPC/RLS/storage/auth must be the authority for:

- Event visibility and viewer roles.
- Event lifecycle transitions and publish readiness.
- Host ownership, staff assignment, and ops/admin authority.
- Tier eligibility and paid-feature gates.
- Ticket, reservation, order, gift claim, transfer, and wallet ownership state.
- Capacity, seat/standing availability, and commerce conflict prevention.
- Check-in mutation, proof mutation, proof verification, and scanner state machines.
- Media ownership, moderation, public/private storage exposure, and upload/delete rights.
- Notification token ownership and reminder ownership.
- Social graph, group membership, DM participation, and private message visibility.

Frontend/mobile/dashboard/web may own:

- Presentation, navigation, copy, local layout, preview state, and disabled states.
- Client-side convenience validation that mirrors backend authority.
- Buyer/editor visualization, provided persisted geometry and availability are backend-valid.
- Localization, non-security user preference UI, and account setup flow presentation.

Frontend behavior must not be treated as security authority unless backed by production RLS/RPC/storage/auth evidence.

## 9. Feature Ownership Map

| Surface | Observed ownership | Product-contract role | Determinism note |
| --- | --- | --- | --- |
| Mobile app | Attendee, host, ticket, reservation, scanner, gallery, social, notification, settings, and venue buyer flows | Primary consumer and mobile host UX | Broadest end-user surface; contains some frontend product-rule helpers |
| Vite dashboard | Host, venue, products, reservations, staff, modules, gallery, ops/admin, visual venue editor | Primary operational and host-management UX | Strong feature surface; some product rules are implemented as dashboard helpers |
| Public web | Landing, public event share, claim handoff, public verification placeholder | Public acquisition/share/deep-link UX | Some surfaces appear incomplete or UI-dependent |
| Hostos Next web/dashboard | Public localized web and overlapping dashboard/event/proof routes | Possible active or parallel surface | Needs source-of-truth classification before product contract reliance |
| Supabase Database Functions / RPCs | Lifecycle, tickets, reservations, commerce, media, notification, venue, check-in, ops/admin | Security-sensitive authority layer | Production evidence outranks local source assumptions |
| Supabase storage/RLS/auth | Table and object visibility, ownership, mutation authority | Security-sensitive authority layer | Public media exposure needs ADR/security decision |
| Supabase Edge Functions | Local folders observed; none visible as deployed in Dashboard | Future/non-current deployment concern unless proven otherwise | Database Functions / RPC evidence is separate from Edge Function deployment evidence |

## 10. Core MVP vs Optional vs Experimental

Core MVP:

- Event discovery/feed.
- Event creation, draft, publish, and event detail.
- Public event share and claim/deep-link handoff.
- Auth, profile, settings, privacy/terms, and localization.

Product-critical:

- Event lifecycle transitions and publish readiness.
- Host/personal persona and tier gating.
- Tickets, commerce orders, reservations, gift claims, transfers, and wallet ownership.
- Venue tooling where ticket/reservation buyer flow depends on venue state.
- Check-in, staff scanner, and proof semantics.
- Storage/media ownership and public/private exposure.
- Notifications and push-token ownership.
- Ops/admin authority for transfer, tier, user, and media operations.

Optional:

- Media/gallery/memory wall presentation.
- Voting, polls, and winners.
- Friends, following, host followers, groups, and direct messages.
- Personal reminders, unless product decision elevates reminders to product-critical.

Experimental or Needs verification:

- Venue visual editor and advanced buyer topology if not yet accepted as the canonical buyer model.
- Public proof verification pages and proof readback/removal behavior.
- Local Edge Functions that are not visible as deployed in Supabase Dashboard.
- Host identity transfer source/provenance despite production RPC existence.
- Duplicate web/app/dashboard roots until active surfaces are classified.

Legacy / deprecated candidates:

- Duplicate `joinfolk-web\app` surfaces that appear to overlap mobile routes and use older RPC names.
- Older ticket/check-in/checklist RPC names where current v2/v5 paths exist.
- Backup or alternate dashboard/page files if confirmed unused.
- Any component-specific Supabase history not used for future accepted migration work.

No feature removal is safe to assume from this audit alone.

## 11. Duplicated / Split / Unclear Surfaces

- Multiple Supabase migration histories remain unresolved: hostos, joinfolk-web, and mobile.
- Multiple frontend roots expose overlapping product flows: mobile Expo app, `joinfolk-web\app`, `joinfolk-web\web`, Vite dashboard, hostos Next web, and hostos Next dashboard.
- Commerce mode, module mutual exclusion, and buyer setup readiness appear in frontend helpers and backend migration/RPC evidence, but the canonical product contract is not documented in one place.
- Event lifecycle statuses and aliases are normalized in more than one frontend surface.
- Ticketing and check-in have current and legacy RPC names in active-looking UI code paths.
- Venue layout/buyer state is split across dashboard visual editor, mobile buyer routes, shared packages, and migration provenance.
- Messaging exists in mobile/dashboard usage, but source provenance and production authority need verification.
- Public proof verification is visible as a public surface but appears incomplete or deferred in at least one web path.

## 12. Backend-Critical Product Rules

These rules require backend/RPC/RLS/storage/auth authority:

- Who can see an event in feeds, public share, group share, ended/archived history, and private contexts.
- Who can create, edit, publish, transition, revert, archive, cancel, or delete an event.
- Whether a host can enable ticket sales, reservations, modules, or venue-linked buyer flows.
- Whether an event can have ticket sales and reservations at the same time.
- Who can buy, request, approve, transfer, claim, gift, revoke, or view a ticket.
- Whether a user already has a reservation, active ticket, pending gift claim, or conflicting entitlement.
- Who can create, decide, cancel, or view reservations.
- Who can mutate check-in state or proof state.
- Who can assign staff or perform scanner actions.
- Who can upload, view, update, hide, moderate, or delete media.
- Whether storage buckets and object paths are intended-public or private.
- Who can send/read/archive/delete direct messages.
- Who can execute ops/admin transfer, tier, media, and user lookup functions.
- Who can register or delete push tokens and reminders.

## 13. Frontend-Dependent Product Rules

Observed frontend-dependent or frontend-heavy rules that need canonical backend mapping:

- Mobile feed visibility helper behavior for event visibility and viewer context.
- Mobile check-in state derivation from event status and timing.
- Dashboard event workstation/action/readiness resolver.
- Dashboard commerce mode and ticket setup state derivation.
- Dashboard module mutual exclusion prompts and tier-specific module visibility.
- Dashboard venue visual editor validation and buyer preview state.
- Public event share rendering based on direct event reads.
- Public verification placeholder/deferred logic.
- Client route availability for host, staff, semi-pro, pro, and ops flows.

These may be useful UI mirrors, but they should not be treated as the authoritative product contract without backend evidence.

## 14. Missing / Incomplete Feature Candidates

- Canonical lifecycle and status alias contract.
- Canonical commerce mode/module mutual exclusion contract.
- Accepted public media/storage ADR.
- Public event share and public verification contract.
- Direct messaging backend authority and source-path provenance.
- Venue buyer/layout contract across dashboard, mobile, shared packages, and backend.
- Check-in proof mutation/readback/removal contract.
- Ops/admin authority model and grant posture contract.
- Source classification for duplicate web/dashboard/app surfaces.
- Canonical module key registry, including participant/camera/gallery/ticket/reservation semantics.

## 15. Candidate Cleanup / Deprecated Surface Register

Candidate cleanup surfaces, pending separate verification and explicit approval:

- Duplicate web/mobile-like routes under `joinfolk-web\app` if superseded by mobile or another active web app.
- Older RPC usage such as legacy check-in, ticket, and checklist functions where current RPC paths are accepted.
- Backup or alternate dashboard page files if confirmed unused.
- Scratch/temp migration files in non-target Supabase trees if later classified as non-source.
- Local Edge Function folders that are not deployed and are not part of accepted future deployment.

This register does not authorize cleanup, deletion, migration, or feature removal.

## 16. Product Decisions Required

- What is the accepted canonical event lifecycle state machine, including aliases and reversible transitions?
- What is the accepted canonical commerce mode vocabulary and mutual-exclusion behavior?
- Which frontend root is active for public web, dashboard, and mobile-like web surfaces?
- Which product flows are MVP core versus optional or experimental?
- Should public media buckets remain public by accepted product/security decision?
- What is the accepted public share and public proof verification contract?
- What is the accepted owner/staff/ticket-owner authority model for check-in proof?
- Which messaging surfaces are part of the accepted product?
- Which venue visual editor and buyer flow is canonical?
- Which legacy RPCs must remain externally callable for compatibility?
- Which app surface owns settings, localization, and account flows for web versus mobile?

## 17. Gap Register Seeds

### PCD-GAP-001

- Domain: Commerce / module contract
- Current issue: Commerce mode vocabulary and mutual-exclusion behavior differ across mobile, dashboard, and handbook language.
- Expected clean contract: One accepted commerce mode contract and backend-authoritative mutual-exclusion rule, mirrored by UI.
- Risk: Product logic drift between buyer, host, and backend flows.
- Priority candidate: P1
- Blocked by: Canonical commerce/module decision and backend parity review.
- Recommended next action: Commerce + Ticketing Contract Audit.

### PCD-GAP-002

- Domain: Event lifecycle
- Current issue: Status aliases and transitions are normalized in multiple frontend helpers.
- Expected clean contract: One documented lifecycle state machine with backend-authoritative transitions.
- Risk: Hosts, attendees, feeds, scanner, and dashboard actions can disagree on event state.
- Priority candidate: P1
- Blocked by: Lifecycle/status contract decision.
- Recommended next action: Authority Boundary / ViewerRole Determinism Audit.

### PCD-GAP-003

- Domain: Repository/product surface ownership
- Current issue: Multiple frontend roots expose overlapping event, ticket, claim, scan, and dashboard surfaces.
- Expected clean contract: Active, legacy, and experimental product surfaces are explicitly classified.
- Risk: Audit, QA, and patch work may target a non-active surface.
- Priority candidate: P1
- Blocked by: Manual source-of-truth classification for web/dashboard/mobile surfaces.
- Recommended next action: Product Surface Source-of-Truth Audit.

### PCD-GAP-004

- Domain: Authority boundary
- Current issue: Frontend direct table/storage access exists for events, tickets, staff assignments, venue layouts, media, and proof-related data.
- Expected clean contract: Every direct access path has documented RLS/storage authority or is replaced by accepted RPC authority.
- Risk: Product rules may be inferred from UI rather than backend authority.
- Priority candidate: P1
- Blocked by: RLS/policy and frontend callsite mapping.
- Recommended next action: Authority Boundary / ViewerRole Determinism Audit.

### PCD-GAP-005

- Domain: Tickets / reservations / entitlement
- Current issue: Many RPC versions and overloads exist for ticketing, purchase, reservation, gift, and entitlement paths.
- Expected clean contract: Active acquisition and ownership paths are named, versioned, and mapped to entitlement guards.
- Risk: Legacy or alternate paths may bypass expected product constraints.
- Priority candidate: P1
- Blocked by: Focused RPC body/overload review.
- Recommended next action: Commerce + Ticketing Contract Audit.

### PCD-GAP-006

- Domain: Venue layout / buyer flow
- Current issue: Venue geometry, product sections, seat/standing choices, and buyer previews are split across dashboard, mobile, shared packages, and migration provenance.
- Expected clean contract: One venue buyer contract maps layout, products, availability, and purchase/reservation authority.
- Risk: Buyer UI, host editor, and backend availability can diverge.
- Priority candidate: P1
- Blocked by: Canonical venue buyer model decision.
- Recommended next action: Venue Buyer Flow Contract Audit.

### PCD-GAP-007

- Domain: Check-in proof
- Current issue: Proof-related RPCs and public verification surfaces are not yet represented as a clean accepted product contract.
- Expected clean contract: Proof mutation, readback, removal, and public verification semantics are explicit and backend-authoritative.
- Risk: Check-in proof behavior may be overexposed, incomplete, or inconsistent across surfaces.
- Priority candidate: Unknown
- Blocked by: Reachability, active-use, and authority review.
- Recommended next action: Continue from the proof check-in RPC hardening draft only after explicit patch scope approval.

### PCD-GAP-008

- Domain: Storage / public media
- Current issue: Public buckets and public read policies are confirmed, but accepted product/security semantics are not documented.
- Expected clean contract: ADR defines which media is intentionally public and which mutations require owner/host/staff authority.
- Risk: Privacy expectations may diverge from storage exposure.
- Priority candidate: P2 / Unknown
- Blocked by: Product/security ADR decision.
- Recommended next action: Public Media ADR Audit.

### PCD-GAP-009

- Domain: Direct messages
- Current issue: DM RPC usage appears in mobile and dashboard, while earlier backend source scan did not confirm messaging in the primary backend tree.
- Expected clean contract: DM participation, visibility, deletion, archive, and unread state are backend-authoritative and source-owned.
- Risk: Messaging may be split-source or insufficiently documented.
- Priority candidate: P2 / Unknown
- Blocked by: Production RPC and source provenance verification.
- Recommended next action: Messaging Contract Verification.

### PCD-GAP-010

- Domain: Ops/admin / host transfer
- Current issue: Host identity transfer exists in production, but provenance is split and grant/authority model needs review.
- Expected clean contract: Ops/admin functions are explicitly gated, auditable, and source-owned.
- Risk: Administrative product behavior may depend on internal gates without a documented authority model.
- Priority candidate: P1 / Unknown
- Blocked by: Ops/admin authority review and source-path confirmation for future work.
- Recommended next action: Ops/Admin Authority Contract Audit.

### PCD-GAP-011

- Domain: Public web / share / verification
- Current issue: Public event share and claim handoff exist; public verification appears deferred or incomplete in at least one surface.
- Expected clean contract: Public read, claim, and verification flows are explicitly product-owned and backend-authoritative where needed.
- Risk: Public UX can imply guarantees that backend/product contract has not accepted.
- Priority candidate: P2
- Blocked by: Public surface classification and share/proof decision.
- Recommended next action: Public Web Contract Audit.

### PCD-GAP-012

- Domain: Notifications / push / reminders
- Current issue: DB notification functions, mobile reminder flows, and local Edge Function folders exist, but Edge Functions are not visible as deployed.
- Expected clean contract: Notification data, push dispatch, reminder ownership, and deployment state are separately documented.
- Risk: Push behavior may be assumed from local code that is not production-active.
- Priority candidate: P2 / Unknown
- Blocked by: Deployment/source confirmation and notification contract review.
- Recommended next action: Notification and Push Contract Audit.

## 18. Recommended Next Audits

1. Authority Boundary / ViewerRole Determinism Audit

   Map frontend direct data access, viewer-role helpers, event visibility, RLS/policy evidence, and RPC authority into one viewer-role contract.

2. Commerce + Ticketing Contract Audit

   Focus on commerce mode, ticket products, orders, reservations, gift claims, transfers, entitlement guards, RPC versions, and active buyer paths.

3. Venue Buyer Flow Contract Audit

   Map venue layout editor, seat/standing-area buyer flows, product sections, availability, dashboard preview, mobile purchase UI, and backend authority.

## 19. Non-Goals

- This audit does not modify application, dashboard, mobile, web, backend, or Supabase code.
- This audit does not create SQL or migrations.
- This audit does not connect to production or inspect production state.
- This audit does not run Supabase CLI, builds, tests, installs, or deployment commands.
- This audit does not authorize cleanup, deletion, source normalization, patching, feature removal, or feature addition.
- This audit does not mark any handbook document canonical.
- This audit does not claim production vulnerability or final exploitability.

## 20. Open Questions

- Which frontend roots are active, legacy, experimental, or public-only?
- What is the accepted core MVP feature set for the next release boundary?
- What is the canonical lifecycle state machine?
- What is the canonical commerce mode and module mutual-exclusion vocabulary?
- Which venue buyer model is accepted as canonical?
- Which direct table access paths are accepted because production RLS/policies are authoritative?
- Which legacy RPC names must remain callable for backward compatibility?
- What is the accepted public media exposure policy?
- Is direct messaging part of MVP, optional product, or experimental scope?
- What is the accepted public proof verification experience?
- Which local Edge Functions are future deployment candidates, if any?
- Which product/security decisions must be captured as ADRs before backend patch planning?

## 21. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds, tests, or installs were run.
- No files were staged or committed.
- Only `07_Audits/ProductContractDeterminismAudit.md` was created/modified for this audit.

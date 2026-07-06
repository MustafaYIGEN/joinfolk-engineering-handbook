# Authority Boundary / ViewerRole Determinism Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false

## 2. Purpose

This audit maps JoinFolk's observed viewer-role, persona, tier, staff, ops, ownership, and authority boundaries.

It separates observed current behavior from the desired authority contract. It also separates frontend UI gating from backend authority, viewer-role derivation from permission enforcement, and product role names from implementation-level checks.

This is an authority-boundary/system audit only. It is not a patch plan, cleanup plan, migration plan, implementation plan, accepted vulnerability list, or authorization to modify backend/RPC/RLS/storage/auth.

## 3. Audit Scope

Read-only sources used:

- Handbook repository: `C:\dev\joinfolk-engineering-handbook`
- Platform/backend candidate repository: `C:\dev\hostos`
- Web/dashboard repository: `C:\dev\joinfolk-web`
- Mobile repository: `C:\dev\hostos\apps\mobile`

Primary handbook context preserved:

- Future accepted Supabase migration working target: `C:\dev\hostos\supabase\migrations`.
- This is not proof that `C:\dev\hostos\supabase` is the historical sole canonical source.
- Split-source migration history remains unresolved.
- Production SQL/RPC evidence remains stronger than local source assumptions.
- Database Functions / RPC evidence is separate from Edge Function deployment evidence.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- No backend patch or migration is authorized by this audit.

## 4. Authority Contract Summary

JoinFolk appears to use a layered authority model:

- Product-level viewer roles define what a person is in relation to an event, venue, social graph, ticket, reservation, media item, or ops surface.
- Frontend helpers derive display roles, render models, module visibility, and route availability.
- Backend/RPC/RLS/storage/auth must enforce security-sensitive, privacy-sensitive, revenue-sensitive, and ops/admin-sensitive permissions.
- UI gates are acceptable as mirrors and affordances only when the backend authority path is explicit.

The desired clean contract is:

- Frontend derives viewer context for UX.
- Backend validates every privileged action.
- Direct table/storage reads rely on documented RLS/storage policies.
- Product role names are documented separately from implementation checks such as `auth.uid()`, `host_id`, `staff_user_id`, `tier`, `is_ops`, ownership columns, and storage path ownership.

## 5. Viewer Role Vocabulary

Observed or expected viewer/product-role concepts:

| Role / concept | Product-level meaning | Observed implementation signal | Authority note |
| --- | --- | --- | --- |
| `guest` | Not authenticated public viewer | Mobile `ViewerRole` union and module access resolver | UI role; backend public/anon policy must enforce public access |
| `authenticated_non_participant` | Authenticated viewer without event relationship | Mobile uses `authenticated` role | Product role is implicit; should be named in contract |
| `participant` | Accepted participant or equivalent event relationship | Mobile access context and event participant reads | Backend/RLS/RPC must define participant truth |
| `ticket_holder` | Viewer owns or holds a ticket | Mobile access context and ticket RPC/direct reads | Revenue-sensitive; backend authority required |
| `checked_in` | Viewer has checked-in status/proof | Mobile access context, ticket statuses, proof truth RPC | Backend authority required |
| `host` / event owner | Event host or event owner | `host_id === uid`, owner guards, host RPCs | Backend authority required |
| `staff` | Assigned event staff | `event_staff_assignments`, staff RPCs | Backend authority required |
| `scanner` / `manager` | Staff subroles | Mobile staff role type and dashboard staff scanner routes | Backend authority required for scanner actions |
| `ops/admin` | Operational administrator | Dashboard `isOps`; production admin RPC has `auth_is_ops()` gate | Ops/admin-sensitive; backend authority required |
| personal persona | Social participant identity | Mobile persona context | UI identity mode; backend must validate any privilege |
| host persona | Organizer identity | Mobile persona context and dashboard host tier | UI identity mode; backend must validate host authority |
| `user`, `semi_pro`, `pro` | Account tier | Mobile/dashboard tier parsers and profile reads | Backend authority required for paid/host capabilities |
| venue owner | Venue host/owner | Venue RPCs and host ownership policy evidence | Backend authority required |
| ticket/reservation/media owner | Resource owner | RPCs, direct table reads, storage object paths | Backend/RLS/storage required |
| group member / group owner | Share group access role | `share_groups` and `share_group_members` direct access | Backend/RLS required |
| friend/follower/host follower | Social graph relation | Social/friend/follow RPCs and direct reads | Backend/RLS/RPC required |
| blocked/muted relation | Safety relation | Block RPCs observed; muted relation not confirmed | Unknown / Needs verification |

## 6. Authority Boundary Matrix

| Domain | Action / permission | Current observed gate | Expected authority owner | Viewer roles involved | Determinism status | Risk class | Evidence source | Recommendation |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Event visibility / feeds | See event in feed/public share | Mobile feed helper, public direct event reads, feed/share RPCs | Backend/RPC/RLS/storage/auth | guest, authenticated, host, group member, participant | Split / duplicated | Privacy-sensitive | Mobile lifecycle helper; public web; production policy report | Document canonical authority contract |
| Event creation/edit/publish | Create, edit, publish event | Mobile/dashboard routes; publish RPCs; dashboard owner guard | Backend/RPC/RLS/storage/auth | host persona, event owner, tier | Mostly deterministic | Security-sensitive | Dashboard guards; mobile publish; backend reports | Preserve and reconcile |
| Lifecycle transitions | Publish, go live, end, archive, revert | Dashboard/mobile lifecycle helpers and transition RPCs | Backend/RPC/RLS/storage/auth | event owner, ops/admin | Split / duplicated | Product correctness | Mobile lifecycle; dashboard types/API | Reconcile |
| Event modules | Enable modules and mutual exclusion | Dashboard module UI plus module RPCs; mobile access engine | Backend/RPC/RLS/storage/auth | host, tier, participant, ticket_holder | Split / duplicated | Product correctness / revenue-sensitive | Dashboard modules; mobile moduleAccess; backend reports | Reconcile |
| Host/personal persona | Persona switching and attribution | Mobile persona context, AsyncStorage, tier fetch | Backend for privileged actions; UI for active mode | personal persona, host persona, tier | UI-derived only | Product correctness | Mobile persona/capabilities | Document canonical authority contract |
| Tier gates | Dashboard, venue, host features | Dashboard guards, mobile parsers, profile reads | Backend/RPC/RLS/storage/auth | user, semi_pro, pro | Mixed | Revenue-sensitive | Dashboard guards; mobile capabilities | Reconcile |
| Tickets / wallet | Buy, request, approve, view, check ownership | Ticket RPCs and some direct ticket reads | Backend/RPC/RLS/storage/auth | ticket_holder, host, participant, authenticated | Split / duplicated | Revenue-sensitive | Mobile/dashboard RPC scans; focused backend report | Harden later |
| Reservations | Create, decide, cancel, view | Reservation RPCs; dashboard/mobile routes | Backend/RPC/RLS/storage/auth | reservation owner, host, venue owner | Mostly deterministic | Revenue-sensitive | Mobile/dashboard RPC scans; production RLS evidence | Preserve and review |
| Gift/claim/transfer | Claim, revoke, transfer, preview | Mobile claim RPCs; production host transfer RPCs | Backend/RPC/RLS/storage/auth | ticket owner, recipient, ops/admin | Split / duplicated | Security-sensitive / revenue-sensitive | Mobile RPC scan; provenance reports | Reconcile |
| Venue ownership | Create/edit/archive venue and offerings | Venue RPCs; SemiProGuard UI gate | Backend/RPC/RLS/storage/auth | venue owner, semi_pro | Mostly deterministic | Revenue-sensitive | Dashboard/mobile venue RPCs; guards | Document canonical authority contract |
| Venue layout / buyer flow | Mutate layout, seat/standing availability | Dashboard visual editor, mobile buyer flow, layout RPCs/direct reads | Backend/RPC/RLS/storage/auth | venue owner, event owner, buyer | Split / duplicated | Revenue-sensitive | Dashboard/mobile/hostos route scans | Reconcile |
| Check-in / staff scanner | Scan/check in ticket | Current scanner RPC; staff scanner routes; staff assignment reads | Backend/RPC/RLS/storage/auth | host, staff, scanner, manager | Mostly deterministic | Security-sensitive | Production scanner evidence; mobile staff wrapper | Preserve current positive controls |
| Check-in proof | Proof mutation/readback/public verification | Proof RPCs, proof page, public verify placeholder | Backend/RPC/RLS/storage/auth | host, staff, ticket owner, public verifier | Backend authority unclear | Security-sensitive | Focused follow-up; proof plan; public routes | Needs decision |
| Media/gallery | Upload/read/update/delete/moderate | Module access helper, media RPCs, storage/direct access | Backend/RPC/RLS/storage/auth | participant, checked_in, host, staff, media owner | Split / duplicated | Privacy-sensitive | Mobile moduleAccess; storage reports | Reconcile |
| Voting/polls/winners | Vote, close poll, finalize winner | Mobile module access and poll/winner RPCs | Backend/RPC/RLS/storage/auth | participant, checked_in, host, staff | Mostly deterministic | Product correctness | Mobile moduleAccess; RPC scans | Document canonical authority contract |
| Notifications/push/reminders | Register token, read notifications, reminders | Notification/reminder RPCs; local Edge functions not deployed visibly | Backend/RPC/RLS/storage/auth | authenticated, notification owner | Split / duplicated | Privacy-sensitive | Production reports; mobile RPC scan | Reconcile |
| Social graph | Friends, following, groups, host followers | Social RPCs and direct group/friend reads | Backend/RPC/RLS/storage/auth | friend, follower, group member, group owner | Backend authority unclear | Privacy-sensitive | Mobile/web direct access and RPC scans | Needs verification |
| Direct messages | Conversations, messages, read/unread | Mobile/dashboard DM RPCs | Backend/RPC/RLS/storage/auth | participant in conversation | Split / duplicated | Privacy-sensitive | Dashboard DM RPCs; mobile RPC inventory | Needs verification |
| Ops/admin | Transfer, tier, user lookup, media ops | Dashboard `OpsGuard`; production admin RPC with internal gate | Backend/RPC/RLS/storage/auth | ops/admin | Backend authority unclear | Operational/admin-sensitive | Dashboard guard; production report | Harden later |
| Storage buckets | Public read and owner/host writes | Public buckets and storage policies; storage client calls | Backend/RPC/RLS/storage/auth | public, authenticated owner, host | Mostly deterministic | Privacy-sensitive | Production parity report; storage usage scans | Needs decision |
| Public web/share | Event share, claim handoff, verification | Public direct reads and handoff pages | Backend/RPC/RLS/storage/auth | guest, claimant, public verifier | UI-derived only | Privacy-sensitive | Public web scans | Feature-complete later |
| Settings/account/privacy | Profile visibility, tier/profile settings | Mobile settings/profile RPCs and direct profile reads | Backend/RPC/RLS/storage/auth | authenticated, profile owner, friend | Mostly deterministic | Privacy-sensitive | Mobile settings/profile scans | Preserve and document |

## 7. ViewerRole Determinism Assessment

Deterministic or mostly deterministic:

- Mobile has a named `ViewerRole` union: `guest`, `authenticated`, `ticket_holder`, `participant`, `checked_in`, `host`, and `staff`.
- Mobile `resolveViewerRole()` derives a highest-priority role from raw booleans with priority `host > staff > checked_in > participant > ticket_holder > authenticated > guest`.
- Mobile module access separates `canView` and `canInteract`.
- Dashboard has explicit route guards for auth, host-tier access, semi-pro venue access, pro/semi-pro event access, event ownership, ops, and staff scanner layout.
- Production evidence confirms RLS enabled on target sensitive tables and positive grant posture for `checkin_ticket_by_id_v2`.

Split, duplicated, or unclear:

- Product concept `authenticated_non_participant` is not a first-class mobile `ViewerRole`; it is represented as `authenticated`.
- Staff is a high-priority operational role in the mobile access resolver, while comments also note staff should not inherit social privileges by rank. This is a useful design signal but needs backend parity.
- `scanner` and `manager` exist as staff subroles, but the product contract for scanner versus manager authority is not fully documented.
- Dashboard guards derive tier and ops from profile context, while production authority must still be enforced by RPC/RLS/internal gates.
- Event owner checks appear in UI through direct `events` reads and `host_id` matching; backend/RLS/RPC authority must remain the enforcement layer.
- Public event/share and verification surfaces use direct reads or placeholder behavior; canonical public viewer roles are not fully defined.

## 8. Backend Authority Evidence Map

Production evidence exists:

- Target sensitive tables exist and have RLS enabled: `commerce_orders`, `event_media`, `event_staff_assignments`, `event_ticket_claims_v1`, `events`, `notifications_v2`, `push_tokens_v1`, `reservations`, `tickets`, `user_notification_settings_v1`, `venue_media`, and `venues`.
- Production policies exist for target tables, including event visibility, event media, event staff assignments, and commerce-order deny-all style evidence.
- `tickets` and `event_ticket_claims_v1` have zero policies with RLS enabled in focused evidence, implying direct access likely defaults to deny while RPC internal guards become critical.
- `checkin_ticket_by_id_v2` denies anon/public execute and checks host/status/ticket/code signals in supplied production evidence.
- `staff_checkin_ticket_v1` shows staff/host guard signals in focused evidence.
- `admin_execute_host_identity_transfer_v1` exists, has `search_path=public`, and includes an `auth_is_ops()` gate in supplied evidence.
- Storage buckets `avatars`, `venue-media`, and `venue-posters` are public with public read policies and constrained write-policy evidence.

Local-source-only or incomplete evidence:

- Mobile viewer-role and module-access helpers are frontend pure functions and do not enforce backend permissions.
- Dashboard route guards are UI gates and do not replace backend authority.
- Local Edge Function folders exist, but no deployed Supabase Edge Functions were visible in Dashboard.
- Production migration provenance supports `hostos` as strongest overall migration-source candidate, but split-source history remains unresolved.
- Full correctness of policies, broad RPC grants, entitlement overloads, proof-related RPCs, direct message authority, and public verification remains Unknown / Needs verification.

## 9. Frontend UI Gate Map

| UI/helper gate | Surface | Observed purpose | Assessment |
| --- | --- | --- | --- |
| `resolveViewerRole()` | Mobile | Derives event-context role from booleans | Acceptable as UI model; not enforcement |
| `getModuleAccess()` / module policies | Mobile | Derives module visibility and interaction | Useful mirror; risky if treated as backend authority |
| `resolveEventState()` / lifecycle helpers | Mobile | Maps statuses/aliases and feed/check-in state | Useful mirror; split lifecycle contract needs backend parity |
| `resolvePersonas()` / persona provider | Mobile | Derives personal/host persona from tier and stored preference | UI state; privileged actions need backend checks |
| `parseTier()` / cached tier fetch | Mobile/dashboard | Reads and normalizes account tier | UI mirror; tier authority must be backend-enforced |
| `getMyStaffRole()` | Mobile | Reads `event_staff_assignments` for current event | Relies on RLS; needs documented policy contract |
| `AuthGuard` | Dashboard | Requires session for dashboard routes | Acceptable UX gate; backend still required |
| `HostGuard` | Dashboard | Blocks `user` tier from dashboard | UI mirror; tier authority must be backend-enforced |
| `SemiProGuard` | Dashboard | Restricts venue routes to `semi_pro` | Revenue-sensitive; backend parity required |
| `ProGuard` | Dashboard | Allows `semi_pro` and `pro` event surfaces | UI mirror; backend parity required |
| `EventOwnerGuard` | Dashboard | Direct-read owner and dashboard-eligible event check | Useful UI gate; relies on RLS/backend owner truth |
| `OpsGuard` | Dashboard | Requires `isOps` profile flag | UI mirror; production RPC internal ops gate is stronger evidence |
| Public share rendering | Web/public | Directly reads event fields for share page | RLS-dependent; public contract incomplete |
| Public verify page | Web/public | Verification UX placeholder/deferred surface | Risky if UX implies proof authority without backend contract |

## 10. Direct Data Access / RLS Reliance Map

| Domain | Direct access observed | RLS/storage authority classification | Notes |
| --- | --- | --- | --- |
| Events | Mobile, dashboard, public web, hostos dashboard direct reads and some direct writes | RLS authority confirmed for table; policy correctness needs review | Event visibility and owner mutation contract remains central |
| Tickets | Mobile/dashboard direct reads; many RPCs | RLS enabled; zero direct policies in focused evidence | RPC-only reliance makes internal guards critical |
| Reservations | Mostly RPC; some event/venue reservation pages | RLS enabled; policy surface exists | Policy correctness remains separate |
| Event staff assignments | Mobile/dashboard direct reads and writes | RLS enabled; policy surface exists | Staff assignment authority should be documented |
| Event media / event-media storage | Mobile/gallery/notification/media helpers | Table RLS confirmed; storage policy evidence mixed by bucket | Bucket naming and public/private semantics need ADR |
| Venue media / venue-media storage | Dashboard/mobile venue media upload/list | Production public bucket and constrained write policies observed | Public read is accepted only if ADR/product decision says so |
| Venue layouts | Dashboard and hostos dashboard direct reads/writes plus layout RPCs | Unknown / Needs verification | Revenue-sensitive buyer availability impact |
| User profiles / profiles | Mobile/dashboard settings, tier, identity, social screens | Unknown / Needs verification | Profile privacy rules need explicit contract |
| Share groups / members | Mobile/web groups direct reads/writes | Unknown / Needs verification | Group membership may drive event visibility |
| Friendships / friend requests / follows | Mobile social direct reads and RPCs | Unknown / Needs verification | Privacy-sensitive social graph |
| Host identity transfers / audit log | Dashboard ops direct reads plus RPCs | Unknown / Needs verification | Production admin RPC exists; table policy not reviewed here |
| Check-in proofs | Hostos public proof route direct read | Unknown / Needs verification | Public proof contract not complete |
| Notifications / push tokens / settings | Mostly RPC; RLS enabled on v2/push/settings target tables | RLS authority confirmed for target tables | Older `notifications_v1` usage needs verification |
| App diagnostics | Mobile direct inserts/reads observed in prior inventories | Unknown / Needs verification | Ops/privacy classification needed |

## 11. Persona / Tier / Staff / Ops Boundary Assessment

Persona:

- Mobile defines `personal` and `host` personas.
- `user` tier gets personal only; `semi_pro` and `pro` get personal and host.
- Host persona can affect attribution and host-observer behavior in frontend render models.
- Persona is a UX/product identity mode, not sufficient backend authority by itself.

Tier:

- Mobile and dashboard both parse `user`, `semi_pro`, and `pro`.
- Dashboard route guards use tier to gate venue/event/dashboard surfaces.
- Tier is revenue-sensitive and must be enforced by backend/RPC/RLS for paid or host capabilities.

Staff:

- Mobile defines staff roles `scanner` and `manager`.
- Staff assignment direct access relies on `event_staff_assignments` RLS.
- `staff_checkin_ticket_v1` has positive guard signals, but the product difference between scanner and manager remains insufficiently documented.

Ops/admin:

- Dashboard `OpsGuard` reads `isOps` from profile context.
- Production admin transfer RPC includes an internal `auth_is_ops()` gate in supplied evidence.
- Broad execute grants remain notable but not final exploitability evidence.

## 12. Revenue-Sensitive Authority Assessment

Revenue-sensitive areas:

- Ticket purchase, ticket request, approval/rejection, wallet ownership, and check-in eligibility.
- Reservation creation, capacity, venue reservation decisions, and cancellation.
- Gift ticket claims, pending recipient claims, transfers, and entitlement conflicts.
- Ticket sales versus reservation mutual exclusion.
- Venue layout, seat selection, standing-area selection, product-section availability, and buyer flow.
- Tier gates for host, semi-pro, pro, venue, and commerce surfaces.

Assessment:

- Many revenue-sensitive operations use RPCs, which is the correct authority direction.
- Production/focused evidence confirms entitlement guard architecture but does not prove complete coverage.
- Multiple RPC versions/overloads and frontend helpers create determinism risk.
- Direct event and venue layout writes in dashboard surfaces need RLS/RPC authority mapping.

## 13. Privacy-Sensitive Authority Assessment

Privacy-sensitive areas:

- Event visibility, private/invite/group/friend sharing, and ended/archived visibility.
- Public web event share.
- Profiles, friend status, following, host followers, groups, and blocked relations.
- Direct messages and unread state.
- Media/gallery visibility, comments, likes, moderation, and public highlights.
- Notifications, reminders, push tokens, and notification settings.
- Storage public buckets and object paths.

Assessment:

- Production RLS evidence is positive for several sensitive tables.
- Public bucket/read exposure is confirmed and requires accepted product/security decision.
- Direct table reads for social graph, profiles, groups, and public share need documented RLS authority.
- DM RPC existence is observed in UI code, but source/provenance and production authority need verification.

## 14. Public Surface Authority Assessment

Observed public surfaces:

- Public event share pages read event data directly.
- Claim pages are handoff surfaces and state that validation happens in app.
- Public verification surface exists, but at least one web path appears deferred/placeholder-like.
- Hostos dashboard has a public proof token route reading `checkin_proofs`.

Expected authority:

- Public event share must expose only accepted public event fields.
- Claim handoff must not validate or grant ownership without backend claim authority.
- Public proof verification must be read-only and explicitly scoped.
- Public/anon storage and table policies must be documented and accepted.

Assessment:

- Public share and proof surfaces are UI-dependent or incomplete from current evidence.
- Production impact is Unknown / Needs verification.

## 15. Duplicated / Split / Unclear Authority Surfaces

- Viewer-role derivation is centralized in mobile for event detail rendering, but no accepted handbook authority contract exists.
- Dashboard route guards duplicate tier, owner, ops, and staff boundary concepts.
- Event lifecycle/status and commerce mode helpers are duplicated across mobile/dashboard.
- Multiple frontend roots expose overlapping event/ticket/claim/scan/public surfaces.
- Direct table access and RPC access coexist for several authority-sensitive domains.
- Ticketing, reservation, entitlement, and check-in functions have legacy/current versions.
- Proof-specific helper provenance strongly implicates mobile migration history, while future working target is hostos.
- Host identity transfer / ops provenance strongly implicates joinfolk-web/mobile split-source history.
- Database Functions / RPC evidence is separate from Edge Function deployment evidence.

## 16. Backend-Critical Invariants

Backend/RPC/RLS/storage/auth should enforce these invariants:

- A viewer can only see events allowed by public, private, invite, group, friend, entitlement, staff, host, or ops rules.
- Only authorized hosts/owners/ops can mutate event content, lifecycle, modules, publish state, and venue links.
- Tier gates cannot be bypassed by client route navigation.
- Ticket, reservation, order, gift, transfer, and wallet ownership cannot be derived from caller-provided IDs alone.
- Entitlement conflict checks cover all active acquisition paths.
- Staff scanner actions require host/staff/scanner/manager authority and event/ticket scoping.
- Check-in proof mutation cannot be authorized by proof metadata or caller-provided user IDs alone.
- Media upload/update/delete/moderation requires owner, host, staff, or ops authority as accepted.
- Public storage read exposure matches accepted product/security decision.
- Social graph, group, profile, DM, notification, and reminder access is scoped to the correct viewer.
- Ops/admin functions require backend ops authority even if UI gates are present.

## 17. UI-Only Mirrors That Are Acceptable

Acceptable as UI mirrors if backend authority is documented:

- Route guards that hide pages from unauthenticated, wrong-tier, non-owner, non-staff, or non-ops users.
- Disabled buttons and warnings for lifecycle, ticket setup, module mutual exclusion, and publish readiness.
- Mobile module `canView`/`canInteract` render models.
- Persona switch UI and persona-based copy/attribution hints.
- Buyer preview and venue layout visual validation.
- Public share display formatting.
- Local profile/settings visibility controls that call backend-authoritative RPCs.

These are not acceptable as the only authority for privileged actions.

## 18. Authority Gaps / Risk Register Seeds

### ABV-GAP-001

- Domain: ViewerRole canonical contract
- Current issue: Mobile has a detailed viewer-role resolver, but no accepted backend-aligned viewer-role contract exists.
- Expected clean authority contract: Product roles, implementation checks, and backend enforcement paths are documented together.
- Risk: Product correctness / security-sensitive.
- Priority candidate: Candidate P1
- Blocked by: Backend policy/RPC mapping and product role decision.
- Recommended next action: Create a viewer-role authority matrix from production policies and RPC guards.

### ABV-GAP-002

- Domain: Event visibility / public share
- Current issue: Feed/public visibility is derived in frontend helpers and public direct reads while backend policy correctness remains partially reviewed.
- Expected clean authority contract: Event visibility is backend-authoritative across feeds, public share, invite/group/friend visibility, and host views.
- Risk: Privacy-sensitive.
- Priority candidate: Candidate P1
- Blocked by: Full event policy review and public share contract.
- Recommended next action: Event Lifecycle and Visibility Contract Audit.

### ABV-GAP-003

- Domain: Tier and persona gates
- Current issue: Tier/persona state gates dashboard/mobile routes and host mode, but backend parity is not fully documented.
- Expected clean authority contract: Tier and persona are UI concepts backed by backend-enforced capability checks.
- Risk: Revenue-sensitive.
- Priority candidate: Candidate P1
- Blocked by: Tier capability and RPC guard review.
- Recommended next action: Tier / Persona Authority Review.

### ABV-GAP-004

- Domain: Direct data access
- Current issue: Direct table/storage access appears across events, tickets, staff assignments, profiles, groups, venue layouts, media, and proof data.
- Expected clean authority contract: Each direct access path has confirmed RLS/storage authority or an accepted RPC alternative.
- Risk: Security-sensitive / privacy-sensitive.
- Priority candidate: Candidate P1
- Blocked by: Direct callsite inventory and policy mapping.
- Recommended next action: Direct Data Access / RLS Reliance Audit.

### ABV-GAP-005

- Domain: Revenue-sensitive RPC overloads
- Current issue: Tickets, reservations, purchases, claims, and transfers have multiple RPC versions and overloads.
- Expected clean authority contract: Active revenue paths are named, versioned, and mapped to guard functions.
- Risk: Revenue-sensitive.
- Priority candidate: Candidate P1
- Blocked by: Production RPC body and overload review.
- Recommended next action: Commerce + Ticketing Contract Audit.

### ABV-GAP-006

- Domain: Staff/scanner roles
- Current issue: `scanner` and `manager` exist as staff subroles, but product permissions are not documented end to end.
- Expected clean authority contract: Scanner and manager capabilities are explicit and backend-enforced.
- Risk: Security-sensitive.
- Priority candidate: Candidate P2
- Blocked by: Staff assignment policy and check-in RPC review.
- Recommended next action: Staff Scanner Authority Review.

### ABV-GAP-007

- Domain: Check-in proof
- Current issue: Proof mutation/readback/public verification surfaces remain unclear.
- Expected clean authority contract: Proof mutation, proof readback, proof removal, and public verification are scoped and backend-authoritative.
- Risk: Security-sensitive.
- Priority candidate: Candidate P0 only if externally reachable proof RPCs can mutate proof state without caller authority; otherwise Candidate P1 / Unknown.
- Blocked by: Reachability and body-level authority verification.
- Recommended next action: Continue proof check-in RPC hardening verification before any patch work.

### ABV-GAP-008

- Domain: Public media/storage
- Current issue: Public buckets and public read policies are confirmed, but accepted exposure semantics are not documented.
- Expected clean authority contract: Public/private object semantics and mutation ownership are accepted by ADR.
- Risk: Privacy-sensitive.
- Priority candidate: Candidate P2 / Unknown.
- Blocked by: Product/security ADR decision.
- Recommended next action: Public Media ADR Audit.

### ABV-GAP-009

- Domain: Social graph and groups
- Current issue: Friends, following, host followers, groups, and share group membership use a mix of RPC and direct table access.
- Expected clean authority contract: Social graph and group visibility are backend-authoritative and documented.
- Risk: Privacy-sensitive.
- Priority candidate: Candidate P2 / Unknown.
- Blocked by: RLS/RPC policy review for social tables.
- Recommended next action: Social Graph Authority Audit.

### ABV-GAP-010

- Domain: Direct messages
- Current issue: DM RPCs are used in mobile/dashboard surfaces, but production/source authority remains incompletely verified.
- Expected clean authority contract: Conversation membership, message visibility, archive/delete, and unread state are backend-authoritative.
- Risk: Privacy-sensitive.
- Priority candidate: Candidate P2 / Unknown.
- Blocked by: Production RPC and source provenance review.
- Recommended next action: Messaging Contract Verification.

### ABV-GAP-011

- Domain: Ops/admin authority
- Current issue: Dashboard `OpsGuard` exists and production admin RPC has internal gate evidence, but broad grants and ops authority model need review.
- Expected clean authority contract: Ops actions are backend-gated, auditable, least-privilege, and independent of UI gates.
- Risk: Operational/admin-sensitive.
- Priority candidate: Candidate P1 / Unknown.
- Blocked by: Ops/admin RPC grant and table policy review.
- Recommended next action: Ops/Admin Authority Contract Audit.

### ABV-GAP-012

- Domain: Venue buyer/layout authority
- Current issue: Venue layout, section access, seat/standing buyer flow, and product-section mapping are spread across UI, shared packages, and RPCs.
- Expected clean authority contract: Buyer eligibility, availability, layout mutation, and product mapping are backend-authoritative.
- Risk: Revenue-sensitive.
- Priority candidate: Candidate P1.
- Blocked by: Venue layout and buyer flow contract review.
- Recommended next action: Venue Buyer Flow Contract Audit.

## 19. Product Decisions Required

- What is the accepted viewer-role vocabulary for product specs?
- Should `authenticated_non_participant` be distinct from implementation-level `authenticated`?
- What are exact scanner versus manager capabilities?
- Which staff capabilities are operational only versus social privileges?
- What is the accepted lifecycle visibility contract for ended, archived, cancelled, private, invite-only, friend, and group-shared events?
- What tier capabilities belong to `user`, `semi_pro`, and `pro`?
- Which direct table access paths are accepted because production RLS is authoritative?
- Which actions must always go through RPCs?
- What is the accepted public proof verification contract?
- What public media exposure is intentional?
- Which duplicate web/dashboard/mobile surfaces are active authority-bearing surfaces?

## 20. Recommended Next Audits

1. Commerce + Ticketing Contract Audit

   Focus on revenue-sensitive RPCs, ticket ownership, reservation ownership, entitlement guard coverage, claims, transfers, and active overloads.

2. Venue Buyer Flow Contract Audit

   Focus on venue ownership, layout mutation, section/seat/standing availability, buyer eligibility, and product-section mapping.

3. Event Lifecycle Contract Audit

   Focus on lifecycle states, aliases, visibility, feed/public share behavior, module availability, check-in timing, and transition authority.

## 21. Non-Goals

- This audit does not modify application, dashboard, mobile, web, backend, or Supabase code.
- This audit does not create SQL or migrations.
- This audit does not connect to production or inspect production state.
- This audit does not run Supabase CLI, builds, tests, installs, or deployment commands.
- This audit does not authorize cleanup, deletion, source normalization, patching, feature removal, or feature addition.
- This audit does not mark any handbook document canonical.
- This audit does not claim production vulnerability or final exploitability.

## 22. Open Questions

- Which production policies exactly define event visibility for guest, authenticated, participant, ticket holder, staff, host, group member, and public share?
- Which backend checks produce the raw booleans used by mobile `ViewerAccessInput`?
- Are `scanner` and `manager` different in backend authority or only UI presentation?
- Which direct `events` writes in dashboard surfaces are fully covered by RLS/policies?
- Are venue layout direct writes intended to be replaced by RPC authority, or are RLS policies accepted?
- Are direct group/social/profile reads and writes fully covered by RLS?
- Are DM RPCs production-active and source-owned?
- What is the accepted authority path for public proof verification?
- Which proof-related functions are externally reachable, and under which roles?
- Which broad execute grants intentionally rely on internal guards?
- Which public-role policies are intentional and product-approved?
- Which legacy RPCs remain required for older clients?

## 23. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds, tests, or installs were run.
- No files were staged or committed.
- Only `07_Audits/AuthorityBoundaryViewerRoleDeterminismAudit.md` was created/modified for this audit.

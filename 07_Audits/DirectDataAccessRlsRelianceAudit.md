# Direct Data Access / RLS Reliance Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false

## 2. Purpose

This document is a direct data access and RLS reliance audit for JoinFolk. It maps where observed product surfaces access Supabase tables and storage directly, where they use Database Functions / RPCs, and where production RLS or storage policy evidence is known from prior operator-supplied verification reports.

This is not a patch plan, cleanup plan, migration plan, implementation plan, accepted vulnerability list, or authorization to modify backend/RPC/RLS/storage/auth. It does not recommend immediate patches and does not claim production vulnerability solely because direct access exists.

The clean contract is that direct table and storage access may be acceptable for display or simple owner-scoped operations only when the RLS/storage policy contract is documented and verified. Revenue-sensitive, security-sensitive, privacy-sensitive, and ops/admin-sensitive mutations should have explicit backend/RPC/RLS/storage authority.

## 3. Audit Scope

Read-only evidence was drawn from:

- Handbook repository: `C:\dev\joinfolk-engineering-handbook`
- Platform/backend candidate repository: `C:\dev\hostos`
- Web/dashboard repository: `C:\dev\joinfolk-web`
- Mobile repository: `C:\dev\hostos\apps\mobile`

Targeted inspection focused on high-signal Supabase client patterns:

- Direct `.from(...)` table access.
- Direct table `.select`, `.insert`, `.update`, `.upsert`, and `.delete` chains.
- Storage `.from(...)`, upload, signed URL, public URL, download, and remove paths.
- `.rpc(...)` callsites that appear to carry backend authority.

This was a targeted source inspection, not a complete callsite inventory. Large generated-looking or low-signal code paths were not exhaustively enumerated.

Current system context preserved:

- Future accepted Supabase migration target: `C:\dev\hostos\supabase\migrations`.
- This target is not proof of historical sole canonical source.
- Split-source migration history remains unresolved.
- Production SQL/RPC evidence remains stronger than local source assumptions.
- Database Functions / RPC evidence is separate from Edge Function deployment evidence.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- No backend patch or migration is authorized by this audit.

## 4. Direct Data Access Contract Summary

Observed JoinFolk data access is mixed:

- Many authority-sensitive flows are RPC-mediated: lifecycle transitions, publish, ticket purchase/order, reservation mutation, claim/transfer, direct messages, notifications, reminders, media record mutation, venue mutation, venue layout saves, and check-in.
- Direct table access remains common for UI reads, draft/event editing, profile/tier reads, staff assignment management, social/group visibility helpers, event participant RSVP, dashboard ticket status updates, event session management, diagnostics, and public web event share reads.
- Storage access is directly client-side for avatars, posters, event media, venue posters, and venue media. Some storage writes are followed by RPC/table updates.
- Production evidence confirms RLS enabled for several sensitive tables, but policy correctness remains separate. RLS enabled is not the same thing as policy correctness.
- `tickets` and `event_ticket_claims_v1` were confirmed with RLS enabled and zero direct policies in focused evidence, which makes RPC internal guards critical for those tables.

The clean contract should classify each direct access path as either:

- display/read-only and acceptable with documented RLS,
- owner-scoped convenience mutation and acceptable only with verified RLS,
- authority-sensitive and preferably RPC-mediated,
- or Unknown / Needs verification.

## 5. Direct Access Inventory Matrix

| Data surface | Access type observed | Observed source surface | Expected authority owner | Production RLS evidence | Policy correctness status | Risk class | Recommendation |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `events` | Mixed direct read/write/delete plus RPC-mediated lifecycle/publish | Mobile create/control/feed/profile archive, dashboard API, public web share | Backend/RPC/RLS/auth for lifecycle and privileged mutation; direct reads only with RLS | RLS confirmed enabled; policies exist | Needs review | Privacy-sensitive, product correctness, security-sensitive | Document RLS contract and prefer RPC for lifecycle-sensitive fields |
| `event_modules` | Mostly RPC-mediated | Mobile and dashboard module APIs | Backend/RPC/RLS/auth | Not covered by supplied production evidence | Unknown / Needs verification | Product correctness, revenue-sensitive | Needs production policy verification |
| `tickets` | Dashboard direct status updates observed; mobile mostly RPC-mediated | Dashboard API approve/reject/revoke direct updates; mobile ticket RPCs | Backend/RPC/RLS/auth | RLS confirmed enabled with zero direct policies | RPC guards critical | Revenue-sensitive, security-sensitive | Reconcile direct dashboard mutation with RPC authority |
| `commerce_orders` | RPC-mediated in observed app paths | Mobile commerce/order creation | Backend/RPC/RLS/auth | RLS confirmed enabled; deny-all style authenticated policy evidence | Likely but needs review | Revenue-sensitive | Preserve RPC-only posture |
| `event_ticket_claims_v1` | RPC-mediated in observed app paths | Mobile claim/transfer APIs | Backend/RPC/RLS/auth | RLS confirmed enabled with zero direct policies | RPC guards critical | Revenue-sensitive, privacy-sensitive | Preserve RPC-only posture and verify guards |
| `reservations` | RPC-mediated in observed app paths | Mobile and dashboard reservation APIs | Backend/RPC/RLS/auth | RLS confirmed enabled; policy surface exists | Needs review | Product correctness, privacy-sensitive | Preserve RPC pattern and document policy contract |
| `venue_reservations` | RPC-mediated in observed app paths | Mobile venue-reservations v2, dashboard venue reservations | Backend/RPC/RLS/auth | Not covered by supplied production evidence | Unknown / Needs verification | Revenue-sensitive, operational/admin-sensitive | Needs production policy verification |
| `event_ticket_products_v1` | RPC-mediated in observed app paths | Mobile ticket-sales, dashboard products | Backend/RPC/RLS/auth | Not covered by supplied production evidence | Unknown / Needs verification | Revenue-sensitive | Prefer RPC authority and verify table policy |
| `event_staff_assignments` | Direct read/upsert/delete plus RPC scanner path | Mobile staff helper, dashboard API | Backend/RPC/RLS/auth or verified RLS | RLS confirmed enabled; policy surface exists | Needs review | Security-sensitive, operational-sensitive | Document RLS contract or move mutations behind accepted RPC later |
| `event_media` | RPC-mediated table mutation; storage direct; signed URL reads | Mobile event media, dashboard gallery | Backend/RPC/RLS/storage/auth | RLS confirmed enabled; policies exist | Needs review | Privacy-sensitive | Preserve RPC record mutation; verify storage bucket contract |
| `venue_media` | RPC-mediated table mutation; public storage direct | Dashboard and mobile venue media | Backend/RPC/RLS/storage/auth | RLS confirmed enabled; storage public read/write policy evidence exists | Needs ADR/security decision | Privacy-sensitive | ADR for public media semantics |
| `venues` | Mostly RPC-mediated; public/detail reads via RPC | Dashboard/mobile venue APIs | Backend/RPC/RLS/auth | RLS confirmed enabled; policies exist | Needs review | Product correctness, operational-sensitive | Preserve RPC pattern |
| `venue_layouts` | RPC-mediated observed; local layout data directness still unclear | Dashboard VenuePage, mobile seating | Backend/RPC/RLS/auth | Not covered by supplied production evidence | Unknown / Needs verification | Revenue-sensitive | Needs production policy verification |
| Layout sections/rows/seats | RPC payloads and UI model; table names not fully confirmed | Dashboard visual editor, mobile buyer flow | Backend/RPC/RLS/auth | Not covered by supplied production evidence | Unknown / Needs verification | Revenue-sensitive | Verify canonical tables and policies |
| `event_sessions_v1` | Direct read/insert/update/delete | Dashboard API and mobile session picker related flow | Backend/RPC/RLS/auth or verified RLS | Not covered by supplied production evidence | Unknown / Needs verification | Revenue-sensitive, product correctness | Candidate RPC-only or explicit RLS contract |
| `profiles` / `user_profiles` | Direct read/update | Mobile profile context/capabilities, dashboard profile/tier/ops lookup | Backend/RLS/auth; RPC for ops-sensitive fields | Not covered by supplied production evidence | Unknown / Needs verification | Privacy-sensitive, ops-sensitive for `is_ops`/tier | Document profile RLS and ops/tier authority |
| `share_groups` / `share_group_members` | Direct read/insert | Mobile create/publish audience helpers | Backend/RLS/auth or RPC | Not covered by supplied production evidence | Unknown / Needs verification | Privacy-sensitive | Needs visibility policy verification |
| Friendships / follows / host followers | Mixed direct reads and RPCs | Mobile discover affinity, social/follow helpers | Backend/RPC/RLS/auth | Not covered by supplied production evidence | Unknown / Needs verification | Privacy-sensitive | Reconcile social graph authority |
| Conversations / messages / direct messages | Mostly RPC-mediated; direct `dm_participants` read observed | Mobile and dashboard DM APIs; mobile conversation screen | Backend/RPC/RLS/auth | Not covered by supplied production evidence | Unknown / Needs verification | Privacy-sensitive | Prefer RPC authority for DM |
| `notifications_v1` / `notifications_v2` | RPC-mediated | Mobile notification APIs/reminders | Backend/RPC/RLS/auth | `notifications_v2` RLS confirmed enabled; v1 not covered | Needs review / Unknown | Privacy-sensitive | Preserve RPC pattern and verify v1/v2 split |
| `push_tokens_v1` | RPC-mediated | Mobile push token registration | Backend/RPC/RLS/auth | RLS confirmed enabled | Needs review | Privacy-sensitive | Preserve RPC pattern |
| `user_notification_settings_v1` | RPC/direct not fully established from targeted scan | Mobile notification settings domain | Backend/RPC/RLS/auth | RLS confirmed enabled | Needs review | Privacy-sensitive | Verify app access path and policy |
| Reminders | RPC-mediated | Mobile reminders | Backend/RPC/RLS/auth | Not covered by supplied table evidence; live reminder RPC search-path issue exists | Unknown / Needs verification | Privacy-sensitive | Verify table policy and SECURITY DEFINER posture |
| `checkin_proofs` | RPC/storage-related proof surfaces; table direct access not confirmed | Mobile ticket/proof helpers | Backend/RPC/RLS/auth | Not covered by supplied production evidence | Unknown / Needs verification | Security-sensitive | Candidate RPC-only surface |
| `app_diagnostics` | Direct insert | Mobile remote diagnostics | Backend/RLS/auth | Local migration RLS evidence existed in prior inventory; production not covered in supplied target list | Unknown / Needs verification | Operational/admin-sensitive, privacy-sensitive | Document diagnostics data policy |

## 6. RPC vs Direct Access Boundary

RPC authority appears preferred where a mutation has product, money, security, privacy, or ops meaning:

- Event lifecycle transitions and publish.
- Ticket purchase, order creation, payment confirmation, ticket issuance, ticket status mutation, wallet reads, and check-in.
- Gift ticket claims and transfers.
- Event and venue reservations.
- Event modules, checklist moderation, polls, voting, gallery records, and media moderation.
- Venue creation/update, layout save, product-section mapping, venue reservations, offerings, and venue media records.
- Direct messages and notification token ownership.
- Ops/admin transfer and authority-sensitive dashboard tools.

Direct reads can be acceptable when they are display-only and RLS is documented. Examples include public event share reads, profile display reads, event list/detail reads, venue poster public URL reads, and UI status/count queries.

Direct writes/mutations are authority-sensitive when they can affect lifecycle, tickets, attendance, staff, products, sessions, privacy, media ownership, notifications, or admin state. These require either a clear RLS contract or an accepted RPC boundary.

RLS enabled is not policy correctness. A table can have RLS enabled and still have policies that are too broad, too narrow, duplicative, or inconsistent with product intent. Default-deny tables with zero policies, such as `tickets` and `event_ticket_claims_v1` in focused production evidence, make RPC internal guards the primary authority path.

## 7. Production RLS Evidence Map

Known production evidence from prior handbook reports only:

- RLS enabled was confirmed for `commerce_orders`, `event_media`, `event_staff_assignments`, `event_ticket_claims_v1`, `events`, `notifications_v2`, `push_tokens_v1`, `reservations`, `tickets`, `user_notification_settings_v1`, `venue_media`, and `venues`.
- `tickets` had RLS enabled with zero direct policies in focused evidence.
- `event_ticket_claims_v1` had RLS enabled with zero direct policies in focused evidence.
- `commerce_orders` had RLS enabled with deny-all style authenticated policy evidence.
- `events` policies existed, including host insert/update/delete policies and public/authenticated select visibility policies, but full policy correctness still needs deeper review.
- `event_media` policies existed for host, participant/staff, system-managed public, own user media, and user update-own-hide examples.
- `event_staff_assignments` policies existed for select/insert/update/delete based on host ownership or staff user.
- `reservations` had policy surface evidence, including public-role usage.
- `venue_media` and `venues` had policy surface evidence.
- Storage buckets `avatars`, `venue-media`, and `venue-posters` were public with public-read policies and constrained write-policy evidence.

Not covered by supplied production RLS evidence:

- `event_modules`
- `venue_reservations`
- `event_ticket_products_v1`
- `venue_layouts` and layout section/row/seat tables
- `event_sessions_v1`
- `profiles` / `user_profiles`
- `share_groups` / `share_group_members`
- Friendships, follows, host followers, and block tables beyond RPC evidence
- Conversations, messages, `dm_participants`, and direct message tables
- `notifications_v1`
- Reminder tables
- `checkin_proofs`
- `app_diagnostics`
- Buckets such as `event-media`, `posters`, and `event-videos`

Do not treat unreviewed tables or buckets as safe.

## 8. RLS Reliance Risk Assessment

High reliance patterns:

- `tickets` and `event_ticket_claims_v1` are likely RPC-only/default-deny direct table surfaces in production. This is positive for direct table exposure, but it increases the importance of internal RPC guard correctness.
- Dashboard direct `tickets` status updates are a source-level concern because production evidence suggests zero direct policies. If those direct updates are active in production, they either fail under RLS, depend on a different policy/source path, or rely on a production state not captured by the supplied evidence. This needs verification, not an exploitability claim.
- `events` has direct create/update/delete/read patterns across mobile, dashboard, and public web. Production RLS exists, but lifecycle and visibility correctness depend on policy details and RPC boundary discipline.
- `event_staff_assignments` direct upsert/delete paths require strong RLS because staff assignment changes are security-sensitive.
- `event_sessions_v1` direct dashboard mutation and mobile buyer use are revenue-sensitive, but supplied production RLS evidence does not cover the table.
- `profiles`/`user_profiles` direct reads and updates carry privacy and authority implications for tier, ops, persona, and identity.
- Storage public-read buckets are product/security decision topics. Direct upload/remove paths require storage policy clarity and bucket inventory beyond the three production-confirmed public buckets.

## 9. Table-by-Table Authority Assessment

### `events`

- Observed access: direct read/insert/update/delete in mobile create/control flows, dashboard API, public web share page, feed fallback/system queries, profile archive, poster/video update paths; RPC-mediated publish and lifecycle transitions.
- Production evidence: RLS confirmed enabled; policies exist.
- Authority sensitivity: lifecycle, visibility, host ownership, poster/media references, venue binding, and commerce readiness are security/product/privacy sensitive.
- Assessment: mixed access. Direct event reads are expected, but direct writes should have a documented RLS contract and should not bypass lifecycle/publish RPC authority.
- Recommendation: Document which event fields may be directly edited under RLS and which are RPC-only.

### `event_modules`

- Observed access: RPC-mediated module reads/writes via `get_event_modules_v1`, `set_event_modules_v1`, and `clear_event_module_v1`.
- Production evidence: not covered in supplied production RLS summary.
- Authority sensitivity: module availability affects commerce, gallery, voting, checklist, ticket/reservation behavior.
- Assessment: mostly RPC-mediated, but table policy status remains Unknown / Needs verification.
- Recommendation: Preserve RPC path and verify production table/RLS state.

### `tickets`

- Observed access: mobile wallet, queue, purchase, approval, rejection, check-in, and ticket detail paths are heavily RPC-mediated. Dashboard API has direct `tickets` updates for approve/reject/revoke.
- Production evidence: RLS confirmed enabled with zero direct policies.
- Authority sensitivity: revenue, wallet ownership, attendance, check-in.
- Assessment: RPC-only reliance is likely intended for production direct access, making dashboard direct mutation a determinism concern.
- Recommendation: Reconcile dashboard direct ticket status mutation with accepted ticket RPC authority.

### `commerce_orders`

- Observed access: app paths use `create_commerce_order_v1` and commerce/order RPCs rather than direct table access in targeted scan.
- Production evidence: RLS enabled with deny-all style authenticated policy evidence.
- Authority sensitivity: revenue and ticket issuance.
- Assessment: positive RPC-only pattern.
- Recommendation: Preserve RPC authority and verify order/payment/issuance guard coverage separately.

### `event_ticket_claims_v1`

- Observed access: mobile claim, preview, revoke, pending gift, and transfer flows are RPC-mediated.
- Production evidence: RLS enabled with zero direct policies.
- Authority sensitivity: wallet ownership and gift transfer privacy.
- Assessment: positive RPC-only pattern with guard-dependency risk.
- Recommendation: Preserve RPC-only posture and verify active claim/transfer guard coverage.

### `reservations`

- Observed access: event reservation flows are RPC-mediated.
- Production evidence: RLS enabled; policy surface exists.
- Authority sensitivity: entitlement conflict, host approval, capacity, privacy.
- Assessment: mostly deterministic RPC boundary.
- Recommendation: Document event reservation RLS/RPC contract and verify policy correctness.

### `venue_reservations`

- Observed access: venue reservation flows are RPC-mediated in mobile and dashboard.
- Production evidence: not covered in supplied production RLS summary.
- Authority sensitivity: venue booking, host decisioning, offering capacity/time windows.
- Assessment: authority likely RPC-based; production table policy unknown.
- Recommendation: Needs production policy verification.

### `event_ticket_products_v1`

- Observed access: product reads/mutations are RPC-mediated in mobile and dashboard.
- Production evidence: not covered in supplied production RLS summary.
- Authority sensitivity: price, currency, capacity, active state, max per buyer, section mapping.
- Assessment: positive RPC pattern, but production table policy unknown.
- Recommendation: Preserve RPC authority and verify production RLS/policies.

### `event_staff_assignments`

- Observed access: direct read/upsert/delete in mobile staff helper and dashboard API; scanner action uses RPC.
- Production evidence: RLS enabled; policies exist.
- Authority sensitivity: scanner/staff/manager permissions and event operations.
- Assessment: direct mutation may be acceptable only with strong documented RLS.
- Recommendation: Document staff assignment policy contract and consider RPC-only contract later if needed.

### `event_media`

- Observed access: table record operations are mostly RPC-mediated; storage object upload/read/remove is direct.
- Production evidence: RLS enabled; policy surface exists.
- Authority sensitivity: media privacy, memory wall visibility, moderation, owner hide/delete.
- Assessment: mixed RPC/storage access; bucket inventory remains incomplete.
- Recommendation: Media / Gallery / Memory Wall Contract Audit.

### `venue_media`

- Observed access: RPC-mediated table list/add/remove/update; direct public URL and storage upload to `venue-media`.
- Production evidence: table RLS enabled; public bucket and public-read storage policy confirmed; write policy appeared host constrained.
- Authority sensitivity: public venue privacy and brand/media moderation.
- Assessment: mostly deterministic but ADR-dependent because public read is intentional only if accepted.
- Recommendation: ADR/security decision for public venue media.

### `venues`

- Observed access: venue create/update/archive/detail/list are RPC-mediated; poster public URL and media paths are direct storage.
- Production evidence: RLS enabled; policy surface exists.
- Authority sensitivity: venue ownership, business tools, public venue details.
- Assessment: positive RPC pattern with public storage dependencies.
- Recommendation: Preserve RPC pattern and verify policy correctness.

### `venue_layouts` and layout section/row/seat tables

- Observed access: layout create/save/get/link are RPC-mediated in dashboard and mobile seating; UI carries rich local geometry.
- Production evidence: not covered in supplied production RLS summary.
- Authority sensitivity: purchase availability, section/product mapping, seat and standing capacity.
- Assessment: backend authority unclear until production table/RLS and RPC bodies are verified.
- Recommendation: Verify canonical tables, RLS, and layout RPC policy.

### `event_sessions_v1`

- Observed access: direct dashboard read/insert/update/delete; mobile session picker uses session availability semantics and ticket counts.
- Production evidence: not covered in supplied production RLS summary.
- Authority sensitivity: revenue-sensitive session capacity and buyer flow.
- Assessment: direct mutations are high reliance on unknown RLS.
- Recommendation: Candidate RPC-only or explicit RLS contract.

### `profiles` / `user_profiles`

- Observed access: direct reads for tier, display name, avatar, persona, ops flag, identity resolution; direct updates for display name in dashboard.
- Production evidence: not covered in supplied production RLS summary.
- Authority sensitivity: privacy, tier/product gates, ops/admin signal.
- Assessment: display/profile reads are expected, but tier and ops fields require explicit backend authority.
- Recommendation: Profile and tier authority audit.

### `share_groups` / `share_group_members`

- Observed access: direct reads/inserts from mobile audience creation and publish diagnostics.
- Production evidence: not covered in supplied production RLS summary.
- Authority sensitivity: private/group event visibility.
- Assessment: privacy-sensitive and RLS-dependent.
- Recommendation: Verify group ownership/member policies and decide whether group mutation should be RPC-mediated.

### Friendships / follows / host followers

- Observed access: mobile discover affinity direct reads from `user_follows`; follow/block/host follower helpers also use RPCs.
- Production evidence: not covered in supplied production RLS summary.
- Authority sensitivity: social graph privacy and feed personalization.
- Assessment: mixed direct/RPC social graph authority.
- Recommendation: Social graph authority contract audit.

### Conversations / messages / direct messages

- Observed access: mobile and dashboard DM APIs are RPC-mediated; mobile conversation screen also directly reads `dm_participants` for fallback identity resolution.
- Production evidence: not covered in supplied production RLS summary.
- Authority sensitivity: privacy-sensitive.
- Assessment: RPC pattern is positive; fallback direct read needs RLS confirmation.
- Recommendation: Prefer RPC authority for DM and verify any direct participant reads.

### `notifications_v1` / `notifications_v2`

- Observed access: notification v2 read/count/mark APIs are RPC-mediated; reminders can call notification creation RPC; v1 notification API still exists.
- Production evidence: `notifications_v2` RLS enabled; `notifications_v1` not covered.
- Authority sensitivity: privacy-sensitive notification contents and read state.
- Assessment: split v1/v2 surface remains.
- Recommendation: Notification / Push / Reminder Contract Audit.

### `push_tokens_v1`

- Observed access: push token registration is RPC-mediated.
- Production evidence: RLS enabled.
- Authority sensitivity: privacy-sensitive and abuse-sensitive.
- Assessment: positive RPC pattern.
- Recommendation: Preserve RPC token ownership authority.

### `user_notification_settings_v1`

- Observed access: target table in production evidence; app access path not fully established from targeted scan.
- Production evidence: RLS enabled.
- Authority sensitivity: privacy and notification preference ownership.
- Assessment: Unknown / Needs verification.
- Recommendation: Verify access path and policy correctness.

### Reminders

- Observed access: mobile reminders use RPCs including `upsert_personal_reminder`, `list_personal_reminders`, and `delete_personal_reminder`.
- Production evidence: reminder tables not in supplied RLS list; some live reminder RPCs missing search_path proconfig in prior production evidence.
- Authority sensitivity: privacy-sensitive.
- Assessment: RPC-mediated, but function hardening and table policy unknown.
- Recommendation: Include in Notification / Push / Reminder Contract Audit.

### `checkin_proofs`

- Observed access: mobile proof readback and proof issuance are RPC-mediated; table direct access not confirmed.
- Production evidence: not covered in supplied production RLS summary.
- Authority sensitivity: security-sensitive check-in truth.
- Assessment: should remain RPC-only.
- Recommendation: Keep in proof check-in hardening review.

## 10. Storage Bucket Authority Assessment

| Bucket | Observed source use | Public read status if known | Upload/update/delete authority if known | Privacy exposure risk | Recommendation |
| --- | --- | --- | --- | --- | --- |
| `avatars` | Mobile avatar public URL and upload paths | Public in production; public read policy confirmed | Owner-path write policy evidence supplied | Privacy-sensitive but likely product-public | ADR/profile media decision |
| `venue-posters` | Dashboard venue poster public URL and upload | Public in production; public read policy confirmed | Host/owner constrained write policy evidence supplied | Public venue poster exposure | Preserve if ADR accepts public venue media |
| `venue-media` | Dashboard venue media public URL/upload and RPC registration | Public in production; public read policy confirmed | Authenticated host/owner upload policy evidence supplied | Public gallery exposure | ADR/security decision |
| `event-media` | Mobile event media upload/download/signed URLs/remove; dashboard signed URLs | Not covered by supplied production bucket evidence | Unknown / Needs verification | Privacy-sensitive event memories/media | Verify bucket status and policies |
| `posters` | Mobile poster snapshot and dashboard event poster upload/public URL | Not covered by supplied production bucket evidence | Unknown / Needs verification | Public event poster exposure | Verify bucket status and policy |
| `event-videos` | Mobile poster-video signed upload URL, signed read URL, remove | Not covered by supplied production bucket evidence | Unknown / Needs verification | Privacy/media exposure and storage abuse | Verify bucket status and policy |

## 11. Frontend Direct Read Map

Mobile notable direct reads:

- `events` for create/edit draft loading, control lists, feed fallback/system-memory query, profile archive, staff event list, and discover helper paths. Classification: RLS-dependent; some are UI display, some are authority-adjacent.
- `event_participants` for RSVP/status/profile archive and participant counts. Classification: RLS-dependent and product correctness.
- `event_staff_assignments` for staff role and staff event list. Classification: security-sensitive and RLS-dependent.
- `share_groups`, `share_group_members`, and `event_share_groups` for audience/publish behavior. Classification: privacy-sensitive and RLS-dependent.
- `user_profiles` for tier, persona, avatar, identity, and country. Classification: privacy-sensitive; tier/ops-style fields require authority contract.
- `user_follows` for discover affinity. Classification: privacy-sensitive.
- `dm_participants` fallback read. Classification: privacy-sensitive; prefer RPC authority.
- `host_identity_transfers` read in host transfer UI. Classification: ops/admin-sensitive; RLS-dependent.

Dashboard notable direct reads:

- `events` for event list/detail, owner guard, counts, and dashboard surfaces. Classification: RLS-dependent and authority-adjacent.
- `user_profiles` and `profiles` for profile, tier, ops, staff lookup, user search. Classification: privacy/ops-sensitive.
- `event_staff_assignments` for staff management. Classification: security-sensitive.
- `event_sessions_v1` for session management. Classification: revenue-sensitive.
- Storage signed/public URLs for event media and venue media/posters. Classification: storage-policy-dependent.

Public web notable direct reads:

- `events` in public event share page. Classification: public visibility/privacy-sensitive and RLS-dependent.

Hostos/shared surfaces:

- Venue shared package is mostly pure geometry/presentation logic, not direct Supabase access in targeted scan.
- Mobile and dashboard app code are the main direct data access surfaces observed.

## 12. Frontend Direct Write / Mutation Map

Mobile notable direct writes:

- `events` insert/update/delete in create/edit/draft and poster/video update paths. Classification: acceptable only with documented RLS; lifecycle-changing fields should prefer RPC authority.
- `event_participants` upsert for RSVP. Classification: acceptable only with documented RLS; product correctness sensitive.
- `event_staff_assignments` upsert/delete in staff helper. Classification: security-sensitive; should have explicit RLS or RPC authority.
- `share_groups` and `share_group_members` insert for audience groups. Classification: privacy-sensitive; RLS contract needed.
- `event_costs` upsert in control cards. Classification: product/financial display sensitive; production RLS unknown.
- `app_diagnostics` insert. Classification: operational/privacy-sensitive; diagnostics policy needed.
- Storage uploads/removes for avatars, posters, event media, and poster videos. Classification: storage-policy-dependent.

Dashboard notable direct writes:

- `events` insert/update and poster snapshot updates. Classification: acceptable only with documented RLS; lifecycle/status fields should remain RPC-authoritative.
- `tickets` status updates to approved/rejected/revoked. Classification: should prefer accepted ticket RPC authority; conflicts with production zero-policy evidence unless additional policy/source context exists.
- `user_profiles` and `profiles` display-name updates. Classification: privacy-sensitive; RLS contract needed.
- `event_staff_assignments` upsert/delete. Classification: security-sensitive; explicit RLS or RPC authority needed.
- `event_sessions_v1` insert/update/delete. Classification: revenue-sensitive; candidate RPC-only or explicit RLS contract.
- Storage uploads for `venue-posters`, `posters`, and `venue-media`. Classification: storage-policy-dependent; table registration via RPC for venue media is positive.

Public web notable direct writes:

- No public web direct mutations were observed in the targeted scan.

## 13. Dashboard Direct Access Assessment

Dashboard is the strongest observed direct mutation surface:

- It creates and updates `events` directly while using RPCs for publish and lifecycle transitions.
- It directly updates `tickets` status in local API functions while also having ticket approval/rejection RPCs elsewhere in the platform.
- It directly manages `event_staff_assignments`.
- It directly manages `event_sessions_v1`.
- It directly updates profile display names and reads tier/ops fields from `user_profiles`.
- It performs direct storage uploads and uses public/signed URLs for poster/media objects.

Assessment:

- Dashboard direct reads are often useful for dense operational UI.
- Dashboard direct writes need explicit RLS contracts because many are authority-sensitive.
- Ticket status and event session mutations are the highest-priority dashboard direct-access review areas from this audit.

## 14. Mobile Direct Access Assessment

Mobile uses many RPCs for high-risk flows, especially commerce, wallet, reservations, claims/transfers, check-in, media record mutation, notifications, direct messages, venues, and modules.

Mobile also uses direct table/storage access for:

- Event draft save/edit and some poster/video field updates.
- RSVP/event participant writes.
- Staff assignment reads and some staff assignment mutations.
- Share group and member creation/read paths.
- Social graph affinity reads.
- Profile/tier/persona reads.
- Diagnostics inserts.
- Media/avatar/poster storage upload/read/remove.

Assessment:

- Mobile has a mostly RPC-first posture for commerce and wallet truth.
- Direct event and group/profile/social access are the major RLS-reliance areas.
- Storage object mutation and event media registration need bucket-by-bucket policy clarity.

## 15. Public Web Direct Access Assessment

The public web source inspected has a direct `events` read in the event share page.

Assessment:

- Public share reads are acceptable only if event visibility RLS/policies match the product contract.
- Production evidence confirms event RLS and policies exist, but policy correctness remains Unknown / Needs verification.
- Public web direct reads should be included in a Public Web / Share Surface Audit.

## 16. Backend-Critical Direct Access Invariants

The following invariants should be backend-authoritative even if frontend direct reads or UI gates exist:

- A viewer can read an event only when event visibility, lifecycle, group/friend/participant/host relationships, and block/privacy rules allow it.
- Event lifecycle status changes occur only through accepted backend authority paths.
- Ticket purchase, order creation, payment confirmation, ticket issuance, wallet ownership, ticket status, claims/transfers, and check-in remain backend-authoritative.
- `tickets` and `event_ticket_claims_v1` direct table access remains denied unless an accepted RLS contract says otherwise.
- Staff assignment mutation requires event host or accepted staff-manager authority.
- Venue layout, event sessions, product-section mapping, and seat/standing availability are backend-authoritative at purchase time.
- Profile tier and ops/admin authority cannot be trusted from UI reads alone.
- Direct message contents and participants are private to authorized participants.
- Notification tokens and notification settings are owner-scoped.
- Storage writes are constrained by owner/host path policy and cannot use caller-chosen paths to claim someone else's object namespace.
- Public storage buckets expose only content accepted by product/security decision.

## 17. Unknown / Needs Verification Surfaces

- Production RLS/policy state for `event_modules`.
- Production RLS/policy state for `venue_reservations`.
- Production RLS/policy state for `event_ticket_products_v1`.
- Production RLS/policy state for `venue_layouts` and section/row/seat tables.
- Production RLS/policy state for `event_sessions_v1`.
- Production RLS/policy state for `profiles` and `user_profiles`.
- Production RLS/policy state for social graph and share group tables.
- Production RLS/policy state for direct message tables.
- Production RLS/policy state for reminder and diagnostics tables.
- Production storage bucket state for `event-media`, `posters`, and `event-videos`.
- Whether dashboard direct `tickets` updates are active, failing, stale, or covered by policy evidence not supplied here.
- Whether all direct `events` update paths are field-limited by RLS/policies in production.

## 18. Candidate RPC-Only Surfaces

These are candidate RPC-only surfaces, not accepted patch plans:

- Ticket purchase/order/issuance.
- Ticket status mutation.
- Gift claims and transfers.
- Event reservations and venue reservations.
- Event lifecycle status transitions.
- Check-in and check-in proof mutation.
- Venue layout publish/buyer snapshot persistence.
- Product-section mapping and session capacity mutation.
- Ops/admin transfer, tier, and media tools.
- Direct messages and read/unread mutation.
- Notification token registration and ownership.
- Public proof verification if present.
- Event session create/update/delete if sessions affect buyer capacity or pricing.

## 19. Duplicated / Split / Legacy Direct Access Surfaces

| Surface / table / helper | Observed role | Current/legacy/unknown | Risk if still active or authoritative | Evidence type | Recommendation |
| --- | --- | --- | --- | --- | --- |
| Dashboard direct `tickets` updates | Approve/reject/revoke ticket status | Unknown | Could bypass canonical ticket RPC contract if RLS permits; may fail if RLS zero-policy posture applies | Local source + production RLS evidence | Reconcile |
| Mobile/dashboard event direct writes | Draft/event field edits and poster/video updates | Current | Field-level lifecycle/visibility/product invariants may be split between UI and RLS | Local source + production RLS evidence | Document RLS contract |
| `event_sessions_v1` direct mutations | Dashboard session CRUD | Current | Buyer/session capacity could become UI/RLS-dependent | Local source only | Prefer RPC authority or verify policy |
| `event_staff_assignments` direct mutations | Staff assignment CRUD | Current | Staff/scanner authority depends on RLS correctness | Local source + production policy surface | Document or RPC-mediate |
| `share_groups` direct inserts | Audience/group creation | Current | Private visibility may depend on direct group/member RLS | Local source only | Verify policy and contract |
| Profile/tier/ops direct reads | UI guard and profile display | Current | UI may treat profile fields as authority without backend parity | Local source only | Document authority boundary |
| `event-media` storage bucket | Media upload/read/remove | Current | Bucket policy unknown in supplied production evidence | Local source only | Verify storage policy |
| `posters` and `event-videos` buckets | Poster/video upload and URL paths | Current | Bucket policy unknown in supplied production evidence | Local source only | Verify storage policy |
| `notifications_v1` vs `notifications_v2` | Split notification API generations | Split / legacy candidate | Privacy/read-state semantics may diverge | Local source + production v2 evidence | Notification audit |
| Direct `dm_participants` read | Mobile fallback identity resolution | Unknown | DM participant privacy depends on RLS if reachable | Local source only | Prefer RPC or verify policy |

## 20. Direct Access Gaps / Risk Register Seeds

### DDA-GAP-001

- Gap ID: DDA-GAP-001
- Domain: Ticket status mutation
- Current issue: Dashboard direct `tickets` status updates coexist with ticket approval/rejection RPCs and production evidence showing `tickets` RLS enabled with zero direct policies.
- Expected clean authority contract: Ticket status mutation is RPC-authoritative or explicitly allowed by a documented RLS policy.
- Risk: Revenue-sensitive and security-sensitive.
- Priority candidate: Candidate P1 / Unknown.
- Blocked by: Production policy verification and active dashboard path review.
- Recommended next action: Confirm whether direct dashboard ticket updates are active, stale, failing under RLS, or intentionally allowed elsewhere.

### DDA-GAP-002

- Gap ID: DDA-GAP-002
- Domain: Event direct mutation
- Current issue: Mobile and dashboard directly insert/update/delete `events` for drafts, edit fields, poster/video references, and dashboard updates while lifecycle/publish are RPC-mediated.
- Expected clean authority contract: Field-level event mutation contract defines which fields are direct-RLS editable and which are RPC-only.
- Risk: Product correctness, privacy-sensitive, security-sensitive.
- Priority candidate: Candidate P1.
- Blocked by: Event policy correctness review and lifecycle field contract.
- Recommended next action: Create a field-level event mutation/RLS contract audit.

### DDA-GAP-003

- Gap ID: DDA-GAP-003
- Domain: Event sessions
- Current issue: `event_sessions_v1` direct dashboard read/insert/update/delete was observed, but production RLS evidence was not supplied.
- Expected clean authority contract: Session capacity and lifecycle mutations are RPC-mediated or have a verified RLS policy contract.
- Risk: Revenue-sensitive and product correctness.
- Priority candidate: Candidate P1.
- Blocked by: Production table/RLS verification for `event_sessions_v1`.
- Recommended next action: Verify RLS/policies and decide whether sessions are candidate RPC-only.

### DDA-GAP-004

- Gap ID: DDA-GAP-004
- Domain: Staff assignment authority
- Current issue: Direct `event_staff_assignments` upsert/delete exists in mobile and dashboard while staff/scanner authority is security-sensitive.
- Expected clean authority contract: Staff assignment mutation requires backend-authoritative host/manager checks through RLS or RPC.
- Risk: Security-sensitive and operational-sensitive.
- Priority candidate: Candidate P1 / P2.
- Blocked by: Policy correctness review and staff role product decision.
- Recommended next action: Review production staff assignment policies against scanner/manager contract.

### DDA-GAP-005

- Gap ID: DDA-GAP-005
- Domain: Storage bucket coverage
- Current issue: Production evidence covers `avatars`, `venue-media`, and `venue-posters`, but local source also uses `event-media`, `posters`, and `event-videos`.
- Expected clean authority contract: Every bucket has documented public/private semantics and upload/update/delete policy.
- Risk: Privacy-sensitive.
- Priority candidate: Candidate P1 / Unknown.
- Blocked by: Production storage bucket/policy verification.
- Recommended next action: Run a storage bucket authority audit.

### DDA-GAP-006

- Gap ID: DDA-GAP-006
- Domain: Profile/tier/ops direct reads
- Current issue: Mobile and dashboard directly read `user_profiles` and `profiles` for display, tier, and ops-related UI gates.
- Expected clean authority contract: Profile display fields are RLS-documented, while tier/ops enforcement remains backend/RPC-authoritative.
- Risk: Privacy-sensitive and operational/admin-sensitive.
- Priority candidate: Candidate P2.
- Blocked by: Profile table policy verification and tier/ops authority decision.
- Recommended next action: Include in Authority Boundary follow-up.

### DDA-GAP-007

- Gap ID: DDA-GAP-007
- Domain: Social graph and share groups
- Current issue: Direct reads/inserts exist for `share_groups`, `share_group_members`, `event_share_groups`, `user_follows`, and related social visibility surfaces.
- Expected clean authority contract: Private event visibility and social graph relationships are RLS/RPC-authoritative.
- Risk: Privacy-sensitive.
- Priority candidate: Candidate P2 / Unknown.
- Blocked by: Production policy verification for social/share tables.
- Recommended next action: Public Web / Share Surface Audit plus social graph policy review.

### DDA-GAP-008

- Gap ID: DDA-GAP-008
- Domain: Direct messages
- Current issue: DM flows are mostly RPC-mediated, but `dm_participants` direct fallback read was observed and production table policy evidence was not supplied.
- Expected clean authority contract: Conversation/message/participant reads and mutations are RPC-mediated or covered by verified participant-only RLS.
- Risk: Privacy-sensitive.
- Priority candidate: Candidate P2 / Unknown.
- Blocked by: Production DM table/RLS verification.
- Recommended next action: Direct message authority review.

### DDA-GAP-009

- Gap ID: DDA-GAP-009
- Domain: Event media table/storage split
- Current issue: Event media record mutation is RPC-mediated, but storage upload/download/remove is direct and production bucket evidence for `event-media` was not supplied.
- Expected clean authority contract: Event media storage policy matches media RPC lifecycle, owner, participant, host, and public/memory visibility rules.
- Risk: Privacy-sensitive.
- Priority candidate: Candidate P1 / Unknown.
- Blocked by: Production storage policy verification for `event-media`.
- Recommended next action: Media / Gallery / Memory Wall Contract Audit.

### DDA-GAP-010

- Gap ID: DDA-GAP-010
- Domain: Diagnostics
- Current issue: Mobile inserts into `app_diagnostics`; production RLS/policy evidence was not part of the supplied target table list.
- Expected clean authority contract: Diagnostics writes are rate/owner constrained and do not expose sensitive user or device data beyond intended ops access.
- Risk: Operational/admin-sensitive and privacy-sensitive.
- Priority candidate: Candidate P3 / Unknown.
- Blocked by: Production diagnostics policy and data-retention review.
- Recommended next action: Operations telemetry/privacy audit.

## 21. Product Decisions Required

- Which event fields may be directly edited under RLS, and which are RPC-only?
- Should dashboard ticket status mutation use only accepted ticket RPCs?
- Should `event_sessions_v1` become RPC-only because it affects buyer capacity and purchase flow?
- Should staff assignment mutation remain direct-RLS or become RPC-mediated?
- What are the accepted public/private semantics for `event-media`, `posters`, and `event-videos` buckets?
- Are profile tier and ops fields display-only in UI, or are any frontend decisions currently relying on them as authority?
- Should share group creation/membership remain direct-RLS or move behind RPC?
- Should direct message participant fallback reads remain allowed?
- Which direct writes are accepted convenience mutations versus authority-sensitive mutations?

## 22. Recommended Next Audits

1. Public Web / Share Surface Audit

   Focus on public event share direct reads, event visibility policies, claim handoff, public verification, group/private visibility, and public storage exposure.

2. Notification / Push / Reminder Contract Audit

   Focus on `notifications_v1/v2`, `push_tokens_v1`, reminder RPCs with missing search_path evidence, owner-scoped read/write authority, and the absence of deployed Edge Functions.

3. Media / Gallery / Memory Wall Contract Audit

   Focus on `event_media`, `event-media` storage, public versus participant/staff visibility, media ownership, moderation, hide/delete behavior, memory wall exposure, and lifecycle-specific gallery rules.

## 23. Non-Goals

- This audit does not modify application, dashboard, mobile, web, backend, or Supabase code.
- This audit does not create SQL, migrations, patch plans, cleanup plans, or implementation instructions.
- This audit does not connect to production or inspect production directly.
- This audit does not run Supabase CLI, builds, tests, installs, or deployment commands.
- This audit does not claim direct access is unsafe solely because it exists.
- This audit does not claim RLS is correct solely because it is enabled.
- This audit does not authorize removing, adding, or changing features.
## 24. Open Questions

- Are dashboard direct `tickets` status updates current, legacy, or failing under production RLS?
- Which `events` fields are safe for direct frontend mutation under RLS?
- What is the production RLS/policy state for `event_sessions_v1`, `event_ticket_products_v1`, `event_modules`, `venue_layouts`, and layout child tables?
- What is the production bucket/policy state for `event-media`, `posters`, and `event-videos`?
- Are `profiles` and `user_profiles` policy contracts documented for tier, ops, persona, avatar, and display-name fields?
- Are share group and member direct inserts intentional product behavior?
- Are DM direct participant reads intentional, or should all DM participant identity be returned by RPC?
- Does production use any Edge Function path for direct data access? Prior manual Dashboard evidence showed no deployed Edge Functions visible.
- Which direct access paths should become gap register items versus accepted RLS contracts?

## 25. No-Modification Confirmation

For this audit:

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `07_Audits/DirectDataAccessRlsRelianceAudit.md` was created/modified.

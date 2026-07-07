# Privacy / Data Retention / Deletion Contract Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false

## 2. Purpose

This audit maps JoinFolk privacy, data retention, deletion, redaction, archival, storage, audit-log, and private-payload boundaries. It is intended to document authority and lifecycle questions before any implementation work.

This is not a patch plan, cleanup plan, migration plan, or authorization to modify backend, RPC, RLS, storage, auth, dashboard, mobile, web, or production systems.

## 3. Audit Scope

In scope:

- Account/auth user data, profiles, user profiles, public identity, host persona, avatars, events, venues, media, messages, notifications, push tokens, diagnostics, audit logs, commerce/orders, tickets, reservations, claims, social graph data, reports/moderation evidence, and public share/feed/search suppression.
- User-controlled, host-controlled, support-controlled, ops/admin-controlled, backend/RPC-mediated, direct-table, and storage lifecycle actions.
- Separation of product deletion, hide, archive, redaction, retention, and audit evidence.

Out of scope:

- Writing SQL, migrations, implementation patches, policy changes, product feature changes, or production verification.
- Treating local source as production truth where production evidence is not available.

## 4. Privacy / Data Retention / Deletion Contract Summary

JoinFolk has many privacy-sensitive data domains, but the lifecycle contract is not yet expressed as one canonical product/security document. Local source and prior handbook audits show delete, hide, archive, expiration, and retention-like concepts across events, media, messages, notifications, diagnostics, commerce, and transfer audit surfaces, but these concepts are not consistently tied to account deletion, public visibility suppression, storage cleanup, audit retention, or support/admin authority.

The strongest production-adjacent evidence remains prior production SQL/RPC/RLS reporting. Local source inspection is useful for locating likely surfaces but is not proof of production behavior. The future Supabase migration working target remains `C:\dev\hostos\supabase\migrations`; this is not historical sole canonical proof, and split-source migration history remains unresolved.

High-signal findings:

- A mobile privacy policy page states that personal data will be removed within 30 days except where retention is required by law or similar obligations. A matching in-app account deletion or user-erasure implementation was not confirmed in targeted source inspection.
- `profiles` and `user_profiles` remain privacy-sensitive identity stores. Production RLS/policy coverage for these tables was not fully covered in prior reports.
- Avatar/media storage exposure must be separated from profile/table visibility. Public storage URL presence does not make all profile or media metadata public.
- Event and public discovery code paths commonly rely on `deleted_at`, archive/status filters, and hidden flags. Feed/search/share suppression after deletion or moderation must be backend-authoritative.
- Media has owner hide/unhide/delete and host moderation concepts, but database record lifecycle and storage object lifecycle are separate and need verification.
- Direct messages have archive/delete RPC surfaces in prior messaging evidence, but exact retention, deletion, and support visibility semantics remain Unknown / Needs verification.
- Notifications, push tokens, settings, reminders, and diagnostics are privacy-sensitive. Push token deletion is separate from notification history deletion.
- Audit logs, transfer records, diagnostics, moderation evidence, commerce records, and payment/order records may require retention beyond ordinary product data deletion.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation. Local Edge Function source remains local-source-only evidence.

## 5. Privacy and Data Lifecycle Surface Inventory Matrix

| Surface / domain | Data lifecycle action or visibility exposed | Access path observed | Expected authority owner | Scope | Production evidence status | Risk class | Recommendation |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Account/auth user | Account deletion, erasure, cascade side effects | Unknown / Needs verification | Backend/RPC/RLS/auth + support/admin process | User owner, ops/admin | Not confirmed | Privacy-sensitive, compliance/audit-sensitive | Needs product decision |
| `profiles` / `user_profiles` | Read/update identity, persona, avatar, bio, tier fields | Mixed direct access and RPC/source references | Backend/RPC/RLS/auth | User owner, public approved fields, support/ops | Production RLS not fully covered | Privacy-sensitive | Verify RLS/policies |
| Public identity and avatars | Public display, avatar URLs, public profile fields | Direct reads, public storage URL use | Backend/RPC/RLS/storage | Public or authenticated depending field | Avatar bucket public-read evidence exists; profile RLS incomplete | Privacy-sensitive | Document contract |
| Events | Create/update/archive/delete/cancel/public visibility | Mixed direct reads and RPCs | Backend/RPC/RLS/auth | Host, participant, public depending state | Events RLS high-level evidence exists | Privacy-sensitive, product correctness | Reconcile |
| Venues/business data | Public venue detail, owner/business fields, media | Mixed direct/RPC evidence | Backend/RPC/RLS/auth | Public, host, venue owner, ops | RLS high-level evidence exists | Privacy-sensitive, operational-sensitive | Verify RLS/policies |
| Event media/storage | Hide, unhide, delete, moderate, public highlight | RPC-mediated and storage API evidence | Backend/RPC/RLS/storage | Uploader, host, public approved media | `event_media` RLS high-level evidence exists; storage bucket production status incomplete | Privacy-sensitive | Verify storage deletion |
| Venue media/posters | Public URLs, venue assets, delete/update lifecycle | Storage API and table access evidence | Backend/RPC/RLS/storage | Public, venue owner, host/ops | Public bucket evidence exists for venue media/posters | Privacy-sensitive | Document contract |
| Messages/conversations | Archive/delete/list/read private messages | DM RPC evidence from prior audit | Backend/RPC/RLS/auth | Conversation member | Production evidence not fully covered | Privacy-sensitive | Verify RPC body/grants |
| Notifications/history | Notification records, read state, payloads | Direct/RPC evidence | Backend/RPC/RLS/auth | Recipient, host/ops where accepted | `notifications_v2` RLS high-level evidence exists | Privacy-sensitive | Verify retention/redaction |
| Push tokens/settings/reminders | Device token create/delete, settings, reminder state | Direct/RPC evidence | Backend/RPC/RLS/auth | User owner | `push_tokens_v1` and settings RLS high-level evidence exists | Privacy-sensitive | Verify RLS/policies |
| `app_diagnostics` | Client diagnostics insert, payload retention | Direct table insert from mobile evidence | Backend/RPC/RLS/auth + support/ops read | User owner, anon safe diagnostics, ops/admin | Production RLS/policy not fully covered | Privacy-sensitive, operational-sensitive | Verify payload minimization |
| Transfer/admin audit logs | Transfer audit rows, actor/action/details | Local audit table/function evidence | Ops/admin RPC + backend | Ops/admin, audit reader | Production completeness not fully covered | Compliance/audit-sensitive | Verify append-only/retention |
| Commerce/payment/order | Orders, payment attempts, provider events, statuses | Mixed RPC/table local evidence | Backend/RPC/RLS/process | Buyer, host visibility, support/ops | `commerce_orders` deny-all style policy evidence exists | Revenue-sensitive, compliance-sensitive | Verify retention/redaction |
| Tickets/reservations/claims | Entitlement, claims, check-in, reservation status | RPC-heavy, direct reads in UI evidence | Backend/RPC/RLS/auth | Buyer, ticket holder, reservation owner, host/staff | Tickets/claims RLS enabled with zero direct policies in prior evidence | Revenue-sensitive | Document contract |
| Reports/moderation evidence | Report/evidence retention, review, appeal | Not confirmed as formal system | Backend/RPC/RLS/support process | Reporter, support/ops | Production evidence not covered | Privacy-sensitive, compliance-sensitive | Unknown / Needs verification |
| Social graph/groups/blocks | Follow, friendship, block, share group lifecycle | Mixed RPC/direct evidence | Backend/RPC/RLS/auth | User owner, friend, group member | Production evidence not fully covered | Privacy-sensitive | Verify RLS/policies |
| Public web/share/feed/search | Suppression after deleted/hidden/archive states | Direct/RPC/read helper evidence | Backend/RPC/RLS/auth | Public, anonymous, authenticated | Public/search policy correctness incomplete | Privacy-sensitive, product correctness | Reconcile |
| Support/ops/admin visibility | Private-data read/delete/redact authority | Dashboard/RPC/source references | Ops/admin RPC + audited process | Support, ops/admin | Partial production evidence only for some admin RPCs | Operational/admin-sensitive | Needs product decision |

## 6. Role Vocabulary and Authority Boundary

- User owner: authenticated account owner of account, profile, push token, notification settings, personal diagnostics, and user-owned content.
- Profile/persona owner: user owner for personal profile fields and accepted host/organizer persona fields.
- Uploader: user who uploaded media; may have owner controls where backend allows them.
- Host: event-scoped operational role; not automatically deletion authority for all user data.
- Staff: event-scoped operational role; scanner/manager authority does not imply privacy deletion authority.
- Conversation member: participant in a private conversation; membership is distinct from profile visibility, ticket ownership, host role, or staff role.
- Buyer/ticket holder: commerce/entitlement role; not general privacy deletion authority.
- Reservation owner: owner of reservation data; reservation lifecycle is not necessarily payment/refund lifecycle.
- Support: read/review role only where explicitly gated; support visibility is not mutation/deletion authority.
- Ops/admin: internal privileged role for accepted admin actions; must be internally gated and auditable.
- Service role: infrastructure authority, not ordinary user authority.
- Public/authenticated viewer: reader of approved public or authenticated product surfaces only.
- Audit reader: support/ops role allowed to inspect audit evidence where policy permits.

Deletion, hide, archive, redact, expire, and retain are separate concepts. UI-only deletion or hide is not backend authority. Product deletion does not automatically delete audit logs. Audit retention does not authorize public/product visibility. Storage deletion and database deletion are separate actions.

## 7. Account / Auth User Deletion Assessment

Account deletion or user-erasure implementation was not confirmed in targeted source inspection. A mobile privacy policy page was observed stating that personal data will be removed within 30 days, except where retention is required by law or similar obligations, but this policy text is not the same as a verified backend account deletion contract.

Expected account deletion contract:

- Define effects on `auth.users`, `profiles`, `user_profiles`, avatars, hosted events, tickets, reservations, media, messages, notifications, push tokens, diagnostics, audit logs, commerce records, reports, blocks, follows, share groups, and public surfaces.
- Separate product data deletion from audit/legal retention.
- Define redaction behavior for public content that cannot be fully removed due to commerce, event history, safety, or legal retention needs.
- Identify whether deletion is self-service, support-mediated, ops/admin-mediated, or unavailable.

Current status: Unknown / Needs verification.

## 8. Profile / Persona / Public Identity Data Assessment

`profiles` and `user_profiles` appear to be central identity stores. Prior profile/persona evidence separated personal identity fields such as `display_name`, `avatar_url`, `personal_avatar_url`, and `bio` from organizer persona fields such as `organizer_display_name`, `organizer_avatar_url`, and `organizer_bio`.

Privacy implications:

- Personal profile deletion/redaction must not accidentally delete host/business persona records needed for event trust, ticketing, audit, or transfer history without an accepted policy.
- Host persona transfer evidence indicates persona fields can be copied/transformed during ops/admin transfer, while target personal identity is preserved and source organizer fields are cleared. That behavior has retention and redaction implications.
- Public profile fields must be explicitly documented. Public avatar storage does not imply private profile fields are public.
- Production RLS/policy evidence for `profiles` and `user_profiles` remains incomplete in prior reports.

Current status: mixed local/prior evidence, with production policy completeness Unknown / Needs verification.

## 9. Event / Venue / Public Surface Data Assessment

Events and venues contain both public-facing data and private/operational data. Local evidence shows `deleted_at`, `archived_at`, lifecycle status, and visibility filters used across feed, public, and event detail paths. Some local migration evidence maps deletion-like states into archive/lifecycle behavior, which reinforces that deletion, cancellation, archive, and public suppression need a clear contract.

Expected contract:

- Event deletion/cancellation/archive must be separate from privacy deletion and legal/audit retention.
- Public share, feed, discovery, and search paths must suppress deleted, hidden, private, or archived data according to backend rules.
- Venue/business ownership data must be separated from personal profile data.
- Venue media/posters and public venue detail fields must have public-safe field contracts.

Current status: public suppression appears partially represented by lifecycle/deleted filters, but parity across feed/search/share/detail remains Needs verification.

## 10. Media / Storage Object Lifecycle Assessment

Prior media evidence found owner hide/unhide/delete controls and host moderation concepts for event media. Media lifecycle includes database records, storage paths, public URLs, signed URLs, highlight status, uploader identity, and moderation flags.

Key separations:

- Hidden media record is not the same as deleted storage object.
- Uploader delete is not the same as host moderation or ops/admin takedown.
- Removing a database row is not sufficient if a public storage object remains accessible.
- Removing a storage object is not sufficient if cached URLs, public highlights, or database references remain visible.
- Public buckets are not automatically unsafe, but they require accepted product/security semantics.

Production bucket evidence was strongest for avatars, `venue-media`, and `venue-posters`; `event-media`, poster, and video bucket deployment/policy status remains Needs verification unless covered by later production evidence.

## 11. Messaging / Private Communication Lifecycle Assessment

Prior messaging audit evidence identified direct-message RPC surfaces such as conversation creation/listing, send/read, unread/read state, archive conversation, and delete message functions. Exact production schema, delete semantics, retention period, and support/admin access were not fully confirmed.

Required distinctions:

- Message author delete, recipient-side hide, conversation archive, backend hard delete, and audit/legal retention are different actions.
- Conversation membership must remain the authority for message reads.
- Message deletion must not be inferred from UI removal alone.
- Private message bodies should not be copied into diagnostics, notification previews, report evidence, or support tools unless explicitly accepted and access-controlled.
- Support/admin private conversation viewer was not confirmed in prior targeted inspection.

Current status: RPC surfaces exist in prior evidence, but retention/deletion semantics are Unknown / Needs verification.

## 12. Notification / Push Token / Reminder Data Assessment

Notifications, push tokens, notification settings, and reminders are privacy-sensitive because they can contain actor identity, event context, private-message previews, device identifiers, delivery timing, and deep links.

Observed/prior evidence:

- `notifications_v2`, `push_tokens_v1`, and `user_notification_settings_v1` had high-level RLS evidence.
- Local migration evidence suggests some notification-related rows may cascade on user deletion, but local migration evidence is not production proof.
- Push token deletion is distinct from notification history deletion.
- Reminder deletion/expiry and local Edge Function delivery evidence remain deployment-sensitive; no deployed Edge Functions were visible in Dashboard based on manual confirmation.

Expected contract:

- Push tokens should be user-scoped and deletable.
- Notification history retention should be separate from push token deletion.
- Notification previews must not retain or expose private message/event/profile data beyond accepted semantics.
- Deep links must re-check canonical access after deletion, archive, hide, moderation, or account changes.

## 13. Diagnostics / Telemetry Data Assessment

Prior diagnostics evidence found mobile `remote-diagnostics.ts` inserting runtime state into `app_diagnostics`. Local migration evidence created `app_diagnostics`, enabled RLS, and included user-owned and anon-safe insert concepts. Production RLS/policy correctness was not fully covered.

Privacy implications:

- Client-written diagnostics are useful for debugging but low-trust for security/audit conclusions.
- Diagnostics payloads may include user IDs, event IDs, device/build data, route names, error state, or contextual metadata.
- Diagnostics should avoid secrets, tokens, payment credentials, raw message bodies, excessive location data, and unnecessary profile/persona fields.
- Support/admin read visibility for diagnostics was not confirmed as a dedicated dashboard surface.
- Retention, redaction, and deletion behavior for diagnostics remains Unknown / Needs verification.

## 14. Audit Log / Traceability Retention Assessment

Audit logs support traceability; they are not permission checks and do not replace backend authority.

Prior/local evidence includes transfer audit concepts such as `host_transfer_audit_log`, `_transfer_audit`, host identity transfer row audit fields, and admin transfer tooling. Production completeness for audit table policies, append-only behavior, retention, and redaction was not fully covered.

Expected audit-log contract:

- Admin/ops mutations should record actor, action, target, timestamp, previous/new state where appropriate, and non-secret details.
- Audit logs may need retention after product data is hidden/deleted.
- Audit logs must be access-controlled, payload-minimized, and not public/product-visible.
- Immutability or append-only semantics must not be claimed without evidence.

Current status: transfer audit evidence exists, but broader admin/moderation/commerce audit coverage is Unknown / Needs verification.

## 15. Commerce / Payment / Ticket / Reservation Retention Assessment

Commerce/order/ticket/reservation records are both product and revenue/compliance data. Prior payments evidence identified `commerce_orders`, `payment_attempts`, provider event log concepts, tickets, claims, reservations, purchase RPC versions, and order/payment-like statuses.

Important boundaries:

- Payment/order state is not the same as ticket entitlement unless backend contract defines it.
- Refund is distinct from cancellation.
- Dispute/chargeback is distinct from refund.
- Reservation status is not payment state unless product explicitly says so.
- Ticket holder/check-in state does not grant refund, dispute, or deletion authority.
- Revenue data may require retention after account deletion or product surface removal.
- Payment/provider payloads must be minimized and should not include secrets or raw sensitive provider content in product logs.

Prior production evidence: `commerce_orders` had deny-all style authenticated policy evidence; `tickets` and `event_ticket_claims_v1` had RLS enabled with zero direct policies and likely depend on RPC/default-deny assumptions.

## 16. Report / Abuse / Moderation Evidence Retention Assessment

Formal report submission/review tables or workflows were not confirmed in prior abuse/moderation audit evidence. Media moderation and block flows were observed, but a complete report evidence lifecycle was not.

Expected report/moderation evidence contract:

- Report submission is not moderation decision authority.
- Reporter identity, reported user identity, reason text, message/media/event evidence, screenshots, and support notes are privacy-sensitive.
- Report evidence should be access-controlled, payload-minimized, and retained/redacted according to safety/legal policy.
- Appeal, reversal, restore, and takedown records should have explicit semantics if the product supports them.
- Moderation traceability should be attributable but not public.

Current status: Unknown / Needs verification.

## 17. Social Graph / Groups / Block / Follow Data Assessment

Friendships, follows, host followers, blocks, share groups, and group memberships can affect profile visibility, event discovery, messaging eligibility, notifications, and public/private boundaries.

Privacy requirements:

- Social graph records should be owner/member-scoped.
- Block and unblock lifecycle should define effects on future messaging, notifications, search/profile discovery, event visibility, and previous conversation state.
- Mute, if present, must be separate from block.
- Share group membership deletion must be separated from public share token validity.
- Account deletion must define whether graph edges are deleted, anonymized, retained, or rendered inactive.

Production evidence for social graph/block/mute table policies was not fully covered.

## 18. Public Web / Share / Feed / Search Suppression Boundary

Public/share/feed/search suppression after deletion, hide, archive, cancellation, moderation, or privacy changes must be backend-authoritative.

Required behavior:

- Public routes should re-check canonical visibility and lifecycle state.
- Feed/search should not display deleted, hidden, private, or moderated content unless product explicitly allows it.
- Public share links must not bypass account deletion, profile redaction, event deletion/archive, media moderation, or private visibility rules.
- Cached/public storage URLs require separate handling from database visibility.

Current status: prior public/search audits identified feed/detail/share parity as product/security critical; complete parity remains Needs verification.

## 19. Support / Ops / Admin Data Visibility and Deletion Authority

Support/admin visibility into private data is privacy-sensitive and operational/admin-sensitive. Support read access is not deletion authority.

Expected contract:

- Support read-only views, ops/admin mutation tools, deletion/redaction tools, transfer tools, diagnostic views, and audit-log views must be separately authorized.
- Ops/admin deletion or redaction authority, if present, must be backend/RPC-gated and auditable.
- Manual deletion/redaction processes require process-level auditability.
- Public/social/host/staff surfaces must not inherit support/ops visibility.

Current evidence confirms some ops/admin transfer authority and audit concepts, but support deletion/redaction tools were not confirmed.

## 20. Payload Privacy / PII / Sensitive Data Boundary

Sensitive data categories in scope:

- User IDs, actor IDs, target IDs, reporter IDs, recipient IDs.
- Names, display names, avatars, bios, organizer/persona fields.
- Email or phone values if present in support/admin/account surfaces.
- Event/private venue details, location/geodata, event IDs, venue IDs.
- Media URLs, storage paths, signed URLs, public URLs, upload metadata.
- Message bodies, message previews, conversation IDs.
- Notification previews, push tokens, device identifiers, reminder timings.
- Diagnostics payloads, route/state/error metadata, IP/user-agent if collected.
- Payment/order/provider references, ticket/reservation/claim identifiers.
- Report reasons, abuse evidence, moderation notes, support notes.
- Audit metadata and admin action details.

Logs and diagnostics should minimize private payloads and never include secrets, service-role values, private keys, JWT secrets, API keys, or raw payment credentials. This audit did not inspect secrets.

## 21. Retention / Redaction / Archival Policy Assessment

The local mobile privacy policy text provides a product-facing retention expectation for personal data removal within 30 days, with legal/security exceptions. A full technical retention, deletion, redaction, archival, cleanup, purge, or restore policy was not confirmed.

Required policy distinctions:

- Product deletion vs archive vs hidden state.
- Hard deletion vs soft deletion.
- Redaction vs deletion.
- Public suppression vs internal/audit retention.
- Account deletion vs event/content deletion.
- Storage object deletion vs database record deletion.
- Push token deletion vs notification history deletion.
- Message deletion/archive vs conversation/audit retention.
- Commerce/legal retention vs profile/persona erasure.

Current status: Unknown / Needs verification.

## 22. Export / Portability / User Data Request Assessment

Data export, account data download, portability, or data subject request implementation was not confirmed in targeted source inspection.

Expected export contract:

- Separate user-owned data from third-party private data, audit logs, support notes, internal diagnostics, payment/legal records, and public content.
- Avoid exporting private data belonging to other users, conversation members, hosts, venues, ticket holders, or reporters.
- Define support/admin process and auditability if exports are manual.

Current status: Not confirmed.

## 23. Storage Buckets / File URL / Signed URL Boundary

Storage lifecycle must be handled separately from table lifecycle.

Prior evidence:

- Public-read evidence exists for avatar and some venue media/poster buckets.
- Public avatar storage does not imply all profile fields are public.
- Venue/media public storage must be tied to accepted product semantics.
- Event media/video/poster bucket production status was not fully covered in prior evidence.
- Signed URL presence, if used, does not remove the need for database visibility and canonical access checks.

Expected contract:

- Database deletes should identify whether storage objects are removed.
- Storage deletes should identify whether database references, public highlights, cached URLs, and audit records remain.
- Public URL exposure should be minimized for private, deleted, hidden, moderated, or expired content.

## 24. Backend RPC / RLS Authority Evidence Map

Use prior handbook evidence only; no production connection was made for this audit.

- RLS enabled high-level evidence exists for events, `event_media`, `venue_media`, venues, reservations, `commerce_orders`, tickets, `event_ticket_claims_v1`, `notifications_v2`, `push_tokens_v1`, `user_notification_settings_v1`, and `event_staff_assignments`.
- `tickets` and `event_ticket_claims_v1` had zero direct policies in prior production evidence and likely depend on RPC/default-deny assumptions.
- `commerce_orders` had deny-all style authenticated policy evidence.
- `app_diagnostics` production RLS/policy evidence was not fully covered.
- `profiles` and `user_profiles` production RLS/policy evidence was not fully covered.
- DM/conversation/message production evidence was not fully covered.
- Report/moderation tables/functions were not fully covered.
- `admin_execute_host_identity_transfer_v1` production evidence exists with `SECURITY DEFINER`, `search_path=public`, and an `auth_is_ops()` gate.
- Some `SECURITY DEFINER` functions had search-path or `proconfig=null` concerns in prior reports.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Production SQL/RPC evidence remains stronger than local source assumptions.
- Unreviewed privacy/deletion/retention functions and tables must not be treated as safe.

## 25. Direct Data Access / RLS Reliance Map

| Surface | Direct access or RPC pattern observed | RLS / authority reliance | Privacy implication |
| --- | --- | --- | --- |
| `profiles` / `user_profiles` | Mixed direct reads/updates and RPC/helper use | Production policies not fully covered | Public/private identity separation needs verification |
| Events | Direct reads plus RPC/control functions | Events RLS high-level evidence exists | Delete/archive/public suppression parity is critical |
| Venues / venue media | Direct/RPC/storage mix | RLS high-level evidence exists | Business identity and public media require accepted field contract |
| `event_media` | RPCs plus storage APIs | RLS high-level evidence exists | Hide/delete/storage cleanup must be verified |
| Messages/conversations | DM RPC evidence | Production evidence incomplete | Delete/archive semantics and member scope need verification |
| `notifications_v2` | Direct/RPC evidence | RLS high-level evidence exists | Payload retention and recipient scoping matter |
| `push_tokens_v1` | Direct token management evidence | RLS high-level evidence exists | Device token deletion must be user-scoped |
| `app_diagnostics` | Direct mobile insert evidence | Production policies incomplete | Client payloads are low-trust and privacy-sensitive |
| Transfer audit logs | Local audit function/table evidence | Production completeness incomplete | Retention/immutability/read access need verification |
| `commerce_orders` | RPC-heavy with direct support/host visibility questions | Deny-all style authenticated policy evidence | Revenue retention and privacy deletion differ |
| Tickets / claims | RPC-heavy; zero direct policies in prior evidence | Default-deny/RPC assumptions | Entitlement retention may outlive account deletion |
| Reservations | RPC/direct read evidence | RLS high-level evidence exists | Reservation deletion is not refund/payment deletion |
| Reports/moderation | Not confirmed as formal table set | Unknown | Evidence privacy and retention unresolved |
| Friendships/follows/blocks/share groups | Mixed RPC/direct evidence | Production policies incomplete | Graph deletion affects visibility and notifications |
| Public feed/search/share | Direct/RPC/helper evidence | Depends on underlying tables/RPCs | Suppression must be backend-authoritative |

## 26. Duplicated / Split / Legacy Privacy-Deletion Surfaces

| Surface / helper / RPC / table | Observed role | Current / legacy / unknown | Risk if still active or authoritative | Evidence type | Recommendation |
| --- | --- | --- | --- | --- | --- |
| Mobile privacy policy text | User-facing deletion/retention expectation | Current / unknown implementation backing | Policy promise may not map to backend lifecycle | Local source | Reconcile |
| `deleted_at` / `archived_at` / lifecycle status | Event suppression/archive/delete concepts | Current / mixed | Archive and delete semantics may diverge across surfaces | Local + prior audits | Document contract |
| `hide_owned_media_v1` / `unhide_owned_media_v1` / `delete_owned_media_v1` | Media owner lifecycle controls | Current / unknown production body | DB/storage mismatch or public URL residue | Prior source evidence | Verify storage deletion |
| `host_moderate_media_v1` | Host moderation control | Current / body incomplete | Host moderation may be mistaken for owner deletion or ops takedown | Prior audit evidence | Verify RPC body/grants |
| DM archive/delete RPCs | Private messaging lifecycle | Current / unknown semantics | UI archive/delete may be mistaken for hard deletion | Prior messaging audit | Document contract |
| Push token delete vs notification history | Device token lifecycle vs product history | Current / split | Token removal may not remove old payloads | Prior notification audit | Verify retention/redaction |
| `app_diagnostics` direct insert | Client telemetry | Current / production policy incomplete | Private payloads may persist without retention rules | Local + prior audit | Verify payload minimization |
| Transfer audit logs | Admin action traceability | Current / incomplete retention | Audit rows may need redaction/access policy while retained | Local + prior audit | Verify append-only semantics |
| Local Edge Function source | Possible logging/delivery behavior | Unknown deployment | Local source may be mistaken for production behavior | Handbook/source evidence | Separate deployment evidence |

## 27. Privacy-Retention-Deletion-Critical Invariants

- Product deletion, archive, hide, and redaction are separate concepts.
- UI deletion is not backend deletion.
- Storage deletion and database deletion are separate.
- Product deletion does not automatically delete audit logs.
- Audit retention does not authorize public/product visibility.
- Support read visibility is not deletion authority.
- Public visibility suppression is backend-authoritative.
- Message deletion/archive semantics must be explicit.
- Push tokens are private and should be user-scoped/deletable.
- Diagnostics are privacy-sensitive and low-trust if client-written.
- Payment/order/ticket records may require retention beyond account deletion.
- Report/moderation evidence retention must be policy-defined.
- Host/staff operational roles do not imply privacy deletion authority.
- Public storage URLs do not make all metadata public.
- Local Edge Function source is not deployment evidence.
- No logs should contain secrets.

## 28. Unknown / Needs Verification Surfaces

- Whether a self-service account deletion or backend user-erasure flow exists.
- Whether mobile privacy policy deletion timelines map to implemented backend behavior.
- Production RLS/policy completeness for `profiles`, `user_profiles`, `app_diagnostics`, DM tables, social graph tables, and report/moderation tables.
- Exact profile/persona redaction behavior after account deletion, host transfer, or support deletion.
- Event delete/archive/cancel parity across Home, Discover, Search, public web/share, and event detail.
- Storage cleanup behavior for deleted event media, avatars, posters, venue media, and videos.
- Message delete/archive retention, hard delete, and support visibility semantics.
- Notification history retention after account deletion, push token deletion, and block/mute/report changes.
- Diagnostics retention, redaction, and support/admin read surface.
- Audit log append-only behavior, access controls, retention, and redaction.
- Commerce/payment/provider log retention and user erasure exceptions.
- Report/moderation evidence model, retention, redaction, appeal, and support review.
- Data export / portability implementation.

## 29. Privacy / Data Retention / Deletion Gaps / Risk Register Seeds

| Gap ID | Domain | Current issue | Expected clean privacy/retention/deletion contract | Risk | Priority candidate | Blocked by | Recommended next action |
| --- | --- | --- | --- | --- | --- | --- | --- |
| PRV-GAP-001 | Account deletion | Account deletion implementation was not confirmed, while privacy text creates a user-facing retention expectation | Canonical account deletion/erasure contract covering all user data domains and legal exceptions | Privacy-sensitive, compliance-sensitive | Candidate P1 | Product/legal deletion policy and production verification | Define account deletion semantics and verify existing backend support |
| PRV-GAP-002 | Profile/persona data | Personal profile, public profile, host persona, avatar, and transfer retention/redaction rules are split | Separate personal identity, host persona, public fields, and transfer audit retention | Privacy-sensitive | Candidate P2 | Profile/persona product decisions and RLS verification | Document redaction/deletion behavior per field class |
| PRV-GAP-003 | Media/storage | Media hide/delete/moderate behavior may not prove storage object deletion or URL invalidation | DB record lifecycle and storage object lifecycle verified separately | Privacy-sensitive | Candidate P1 | Storage bucket policy and delete RPC verification | Verify media deletion behavior and public URL suppression |
| PRV-GAP-004 | Messaging lifecycle | DM archive/delete functions exist in prior evidence, but semantics are unclear | Explicit message delete, archive, retention, and support visibility contract | Privacy-sensitive | Candidate P2 | DM schema/RPC production review | Verify DM lifecycle functions and retention behavior |
| PRV-GAP-005 | Notifications/push/reminders | Push token deletion, notification history deletion, and reminder retention are separate but not fully documented | User-scoped token deletion and documented notification/reminder retention | Privacy-sensitive | Candidate P2 | Notification RLS/payload review | Define deletion and retention rules for tokens/history/reminders |
| PRV-GAP-006 | Diagnostics | `app_diagnostics` direct client inserts may retain private payloads without confirmed retention/read policy | Payload-minimized diagnostics with explicit write/read/retention/redaction rules | Privacy-sensitive, operational-sensitive | Candidate P2 | Diagnostics policy and production RLS verification | Verify diagnostics payloads, RLS, retention, and admin visibility |
| PRV-GAP-007 | Audit logs | Transfer/admin audit evidence exists, but append-only/retention/redaction/access semantics are incomplete | Audit logs retained, access-controlled, payload-minimized, and tamper-resistant where required | Compliance/audit-sensitive | Candidate P2 | Audit table/function production review | Define audit log retention and redaction boundaries |
| PRV-GAP-008 | Commerce/ticket retention | Revenue data likely needs retention beyond product/account deletion, but policy is not canonical | Legal/operational retention policy for orders, payments, tickets, claims, reservations, and refunds/disputes | Revenue-sensitive, compliance-sensitive | Candidate P1 | Payments/legal policy and provider evidence | Map commerce records to retention and erasure exceptions |
| PRV-GAP-009 | Report/moderation evidence | Formal report evidence lifecycle was not confirmed | Report evidence retention/redaction/appeal/support-review contract | Privacy-sensitive, compliance-sensitive | Candidate P2 | Abuse/reporting product and backend verification | Define moderation evidence policy before implementation changes |
| PRV-GAP-010 | Public suppression parity | Deleted/hidden/archived/private data suppression across feed/search/share/detail is not fully proven | Backend-authoritative suppression parity for all public surfaces | Privacy-sensitive, product correctness | Candidate P1 | Search/public route parity review | Verify suppression behavior across canonical public routes |

## 30. Product Decisions Required

- Should JoinFolk provide self-service account deletion, support-mediated deletion, or both?
- Which account data must be deleted, redacted, anonymized, retained, or archived?
- Which profile/persona fields remain for hosted events, commerce, audit, trust, or transfer history after account deletion?
- What is the accepted retention period for messages, notifications, push tokens, diagnostics, report evidence, media, commerce records, and audit logs?
- Which public content should remain visible after account deletion, and under what redaction rules?
- Should media delete remove storage objects immediately, queue cleanup, or retain objects under a legal/safety exception?
- What data export scope is required, and how should third-party/private data be excluded?
- Which support/admin reads and deletion/redaction actions require audit records?

## 31. Recommended Next Audits

- Legal / Trust & Safety Policy Mapping Audit.
- Release Readiness / Production Hardening Gap Register.
- Data Export / Account Deletion Implementation Gap Audit.

## 32. Non-Goals

- No application code changes.
- No dashboard, mobile, or web changes.
- No Supabase tree changes.
- No SQL, migrations, policies, RPC patches, or storage policy changes.
- No production connection or Supabase CLI use.
- No claim that incomplete evidence is safe or unsafe by itself.
- No recommendation for immediate patches.
- No feature removal or behavior change recommendation.

## 33. Open Questions

- Is there an implemented account deletion or erasure flow, or only policy/support text?
- What is the authoritative deletion timeline for user personal data?
- Which records are retained for legal, financial, safety, fraud, moderation, or audit reasons?
- How are `profiles` and `user_profiles` redacted when an account is deleted but hosted events or tickets remain?
- Are media storage objects removed when media database records are deleted?
- Do public URLs or signed URLs remain valid after database deletion/hide/moderation?
- What do DM delete and archive functions actually delete, hide, or retain?
- Are notification records purged, redacted, or retained after account deletion?
- Are push tokens deleted on logout, uninstall, account deletion, or settings changes?
- What is the retention policy for `app_diagnostics` and support-visible diagnostic payloads?
- Are audit logs append-only, and who can read or redact them?
- Are report/moderation evidence records implemented, and what is their retention/redaction policy?
- Which public/feed/search/share surfaces re-check deleted/hidden/archived state at read time?
- Is data export/portability supported, planned, or manual?

## 34. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `07_Audits/PrivacyDataRetentionDeletionContractAudit.md` was created/modified.

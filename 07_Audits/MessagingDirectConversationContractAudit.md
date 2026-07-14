# Messaging / Direct Conversation Contract Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false

## 2. Purpose

This audit maps JoinFolk messaging, direct conversation, and private communication authority boundaries across mobile, dashboard, backend Database Functions / RPCs, RLS, notifications, identity display, social graph rules, public/deep-link boundaries, and possible support/admin visibility.

This is not an implementation plan, patch plan, cleanup plan, migration plan, or authorization to modify backend/RPC/RLS/storage/auth behavior.

The audit separates:

- direct conversation membership from friendship, follow, group, event participation, ticket ownership, checked-in state, staff role, and host authority
- sender authority from recipient read authority
- private message content from notification and push preview payloads
- messaging identity display from profile/persona authority
- block/mute effects from notification settings
- mobile/dashboard UI from backend/RPC/RLS message authority
- support/admin visibility from ordinary user conversation visibility
- production evidence from local-source-only evidence
- Database Functions / RPC evidence from Edge Function deployment evidence

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

- DM/conversation screens and helpers.
- Mobile and dashboard RPC wrappers.
- Conversation participant fallback reads.
- Notification and social graph audit carryover.
- Local migration provenance at function/table-name level only.
- Public/deep-link and identity interactions at a high level.

## 4. Messaging / Direct Conversation Contract Summary

JoinFolk has a Direct Messages V1 surface in both mobile and dashboard:

- Mobile exposes `/messages` and `/messages/[conversationId]` screens.
- Dashboard exposes a host/persona-oriented messages page.
- Mobile and dashboard wrappers call the same DM RPC family for conversation creation, message send/list, conversation list, mark read, unread count, archive, and soft delete.
- DM rows are persona-aware with `personal` and `host` persona scopes.
- Mobile conversation header resolution uses a direct `dm_participants` fallback read when no message from the other participant exists yet.
- Dashboard identity resolution directly reads `user_profiles` for other participant display fields.
- Local migration provenance for DM V1 is strongest under `C:\dev\hostos\apps\mobile\supabase\migrations`, not the future working target path.

Clean contract expectation:

- Conversation membership is the core read/list/send authority.
- Message send, read, archive, delete, unread count, and mark-read behavior should remain backend/RPC/RLS-authoritative.
- Persona scope is a display and routing dimension, not permission by itself.
- Notification previews and push payloads must not expose private message body or linked private context to non-members.
- Message deep links must re-check conversation membership on open.
- Direct participant/profile reads are acceptable only with verified RLS/policy contracts.

## 5. Messaging Surface Inventory Matrix

| Surface / domain | Messaging action or visibility exposed | Access path observed | Expected authority owner | Scope | Production evidence status | Risk class | Recommendation |
|---|---|---|---|---|---|---|---|
| Mobile DM inbox | Lists conversations, previews, unread counts | `dm_get_conversations_v1` RPC; identity helper reads | Backend/RPC/RLS/auth | Conversation member, persona scope | `10f COMPLETE_VALIDATED` for participant-scoped authenticated policies and `11a COMPLETE_VALIDATED` for exact live production bodies | Privacy-sensitive | Preserve RPC read and verify identity field contract |
| Mobile conversation thread | Lists messages, sends messages, marks read | `dm_get_messages_v1`, `dm_send_message_v1`, `dm_mark_read_v1` | Backend/RPC/RLS/auth | Conversation member, sender | `11a COMPLETE_VALIDATED` for exact live production bodies; active mobile caller parity confirmed | Privacy-sensitive | Preserve RPC authority; future patch scope is anon-only EXECUTE containment |
| Mobile header fallback | Resolves other participant when only own message exists | Direct `dm_participants` read | Backend/RLS/auth | Conversation member | Not covered by supplied production reports | Privacy-sensitive | Verify RLS or move behind RPC later |
| Dashboard host inbox | Lists host-persona conversations and unread counts | `dm_get_conversations_v1`, `dm_get_unread_count_v1` with host persona | Backend/RPC/RLS/auth | Host persona conversation member | `11a COMPLETE_VALIDATED` for exact live production bodies; active dashboard caller parity confirmed | Privacy-sensitive / Operational | Preserve RPC authority; keep host-persona membership semantics documented |
| Dashboard host thread | Reads/sends/marks host-persona messages | `dm_get_messages_v1`, `dm_send_message_v1`, `dm_mark_read_v1` | Backend/RPC/RLS/auth | Host persona conversation member | `11a COMPLETE_VALIDATED` for exact live production bodies; active dashboard caller parity confirmed | Privacy-sensitive | Preserve RPC authority |
| Dashboard identity display | Shows other participant display/avatar fields | Direct `user_profiles` read | Backend/RLS/auth | Dashboard host user | Profile RLS not fully covered | Privacy-sensitive | Verify public/private identity field contract |
| Conversation creation | Opens personal or host conversation | `dm_get_or_create_conversation_v1` | Backend/RPC/RLS/auth | Authenticated sender and target | `11a COMPLETE_VALIDATED`; later production body matches the v1.5 host-contact behavior required by mobile | Privacy-sensitive / Product correctness | Preserve eligibility rules; future patch scope is anon-only EXECUTE containment |
| Message deletion | Soft delete own message | `dm_delete_message_v1` | Backend/RPC/RLS/auth | Sender or explicit admin | `11a COMPLETE_VALIDATED`; wrapper exists but no confirmed active UI caller | Privacy-sensitive | Preserve sender-only soft-delete contract |
| Archive conversation | Hide/archive for one persona/member | `dm_archive_conversation_v1` | Backend/RPC/RLS/auth | Conversation participant | `11a COMPLETE_VALIDATED`; wrapper exists but no confirmed active UI caller | Product correctness | Preserve per-participant archive semantics |
| Notification preview | DM preview in notification center/push | Notification V2 rows include conversation fields; DM send migration evidence mentions notification insert | Backend/RPC/RLS/auth plus delivery path | Recipient | `notifications_v2` RLS confirmed; DM payload correctness not reviewed | Privacy-sensitive | Minimize previews and verify payload |
| Block/friend/host-contact eligibility | Allows or rejects starting conversation | Error mapping and migration provenance mention blocked/friendship/host-contact checks | Backend/RPC/RLS/auth | Authenticated social/host context | Social table production evidence incomplete | Privacy-sensitive | Document eligibility model |
| Realtime subscription | Live message delivery | No active realtime subscription found in inspected DM screens | Unknown / Needs verification | Conversation member | Not covered | Privacy-sensitive | Keep Unknown; verify if added later |
| Support/admin messaging | Possible support visibility into private messages | No support/admin DM view confirmed in targeted source | Ops/admin RPC if present | Ops/admin/support | Not evidenced | Operational/admin-sensitive | Do not claim exists; define before adding |
| DM photo messages | Private photo message design | Deferred design doc only | Backend/RPC/RLS/storage/auth | Conversation member | Not runtime evidence | Privacy-sensitive | Keep deferred; no implementation assumed |

## 6. Conversation Data Model Assessment

Observed local provenance and wrappers indicate a DM V1 model with:

- `dm_conversations`
- `dm_participants`
- `dm_messages`
- conversation IDs
- participant user IDs
- persona scope values such as `personal` and `host`
- sender user ID
- sender persona
- message body
- created timestamp
- deleted marker
- archived state per participant/persona
- unread count derived from participant/message state
- read state represented through mark-read and unread-count RPC behavior

Exact production schema parity is Unknown / Needs verification because supplied production reports did not directly verify DM tables or policies.

No separate `conversation_members`, `conversation_participants`, `direct_messages`, or `message_reads` production tables were confirmed in supplied evidence. They remain candidate names only unless future source or production evidence proves them.

## 7. Conversation Membership Authority Assessment

Membership is the core authority for messaging.

Observed evidence:

- All main mobile and dashboard read/send/list/archive/delete wrappers use DM RPCs.
- The RPC family is persona-aware.
- Mobile performs one direct `dm_participants` fallback read for header identity when no incoming message exists.
- Local migration provenance shows participant tables and member-scoped policy/function intent, but this was local source evidence.

Expected contract:

- A user can list/read a conversation only if they are an authorized participant for the relevant persona scope.
- Conversation membership cannot be inferred from route parameters.
- Conversation membership is distinct from friendship, follows, group membership, event participation, ticket ownership, checked-in state, host role, and staff role.
- Host persona conversation access requires membership in that host persona context, not just event host authority.

Status: Mostly RPC-mediated, but direct participant fallback and production policy evidence remain Needs verification.

## 8. Message Send Authority Assessment

Observed evidence:

- Mobile sends through `dm_send_message_v1` with `conversationId`, active persona, and body.
- Dashboard sends through `dm_send_message_v1` with host persona.
- Error mapping includes blocked, friendship required, host-contact relation required, target persona unavailable, host DM unavailable, rate-limited, empty message, message too long, not participant, and self-DM not allowed cases.
- Local migration verification comments reference block, friendship, rate limit, and notification insert checks.

Expected contract:

- Sender must be authenticated.
- Sender must be an authorized conversation participant for the persona used.
- Sender persona cannot be selected purely by UI if backend does not confirm it.
- Send eligibility should respect block, mute/report, friendship, host-contact, rate limit, and profile/persona availability rules where product requires.
- Message body validation belongs in backend authority, with UI validation as a mirror.

Status: RPC-mediated and privacy-sensitive. Production body/grant parity remains Unknown / Needs verification.

## 9. Message Read / List Authority Assessment

Observed evidence:

- Mobile and dashboard list messages through `dm_get_messages_v1`.
- Mobile and dashboard list conversations through `dm_get_conversations_v1`.
- Conversation list rows include other user ID, other persona scope, last message preview, last message time, unread count, and archive status.
- Mobile and dashboard separately resolve participant identity for display.

Expected contract:

- Reading/listing messages must be conversation-member scoped.
- Conversation list previews are private message content and must be recipient/member scoped.
- Last-message previews should respect deletion/visibility semantics.
- Archived conversations should hide only for the participant/persona that archived them unless product says otherwise.
- Support/admin reads, if ever present, require explicit ops/admin authority and audit semantics.

Status: Mostly RPC-mediated. Direct identity and participant fallback reads need RLS confirmation.

## 10. Unread Counts / Read Receipt Assessment

Observed evidence:

- `dm_get_unread_count_v1` returns persona-scoped unread counts.
- `dm_mark_read_v1` marks a conversation read for a persona scope.
- Mobile marks read on conversation focus.
- Dashboard marks host conversation read after loading a selected thread.

Expected contract:

- Unread count is scoped to the authenticated participant and persona.
- Mark-read updates only the caller's participant/read state.
- One participant must not be able to mark another participant's messages/read state.
- Notification unread counts should match conversation read state or document intentional differences.

Status: RPC-mediated; production policy and function body evidence remain Needs verification.

## 11. Notification / Push Preview Assessment

Observed evidence:

- DM rows expose `last_message_preview` in conversation list.
- Notification audit identified `notifications_v2` rows with actor, target persona, conversation, media, deep link, read, seen, and grouping fields.
- Local DM migration provenance references notification side effects from message sending.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.

Expected contract:

- Notification previews must not leak private message content to non-members.
- Push payloads should minimize message body content and prefer generic previews where privacy settings require.
- Deep links from notifications must re-check conversation membership on open.
- Notification actor identity must follow profile/persona public/private field contracts.
- Notification delivery is separate from message storage authority.

Status: Privacy-sensitive. `notifications_v2` RLS was confirmed at a high level, but message payload correctness remains Needs verification.

## 12. Block / Mute / Report Interaction Assessment

Observed evidence:

- DM error mapping includes blocked and relationship-required cases.
- Local migration verification comments reference block checks for `dm_get_or_create_conversation_v1`.
- Social graph audit found block/follow/friendship RPCs, but production evidence for social tables was not fully covered.
- No dedicated report/moderation DM runtime surface was confirmed in targeted DM files.

Expected contract:

- Blocks may affect conversation creation, send eligibility, list visibility, notification eligibility, and push delivery according to product rules.
- Mutes may suppress notification/push without necessarily removing read access.
- Reports/moderation, if added, must not expose message content outside authorized support/admin workflows.
- Block/mute effects should be backend-authoritative, not UI-only.

Status: Product intent appears present for blocks and relationship gates; full mute/report behavior is Unknown / Needs verification.

## 13. Social Graph / Profile Visibility Interaction

Observed evidence:

- DM creation error mapping includes friendship-required and host-contact relation-required states.
- Profile pages include buttons to message a personal profile or contact host, routing into DM conversation IDs.
- Dashboard host inbox uses host persona by default.
- Identity resolution uses `user_profiles` and persona-specific display/avatar fields.

Expected contract:

- Friendship, follow, host follower, and profile visibility may affect who can initiate a conversation.
- Once a conversation exists, read authority should be membership-based.
- Public profile visibility does not imply permission to message.
- Host persona messaging should use accepted host persona fields, not private account fields.
- Persona selection cannot bypass profile, host-contact, block, or relationship rules.

Status: Mixed product/social boundary. Needs product decision on who may message personal profiles and host personas.

## 14. Event / Media / Profile / Ticket Link Boundary Assessment

Observed evidence:

- Targeted DM files did not show structured event/media/ticket/claim/reservation attachment handling.
- Public/share and notification audits identified deep links across events, claims, tickets, media, profiles, and conversations.
- DM photo message design is deferred and not runtime evidence.

Expected contract:

- Message text may contain links, but opening linked private resources must re-check canonical resource authority.
- Conversation membership does not automatically grant access to private events, media, tickets, claims, reservations, venues, or profiles.
- Notification previews must not expose private linked resource details.
- Future structured attachments should store type and target metadata with backend validation.

Status: Runtime link authority is Unknown / Needs verification. No feature removal or patch is authorized.

## 15. Realtime Subscription Authority Assessment

Observed evidence:

- Inspected mobile conversation screen reloads messages after send and on focus.
- No active DM realtime subscription/channel was found in the focused DM screen inspection.
- Generic auth subscription code exists elsewhere but is not DM message delivery authority.

Expected contract:

- Realtime delivery, if introduced, must be scoped to authorized conversation members.
- Channel names, route params, or client filters are not security controls.
- Realtime payloads should not include private messages for non-members.

Status: No active DM realtime delivery confirmed; Needs verification if added later.

## 16. Support / Ops / Admin Messaging Visibility Assessment

Observed evidence:

- No support/admin private conversation viewer was confirmed in targeted DM source.
- Dashboard host messaging is a host-persona inbox, not proof of support/admin visibility.
- Prior staff/ops audit recommended a separate ops/admin/support tools audit because private messaging may intersect with support workflows.

Expected contract:

- Support/admin visibility into private conversations, if present, is operational/admin-sensitive.
- Ops/admin reads must be internally gated and auditable.
- Dashboard/support UI guards are not sufficient authority.
- Ordinary host/staff/event authority must not grant private DM visibility.

Status: Unknown / Needs verification. Do not claim support/admin visibility exists.

## 17. Dashboard / Support Surface Map

Observed dashboard surfaces:

- `C:\dev\joinfolk-web\dashboard\src\pages\MessagesPage.tsx`
- `C:\dev\joinfolk-web\dashboard\src\lib\dm.ts`

Dashboard behavior:

- Uses host persona wrappers for conversation list, messages, send, mark-read, and unread count.
- Reads `user_profiles` directly for identity display.
- Uses query string conversation ID to select the active thread, then calls RPCs for actual message reads.

Authority classification:

- Dashboard route/query parameters are not authority.
- Host persona DM authority should be enforced by DM RPCs and RLS.
- Direct identity reads must follow profile/persona public/private field contract.
- No support/admin override behavior was confirmed in this surface.

## 18. Mobile Messaging Surface Map

Observed mobile surfaces:

- `C:\dev\hostos\apps\mobile\app\messages\index.tsx`
- `C:\dev\hostos\apps\mobile\app\messages\[conversationId].tsx`
- `C:\dev\hostos\apps\mobile\screens\messages\MessagesListScreen.tsx`
- `C:\dev\hostos\apps\mobile\screens\messages\ConversationScreen.tsx`
- `C:\dev\hostos\apps\mobile\lib\dm.v1.ts`
- Profile screens with message/contact-host entry points.

Mobile behavior:

- Lists conversations through RPC.
- Lists messages through RPC.
- Sends through RPC.
- Marks read through RPC.
- Resolves other participant identity using profile identity helpers.
- Falls back to direct `dm_participants` read for a header edge case.

Authority classification:

- Mobile UI is a mirror, not authority.
- RPCs should enforce send/read/mark/delete/archive behavior.
- Direct fallback participant read is RLS-dependent and should be reviewed.

## 19. Public Web / Deep Link Boundary Assessment

Observed evidence:

- Mobile and notification surfaces use route/deep-link patterns into conversations.
- Public web audit focused public event/share/claim/verification surfaces; no public web private DM reader was confirmed.
- Message notification deep links must route to a private conversation screen.

Expected contract:

- Public web/deep links must never expose private conversation content.
- Opening a message route must re-check authenticated conversation membership.
- Claim/event/profile/media links inside messages must re-check their own canonical authority.
- A conversation ID in a URL is not access authority.

Status: Needs verification for all deep-link entry points.

## 20. ViewerRole / Entitlement / Messaging Interaction Map

Messaging authority is separate from:

- `guest`
- `authenticated_non_participant`
- `participant`
- `ticket_holder`
- `checked_in`
- `host`
- `staff`
- `scanner`
- `manager`
- `ops/admin`

Interaction rules:

- Conversation membership is the read/send authority for DM content.
- Event participant, ticket-holder, checked-in, host, or staff status does not automatically grant conversation access.
- Host persona may be used in a conversation only if the backend accepts that persona scope.
- Ops/admin visibility is not implied by role labels unless explicit audited support/admin authority exists.

## 21. Backend RPC / RLS Authority Evidence Map

Prior handbook and local evidence only:

- Production RLS/policy evidence for DM/conversation/message tables was not fully covered in supplied production reports.
- Direct Data Access / RLS Reliance Audit flagged conversations/messages/DMs as follow-up scope.
- Direct Data Access audit found DM surfaces mostly RPC-mediated, with a direct `dm_participants` read observed.
- Local mobile Supabase migration history contains DM V1 schema, RPC, verification, and host-contact provenance.
- Local DM function/table provenance is strongest in `C:\dev\hostos\apps\mobile\supabase\migrations`, which reinforces split-source migration caveats.
- `notifications_v2` RLS was confirmed enabled at a high level, but payload policy correctness still needs review.
- Profiles/user_profiles production RLS/policy evidence was not fully covered.
- Social graph/block/mute table production evidence was not fully covered.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Production SQL/RPC evidence remains stronger than local source assumptions.

Unreviewed message/conversation tables, functions, and policies must not be treated as safe.

## 22. Direct Data Access / RLS Reliance Map

| Data surface | Direct access observed | RPC-mediated access observed | RLS/policy evidence | Risk | Recommendation |
|---|---|---|---|---|---|
| `dm_conversations` | No direct runtime read confirmed in focused screens | Conversation list/create/archive RPCs | Local migration evidence only | Privacy-sensitive | Keep RPC-mediated |
| `dm_participants` | Mobile direct fallback read for other participant identity | Conversation create/list/read RPCs reference membership | Local migration evidence only | Privacy-sensitive | Verify RLS or wrap fallback in RPC |
| `dm_messages` | No direct runtime read/write confirmed in focused screens | Message list/send/delete RPCs | Local migration evidence only | Privacy-sensitive | Keep RPC-mediated |
| `user_profiles` | Dashboard direct identity read; mobile identity helpers resolve users | Profile/identity RPCs not confirmed for DM display | Production RLS not fully covered | Privacy-sensitive | Verify public/private identity field contract |
| `notifications_v2` | Not directly read in DM screen evidence | Notification RPCs from notification audit | RLS enabled at high level | Privacy-sensitive | Verify DM payload/privacy semantics |
| Social/block/friend tables | Not directly read in DM focused files | DM RPC error mapping indicates relationship checks | Production evidence incomplete | Privacy-sensitive | Verify block/friend/host-contact rules |
| Linked events/media/tickets | No structured DM attachment access confirmed | Linked resource RPCs elsewhere | Depends on each domain | Privacy/revenue-sensitive | Re-check canonical linked-resource authority |

## 23. Duplicated / Split / Legacy Messaging Surfaces

| Surface / helper / RPC / table | Observed role | Current / legacy / unknown | Risk if still active or authoritative | Evidence type | Recommendation |
|---|---|---|---|---|---|
| Mobile `dm.v1.ts` | Canonical-looking mobile DM wrapper | Current/plausible | Backend parity unknown | Local source | Preserve RPC pattern |
| Dashboard `dm.ts` | Host-persona dashboard DM wrapper | Current/plausible | Host persona authority drift | Local source | Verify host persona membership |
| `dm_get_or_create_conversation_v1` | Conversation creation and eligibility | Current/live | Exact live production body validated; later v1.5 host-contact behavior confirmed | `11a` production body export + static caller audit | Preserve contract; patch-ready only for anon EXECUTE containment |
| `dm_send_message_v1` | Message send and notification side effect | Current/live | Exact live production body validated; preview and notification side effects remain privacy-sensitive but no auth gap was proven | `11a` production body export + static caller audit | Preserve contract; patch-ready only for anon EXECUTE containment |
| `dm_get_messages_v1` | Message read/list | Current/live | Exact live production body validated; member-scoped read confirmed | `11a` production body export + static caller audit | Preserve contract; patch-ready only for anon EXECUTE containment |
| Direct `dm_participants` fallback | Header identity resolution | Current/plausible | Direct RLS dependency for participant info | Local source | Verify or move behind RPC later |
| `notifications_v1` / `notifications_v2` | Notification preview generations | Split | Message preview privacy drift | Prior notification audit | Reconcile preview contract |
| DM photo messages design | Future private media messages | Deferred | Mistaken as implemented contract | Local design doc | Keep explicitly non-runtime |
| Mobile migration DM source | DM schema/RPC provenance | Split-source | Future patch could target wrong tree | Local source-map evidence | Confirm target before implementation |

## 24. Messaging-Critical Invariants

- Message reads are scoped to conversation members.
- Message sends require authenticated sender authority.
- Conversation membership is backend/RPC/RLS-authoritative.
- Frontend route parameters and UI guards are not security controls.
- Read receipts and unread counts are member/persona-scoped.
- Notification previews do not leak private message content to non-members.
- Push payloads minimize private content and re-check authority through deep links.
- Blocks/mutes affect messaging and notifications according to product rules.
- Messaging identity display follows profile/persona public/private field contracts.
- Message links to private events, media, tickets, reservations, claims, venues, or profiles do not bypass canonical access checks.
- Support/admin visibility, if present, is internally gated and auditable.
- Public web/deep links never expose private conversation content.
- Database Functions / RPC evidence is separate from Edge Function deployment evidence.
- Production SQL/RPC evidence remains stronger than local source assumptions.

## 25. Unknown / Needs Verification Surfaces

- Production existence, RLS status, and policy correctness for `dm_conversations`, `dm_participants`, and `dm_messages`.
- Production body/grant parity for all DM V1 RPCs.
- Whether DM V1 functions are deployed from mobile migration history, hostos migration history, manual production edits, or another path.
- Exact relationship rule for personal DM initiation.
- Exact host-contact rule for host persona DM initiation.
- Mute behavior for list visibility versus notification suppression.
- Report/moderation model for abusive messages.
- Support/admin visibility model, if any.
- Notification preview field contract for message body and actor/persona identity.
- Deep-link membership re-check coverage.
- Future photo/media message contract and storage bucket authority.

## 26. Messaging / Direct Conversation Gaps / Risk Register Seeds

### MDC-GAP-001

- Domain: DM production authority evidence
- Current issue: DM tables and RPCs are well represented locally, but supplied production reports did not fully cover DM RLS/policy/function parity.
- Expected clean messaging/direct conversation contract: DM membership, reads, sends, unread counts, archive, and delete are production-verified backend/RPC/RLS-authoritative.
- Risk: Privacy-sensitive.
- Priority candidate: Candidate P1.
- Blocked by: Production policy/RPC review and source-path provenance confirmation.
- Recommended next action: Run a read-only production DM authority verification report when explicitly approved.

### MDC-GAP-002

- Domain: Direct participant fallback read
- Current issue: Mobile conversation header directly reads `dm_participants` for an edge-case identity fallback.
- Expected clean messaging/direct conversation contract: Participant identity is available only to authorized conversation members through verified RLS or RPC output.
- Risk: Privacy-sensitive.
- Priority candidate: Candidate P2.
- Blocked by: `dm_participants` RLS verification.
- Recommended next action: Verify policy semantics or consider adding participant identity to existing RPC output later.

### MDC-GAP-003

- Domain: Host persona messaging
- Current issue: Dashboard uses host persona by default, but host persona DM eligibility and target persona availability need canonical documentation.
- Expected clean messaging/direct conversation contract: Host persona messaging is explicitly allowed only through backend-verified host-contact/persona rules.
- Risk: Privacy-sensitive / Product correctness.
- Priority candidate: Candidate P2.
- Blocked by: Product decision on host contact semantics.
- Recommended next action: Document who can message a host persona and when.

### MDC-GAP-004

- Domain: Block/mute/report behavior
- Current issue: Block and relationship error signals exist, but full mute/report behavior for send/list/notify is not proven.
- Expected clean messaging/direct conversation contract: Blocks, mutes, reports, and notification suppression are backend-authoritative and product-defined.
- Risk: Privacy-sensitive / Security-sensitive.
- Priority candidate: Candidate P2.
- Blocked by: Social graph/block/mute production review and product decisions.
- Recommended next action: Include DM effects in abuse/reporting audit.

### MDC-GAP-005

- Domain: Notification previews
- Current issue: DM conversation list has previews and notification payloads may reference conversations, but private preview policy is not fully verified.
- Expected clean messaging/direct conversation contract: Notification and push previews expose only recipient-authorized and privacy-accepted fields.
- Risk: Privacy-sensitive.
- Priority candidate: Candidate P1.
- Blocked by: Notification payload review and settings/private-preview decision.
- Recommended next action: Verify DM notification payload construction.

### MDC-GAP-006

- Domain: Deep-link resource boundary
- Current issue: Conversation routes and future message links require membership/resource re-checks; coverage is not fully proven.
- Expected clean messaging/direct conversation contract: Conversation and linked resource access are independently re-authorized on open.
- Risk: Privacy-sensitive / Product correctness.
- Priority candidate: Candidate P2.
- Blocked by: Deep-link route inventory.
- Recommended next action: Audit DM notification/deep-link entry points.

### MDC-GAP-007

- Domain: Split-source DM provenance
- Current issue: DM migration provenance appears in mobile Supabase tree, while future accepted migration target is hostos.
- Expected clean messaging/direct conversation contract: Future DM patches name the accepted target path and account for mobile provenance.
- Risk: Product correctness.
- Priority candidate: Unknown / Needs verification.
- Blocked by: Source-of-truth and production provenance confirmation.
- Recommended next action: Do not implement DM backend changes until target path and production parity are confirmed.

### MDC-GAP-008

- Domain: Support/admin visibility
- Current issue: No support/admin DM viewer was confirmed, but future support tooling could require private message access.
- Expected clean messaging/direct conversation contract: Support/admin access is explicitly gated, auditable, and separate from ordinary user/host/staff access.
- Risk: Operational/admin-sensitive / Privacy-sensitive.
- Priority candidate: Unknown / Needs verification.
- Blocked by: Product decision on support tooling.
- Recommended next action: Cover in Ops / Admin / Support Tools Authority Audit.

## 27. Product Decisions Required

- Who can initiate a personal DM?
- Who can initiate a host persona DM?
- Is friendship required for personal DMs?
- Is host follower, event participant, ticket holder, or public profile visibility enough to contact a host?
- What exactly should block and mute do to send, list, read, unread count, notification, and push?
- Are message body previews allowed in in-app notifications and push payloads?
- Should archive be per persona/member only?
- Who can soft-delete a message, and what remains visible to the other participant?
- Will support/admin ever be allowed to view private messages?
- Are photo/media messages deferred, active, or out of scope?

## 28. Recommended Next Audits

1. Ops / Admin / Support Tools Authority Audit.
2. Diagnostics / Observability / Audit Log Contract Audit.
3. Abuse / Reporting / Moderation Contract Audit.

These follow because messaging intersects with private support visibility, auditability, block/mute/report behavior, and notification privacy.

## 29. Non-Goals

- This audit does not authorize code changes.
- This audit does not authorize dashboard, mobile, web, or Supabase tree changes.
- This audit does not create SQL, migrations, or implementation instructions.
- This audit does not connect to production.
- This audit does not claim production vulnerability.
- This audit does not claim feature removal is safe.
- This audit does not claim support/admin message visibility exists.
- This audit does not treat local DM migration source as canonical production truth.
- This audit does not treat local Edge Function source folders as deployed production Edge Functions.

## 30. Open Questions

- Are DM V1 tables and functions present in production exactly as local mobile migration history suggests?
- Which Supabase source tree should receive any future accepted DM-related backend change?
- Should the mobile direct `dm_participants` fallback remain direct RLS-based?
- What are the accepted public/private identity fields for DM participant display?
- Are host persona DMs allowed from any user, followers only, event participants only, ticket holders only, or another relationship?
- What privacy setting controls message preview content in notifications and push?
- Are message reports stored, reviewed, or actionable today?
- Should blocks hide existing conversations or only block new sends?
- Does mark-read behavior differ for personal and host persona conversations?
- Are message deep links covered by authenticated membership re-checks in every entry path?

## 31. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `07_Audits/MessagingDirectConversationContractAudit.md` was created/modified.

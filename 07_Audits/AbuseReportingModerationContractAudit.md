# Abuse / Reporting / Moderation Contract Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false

## 2. Purpose

This audit maps JoinFolk abuse, reporting, moderation, block, mute, report, and support-review surfaces. It separates observed current behavior from the desired abuse/moderation contract and keeps report submission, moderation decisions, owner controls, host authority, and ops/admin review as separate authority concepts.

This is not an implementation plan, patch plan, cleanup plan, or migration plan. No backend/RPC/RLS/storage/auth, Supabase, dashboard, mobile, web, or application code changes are authorized by this audit.

## 3. Audit Scope

In scope:

- Block/unblock and blocked-user listing.
- Mute and notification suppression evidence.
- Report submission and report review surfaces where found.
- Media moderation, owner/uploader hide/delete, and public highlight suppression.
- Comment and memory-wall moderation candidates.
- Messaging abuse, blocked DM behavior, and unresolved report flows.
- Profile/persona/user, event/venue, public/feed/search moderation boundaries.
- Host, staff, support, and ops/admin moderation authority.
- Auditability, evidence payload privacy, retention, redaction, appeal, and reversal questions.

Read-only source inspection covered targeted handbook docs and local sources under the allowed paths. Source inspection was intentionally targeted and did not attempt exhaustive enumeration of every component.

## 4. Abuse / Reporting / Moderation Contract Summary

Observed abuse and moderation evidence is uneven:

- Mobile block/unblock is RPC-mediated through `block_user_v1`, `unblock_user_v1`, and `get_my_blocked_users_v1`.
- The block wrapper comments indicate blocking removes follows, pending invites, and active memberships, while unblocking does not restore them. Production body/policy evidence still needs verification.
- Direct messages use RPC-mediated conversation/message functions and map backend `BLOCKED` and relationship-required failures into UI errors. No dedicated private-message report/review flow was confirmed in targeted DM files.
- Mobile event media has owner controls through `hide_owned_media_v1`, `unhide_owned_media_v1`, `delete_owned_media_v1`, and hidden flags such as `hidden_by_user` and `hidden_by_host`.
- Dashboard gallery moderation uses host media workflows and prior audits identified `host_moderate_media_v1` for feature, unfeature, hide, unhide, and delete behavior.
- Ops media draft tooling uses direct table CRUD on `ops_media_drafts` with RLS assumptions, not a dedicated moderation RPC in the observed dashboard path.
- Event memory comments and media comments exist in local migration/source evidence, including hidden/comment moderation concepts, but production policy/body completeness was not reviewed here.
- Dedicated report submission, report review, appeal, restore, ban, suspend, or support-review surfaces were not confirmed in targeted runtime paths.

Expected clean contract:

- Block, mute, and report are separate product concepts.
- Report submission does not grant moderation authority.
- Host moderation is event-scoped; ops/admin moderation is separate.
- Owner/uploader hide/delete is not moderator takedown.
- Staff/scanner/manager roles do not imply moderation unless explicitly backend-enforced.
- Private report evidence is support/ops/admin-gated, auditable, and payload-minimized.
- Moderation visibility changes are backend/RPC/RLS authoritative, not UI-only filters.

## 5. Abuse and Moderation Surface Inventory Matrix

| Surface / domain | Abuse/report/moderation action or visibility exposed | Access path observed | Expected authority owner | Scope | Production evidence status | Risk class | Recommendation |
|---|---|---|---|---|---|---|---|
| User block | Block target user | RPC-mediated mutation: `block_user_v1` | Backend/RPC/RLS/auth | Authenticated reporter/user owner | Local wrapper + migration evidence; production body/policy incomplete | Privacy-sensitive; security-sensitive | Document contract; verify RLS/policies |
| User unblock | Remove block without restoring relations | RPC-mediated mutation: `unblock_user_v1` | Backend/RPC/RLS/auth | Authenticated user owner | Local wrapper + migration evidence; production body/policy incomplete | Product correctness | Verify relationship side effects |
| Blocked-user list | Read my blocked users | RPC-mediated read: `get_my_blocked_users_v1` | Backend/RPC/RLS/auth | User owner | Local wrapper + migration evidence; production policy incomplete | Privacy-sensitive | Verify owner scope |
| Host followers with blocks | Follower list excludes blocked users | RPC-mediated read candidate | Backend/RPC/RLS/auth | Host owner | Local wrapper evidence; production incomplete | Privacy-sensitive; product correctness | Preserve; document effects |
| Mute | Notification/content suppression | Not confirmed beyond unrelated media/video mute terms | Backend/RPC/RLS/auth | User owner | Not covered | Unknown / Needs verification | Needs product decision |
| DM blocked behavior | Prevent or surface blocked DM attempts | RPC-mediated DM errors | Backend/RPC/RLS/auth | Conversation members | Local DM wrapper evidence; production incomplete | Privacy-sensitive; security-sensitive | Verify DM body behavior |
| DM report/moderation | Report private message or conversation | Not confirmed | Backend/RPC/RLS/auth + support review | Conversation member/support | Not covered | Privacy-sensitive | Unknown / Needs verification |
| Owner media hide | Uploader hides owned media | RPC-mediated mutation: `hide_owned_media_v1` | Backend/RPC/RLS/auth | Uploader/owner | Local wrapper/migration evidence; production incomplete | Privacy-sensitive | Preserve; distinguish from moderation |
| Owner media unhide | Uploader restores owned hidden media | RPC-mediated mutation: `unhide_owned_media_v1` | Backend/RPC/RLS/auth | Uploader/owner | Local wrapper evidence; production incomplete | Privacy-sensitive | Verify restore semantics |
| Owner media delete | Uploader deletes owned media | RPC-mediated mutation: `delete_owned_media_v1` plus storage cleanup attempt | Backend/RPC/RLS/storage/auth | Uploader/owner | Local wrapper/migration evidence; production incomplete | Privacy-sensitive; product correctness | Verify storage consistency |
| Host media moderation | Feature/unfeature/hide/unhide/delete event media | Host RPC candidate: `host_moderate_media_v1` | Host RPC + Backend/RPC/RLS/auth | Event host; staff/ops unknown | Prior media/staff evidence; production body incomplete | Privacy-sensitive; operational/admin-sensitive | Verify host scope and auditability |
| Dashboard gallery panel | Host gallery action UI | Hooks/RPC path; comments conflict with direct-table wording | Backend authority expected | Host | Local dashboard evidence only | Product correctness | Reconcile implementation reality |
| Ops media drafts | Ops media review/drafts | Direct table CRUD on `ops_media_drafts` | Backend/RPC/RLS/auth or ops RPC | Ops/admin | Local source/migration evidence; production policy not fully covered | Operational/admin-sensitive; privacy-sensitive | Verify RLS/policies; verify auditability |
| Event memory comments | Comment creation/read/hide candidates | RPC/table evidence in local migrations | Backend/RPC/RLS/auth | Participant/public/host unknown | Local-source-only evidence here | Privacy-sensitive; product correctness | Needs verification |
| Media comments | Media comment creation/read candidates | RPC evidence: `create_media_comment_v1` and comment reads | Backend/RPC/RLS/auth | Authenticated/participant unknown | Local-source-only evidence here | Privacy-sensitive | Needs verification |
| Report submission | User reports abuse/content | Not confirmed in targeted runtime paths | Backend/RPC/RLS/auth | Reporter/support | Not covered | Privacy-sensitive; compliance/audit-sensitive | Needs product decision |
| Report review/resolution | Support/admin resolves reports | Not confirmed | Ops/admin RPC + support process | Support/ops/admin | Not covered | Operational/admin-sensitive | Unknown / Needs verification |
| Public/feed/search suppression | Hide moderated content from public surfaces | Hidden flags and prior search/public audit dependencies | Backend/RPC/RLS/auth | Public/authenticated viewers | Production policy correctness incomplete | Privacy-sensitive | Verify visibility parity |
| Moderation audit logs | Trace moderation decisions | Not confirmed for most moderation actions | Backend/RPC/RLS/auth + audit process | Host/support/ops | Diagnostics audit flagged unresolved | Compliance/audit-sensitive | Verify auditability |

## 6. Role Vocabulary and Authority Boundary

- Reporter: user submitting a report. Reporting does not grant moderation decision authority.
- Reported user: user who is the subject of a report or block.
- Target owner: owner of a target resource such as event, venue, media, message, profile, or comment.
- Uploader: user who uploaded media. Uploader owner controls are not the same as moderator takedown.
- Host: event-scoped owner/operator. Host moderation must remain event-scoped.
- Staff: event-scoped operational helper. Staff role does not imply moderation unless explicitly defined.
- Scanner: check-in staff role. Scanner is not a moderation role by default.
- Manager: possible staff role. Manager authority is Unknown / Needs verification unless backend evidence defines it.
- Moderator: product role for content decisions. No standalone moderator role was confirmed in targeted source.
- Ops/admin: internal operational authority. Ops/admin moderation is distinct from host moderation.
- Support: possible review role. Support review visibility is not mutation authority.
- Public/authenticated user: product access state, not moderation authority.

Block, mute, and report are different concepts. Block may affect future interaction and visibility. Mute may suppress notifications without removing content access. Report submission creates review evidence, but does not itself remove content unless backend product rules say so.

## 7. Block / Unblock Assessment

Observed evidence:

- Mobile `block.v1.ts` wraps `block_user_v1`, `unblock_user_v1`, and `get_my_blocked_users_v1`.
- Wrapper comments state blocking removes follows, pending invites, and active memberships.
- Wrapper comments state unblocking does not restore follows or memberships.
- Host follower listing is expected to exclude blocked users.
- Prior Social Graph audit classified blocks as RPC-mediated but production evidence incomplete.

Contract assessment:

- Blocking should be authenticated and owner-scoped to the blocker.
- Block effects should be documented across messages, profile visibility, host contact, feed/search, notifications, and future interaction.
- Unblock behavior should explicitly document which relationships are not restored.
- Block state should not grant moderation authority over unrelated content.

Status: Product-critical and privacy-sensitive. Mostly RPC-mediated in observed mobile paths, but production body/policy completeness remains Unknown / Needs verification.

## 8. Mute / Notification Suppression Assessment

No dedicated user mute system was confirmed in targeted runtime source. Several unrelated UI/media mute terms exist, such as feed video audio mute, but those are UX controls, not social mute or abuse suppression.

Contract expectation:

- Mute must be distinct from block.
- Mute may suppress notifications, push, or list prominence without changing canonical content access unless product explicitly says otherwise.
- Notification suppression must not replace resource authorization checks.
- Push delivery and notification payload privacy must still respect notification settings and backend eligibility.

Status: Unknown / Needs verification. Linkage to the Notification / Push / Reminder audit remains unresolved.

## 9. Report Submission Assessment

Dedicated report submission UI, report tables, or report RPCs were not confirmed in the targeted runtime paths inspected for this audit.

Expected clean report submission contract:

- Reporters should be authenticated unless anonymous reporting is explicitly accepted by product/security.
- Report targets should use explicit `target_type` and `target_id` semantics.
- Report reason and evidence payloads should be minimized.
- Reported users should not automatically see report evidence or reporter identity.
- Report submission should not itself be a moderation decision unless a backend rule explicitly defines automatic action.

Potential target types needing product decision:

- User/profile/persona.
- Event or venue.
- Event media, venue media, public highlight, or relic.
- Comment or memory-wall entry.
- Direct message or conversation.
- Ticket/order/reservation only if abuse/support use cases require it.
- Notification or public share surface.

Status: Unknown / Needs verification.

## 10. Report Review / Resolution Assessment

No dedicated support/admin report review page or report-resolution RPC was confirmed in targeted source inspection.

Expected clean review contract:

- Review visibility is support/ops/admin-sensitive.
- Resolution transitions should be explicit, such as open, reviewing, resolved, rejected, restored, or escalated, if product accepts those states.
- Decision actions should be attributable and auditable.
- Reviewers should see only evidence fields required for safety review.
- Support read visibility must not imply mutation authority.

Status: Unknown / Needs verification. Do not infer support report visibility from ops route guards, host moderation, or diagnostic access.

## 11. Media Moderation Assessment

Media has the strongest observed moderation evidence.

Observed evidence:

- `host_moderate_media_v1` exists in local migration/source evidence and prior Media/Staff audits.
- Dashboard gallery code identifies itself as a host gallery moderation panel and exposes feature/delete actions through hooks.
- Prior Media audit stated dashboard moderation sends feature, unfeature, hide, unhide, and delete actions through `host_moderate_media_v1`.
- Mobile event-media helpers model `hidden_by_host` and `hidden_by_user`.
- Owner media controls call `hide_owned_media_v1`, `unhide_owned_media_v1`, and `delete_owned_media_v1`.
- `event_media` RLS was confirmed at a high level in prior production evidence, but policy correctness and host moderation body/grants were not fully reviewed here.

Contract assessment:

- Host moderation should be event-scoped.
- Ops/admin moderation should be distinct from host moderation.
- Owner/uploader hide/delete should not grant host or ops moderation authority.
- Storage object deletion and media record state must stay consistent.
- Public highlights and memory-wall visibility should exclude host-hidden or user-hidden content unless product explicitly accepts otherwise.

Status: Active product-critical path. RPC-mediated in the strongest observed paths, but production parity and auditability need verification.

## 12. Comment / Memory Wall Moderation Assessment

Local migration/source evidence indicates event memory comments, media comments, hidden comment fields, and comment RPCs such as `create_media_comment_v1` and `get_event_memory_comments_v1/v2`.

Observed or implied behavior:

- Event memory comments have local-source evidence of RLS and hidden/comment moderation concepts.
- Media comments appear to consider block relationships in local migration evidence.
- Public read behavior appears in some local migration comments, but production policy correctness is not established here.

Contract expectation:

- Author delete, host moderation, ops/admin moderation, and report workflow must be distinct.
- Comment visibility should inherit event/media visibility and moderation state.
- Comment report evidence should avoid storing unnecessary private context.
- Public memory-wall comments should not expose hidden or moderated content unless product explicitly allows it.

Status: Unknown / Needs verification for active production behavior and moderation authority.

## 13. Messaging / Private Conversation Abuse Assessment

Observed evidence:

- Mobile DM wrapper uses RPCs including `dm_get_or_create_conversation_v1`, `dm_send_message_v1`, `dm_get_conversations_v1`, `dm_get_messages_v1`, `dm_mark_read_v1`, `dm_get_unread_count_v1`, `dm_archive_conversation_v1`, and `dm_delete_message_v1`.
- DM error mapping includes `BLOCKED`, `FRIENDSHIP_REQUIRED`, `HOST_CONTACT_RELATION_REQUIRED`, `RATE_LIMITED`, `NOT_PARTICIPANT`, and `SELF_DM_NOT_ALLOWED`.
- The Messaging audit did not confirm a support/admin private conversation viewer.
- Report/moderation runtime surface was not confirmed in targeted DM files.

Contract expectation:

- Message abuse reporting, if added or found, should avoid logging full private message bodies unless explicitly accepted.
- Blocked states should be backend-enforced by DM RPCs, not UI-only.
- Message deep links must re-check conversation membership and target resource authority.
- Support/admin private conversation review must be explicit, gated, auditable, and payload-minimized.

Status: Messaging abuse/reporting is Unknown / Needs verification. Blocked DM behavior is a positive backend-authority signal but not proof of full abuse workflow.

## 14. Profile / Persona / User Abuse Assessment

Observed evidence:

- User block flows exist.
- Dedicated profile/persona report flows were not confirmed in targeted runtime paths.
- Profile/persona production RLS and policy evidence was not fully covered by supplied reports.
- Public identity and avatar exposure are governed by the Profile / Persona / Public Identity audit.

Contract expectation:

- Profile/persona abuse reports may include sensitive identity fields and should be payload-minimized.
- Public profile visibility does not imply report evidence visibility.
- Blocking a user should not automatically delete or suppress all public profile data unless product defines that behavior.
- Host/persona abuse handling should distinguish personal identity, host persona, and venue/business identity.

Status: Block exists; report/review for profile/persona abuse is Unknown / Needs verification.

## 15. Event / Venue / Public Surface Moderation Assessment

Dedicated event or venue abuse report/takedown flows were not confirmed in targeted runtime paths.

Known dependencies:

- Public event/share visibility depends on Public Web and Search/Discovery audits.
- Venue media and venue public context depend on Venue Buyer and Media audits.
- Feed/public/search suppression after moderation must be backend-authoritative.

Contract expectation:

- Event/venue takedown or public suppression should be ops/admin or authorized host/backend authority, not UI-only filtering.
- Moderated public surfaces should not leak private report evidence.
- Public share and deep-link behavior must re-check canonical visibility after moderation.

Status: Unknown / Needs verification.

## 16. Host / Staff / Ops Moderation Authority Map

| Role / scope | Observed moderation-related authority | Current classification | Boundary |
|---|---|---|---|
| Uploader/owner | Hide, unhide, delete owned media through owner RPC wrappers | Active owner control | Not host/ops moderation |
| Host | Dashboard gallery moderation and `host_moderate_media_v1` evidence | Active / production body incomplete | Event-scoped only |
| Staff | Staff role exists for operations/check-in; moderation not proven | Unknown / Needs verification | Staff is not moderation by default |
| Scanner | Check-in role | No moderation authority observed | Scanner is not moderation |
| Manager | Possible staff/ops concept | Unknown / Needs verification | Requires backend definition |
| Ops/admin | Ops media drafts, support/admin surfaces, transfer tools | Partial evidence | Separate from host authority |
| Support | Review visibility not confirmed | Unknown / Needs verification | Read visibility is not mutation authority |
| Public/authenticated user | Report/block submission candidates | Block confirmed; report unknown | Submission is not decision authority |

## 17. Owner / Uploader Controls vs Moderator Authority

Observed owner/uploader controls:

- Hide owned media.
- Unhide owned media.
- Delete owned media.
- Fetch own hidden media.

These controls should be treated as personal ownership controls, not moderation takedown. They may affect public/profile/feed visibility for the uploader's own content, but should not grant authority over other users' content.

Moderator authority differs:

- Host moderation controls content within an event scope.
- Ops/admin moderation can cover cross-event or platform-level review only if explicitly backend-gated.
- Support review visibility does not imply ability to mutate content state.

## 18. Notification / Push / Deep Link Abuse Boundary

Blocks, mutes, reports, and moderation decisions can affect notifications, push delivery, and deep-link handling.

Observed evidence:

- DM errors include blocked/relationship-required cases.
- Notification and push audits flagged payload privacy and settings/eligibility behavior.
- Local push dispatch source was not production deployment proof.

Contract expectation:

- Block/mute/report state should be considered by notification eligibility where product requires.
- Notification suppression does not automatically change content access.
- Push payloads should not include private report evidence, private message bodies, or hidden media details.
- Deep links must re-check canonical resource authority after moderation, block, or visibility changes.

Status: Unknown / Needs verification for complete cross-domain behavior.

## 19. Public Visibility / Feed / Search Suppression Boundary

Moderation must be enforced at the same authority level as public/feed/search visibility.

Observed evidence:

- Media functions and local migrations reference `hidden_by_host`, `hidden_by_user`, and hidden comment concepts.
- Search/Discovery and Public Web audits identified feed/detail/share parity as authority-sensitive.
- Public highlights and relics rely on media visibility filters.

Contract expectation:

- Hidden, rejected, deleted, or moderated content should not appear in public feed/search/share surfaces unless product explicitly allows it.
- Public suppression should be backend/RPC/RLS authoritative.
- Frontend filters are not security controls.
- Feed, public share, event detail, memory wall, and profile surfaces should agree on moderation status or document intentional differences.

Status: Product-critical and privacy-sensitive; active behavior is split and Needs verification.

## 20. Support / Ops Review Visibility Assessment

No dedicated report-review dashboard was confirmed.

Observed adjacent support/ops surfaces:

- Ops media draft management.
- Ops transfer tooling and audit display.
- Ops user/tier/event inspection pages from prior audits.
- Diagnostics/audit log questions from the Diagnostics audit.

Contract expectation:

- Support/admin report evidence access is privacy-sensitive and operational/admin-sensitive.
- Review visibility should be explicit, gated, auditable, and payload-minimized.
- Host moderation does not imply support review authority.
- Ops route guards alone do not establish backend authority.

Status: Unknown / Needs verification.

## 21. Audit Log / Traceability Assessment

Moderation decisions and report resolutions should be attributable where product requires.

Observed evidence:

- Diagnostics audit flagged media moderation logging and private message report logging as unresolved.
- Transfer audit has stronger evidence than moderation audit.
- Dedicated moderation audit logs were not confirmed in targeted runtime paths.

Expected clean audit contract:

- Audit records include actor, action, target, timestamp, reason/resolution, and non-secret details.
- Report submission and report resolution are distinct audit events.
- Owner hide/delete and moderator takedown are distinct audit events.
- Audit logs are traceability evidence, not permission checks.

Status: Unknown / Needs verification for most abuse/moderation actions.

## 22. Payload Privacy / Evidence Data Boundary

Sensitive evidence data includes:

- Message content.
- Media URLs, storage paths, and signed URLs.
- User, profile, persona, avatar, and public identity fields.
- Event, private venue, group, or invite-only context.
- Ticket, order, claim, and reservation identifiers.
- Device and location data.
- Reporter identity.
- Reported user identity.
- Freeform report reason text.
- Screenshots or attachments if supported later.

Reports, moderation logs, and support-review records should minimize private data and never include secrets. No secrets were inspected for this audit.

## 23. Retention / Redaction / Appeal Assessment

Retention, redaction, appeal, reversal, restore, unhide, and takedown review behavior was not confirmed for report/moderation systems.

Observed partial reversal behavior:

- Owner media unhide exists as a user-owned restore concept.
- Host media unhide is implied by `host_moderate_media_v1` from prior Media audit evidence.
- Report appeal or support reversal was not confirmed.

Contract expectation:

- User deletion and moderation retention may differ.
- Report evidence retention should be policy-defined.
- Appeal and reversal states should be explicit where product requires them.
- Redaction should preserve necessary audit traceability while minimizing private data.

Status: Unknown / Needs verification.

## 24. Backend RPC / RLS Authority Evidence Map

Prior handbook evidence only; no production connection was made.

- Social graph/block/mute table production evidence was not fully covered.
- `block_user_v1`, `unblock_user_v1`, and `get_my_blocked_users_v1` exist in local source/migration evidence and prior Social Graph audit evidence, but production body/policy completeness needs verification.
- `event_media` RLS was confirmed at a high level, but policy correctness varies.
- `host_moderate_media_v1` exists from prior media/staff evidence, but full production body/policy review remains incomplete.
- DM/conversation/message production evidence was not fully covered.
- Notifications_v2 and push_tokens_v1 RLS were confirmed at a high level, but payload/suppression correctness needs review.
- Profiles/user_profiles production RLS/policy evidence was not fully covered.
- Public web/share/feed visibility policy correctness depends on prior Public Web and Search/Discovery audits.
- `ops_media_drafts` has local source/migration evidence for ops RLS, but production policy completeness was not fully covered.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Production SQL/RPC evidence remains stronger than local source assumptions.

Unreviewed report/moderation tables and functions must not be treated as safe.

## 25. Direct Data Access / RLS Reliance Map

| Data surface | Direct/RPC access observed | Authority concern | Evidence status | Recommendation |
|---|---|---|---|---|
| Reports / abuse tables | Not confirmed | Report evidence visibility and review authority | Not covered | Unknown / Needs verification |
| Blocks | RPC wrappers observed | Owner scope and cross-domain effects | Local source/migration; production incomplete | Verify RLS/policies |
| Mutes | Not confirmed as social mute | Notification suppression semantics | Not covered | Needs product decision |
| Friendships/follows/host followers | Block side effects and follower exclusion | Social visibility after block | Local wrapper/prior audit | Verify side effects |
| `event_media` | RPC reads/mutations; hidden flags | Media privacy and moderation | RLS high-level evidence | Verify policy/body |
| `venue_media` | Public/venue media context | Venue/public moderation | Prior storage/media evidence | Needs verification |
| `event_memory_comments` | Local table/RPC evidence | Comment privacy and moderation | Local-source-only here | Verify active path |
| `photo_comments` / media comments | Local RPC/table evidence | Comment visibility and block filtering | Local-source-only here | Verify active path |
| Messages/conversations | RPC wrappers observed | Private abuse handling and report visibility | Production not fully covered | Needs verification |
| `notifications_v2` | Notification records | Suppression and payload privacy | RLS high-level evidence | Verify payload policy |
| `push_tokens_v1` | Push token records | Suppression and token privacy | RLS high-level evidence | Verify owner scope |
| `profiles` / `user_profiles` | Identity surfaces | Profile abuse/report privacy | Production not fully covered | Verify policies |
| Events/venues | Public moderation/takedown not confirmed | Feed/share/search suppression | Prior audits only | Needs verification |
| Audit logs | Transfer audit stronger; moderation logs unknown | Traceability and private evidence | Incomplete | Verify auditability |
| `ops_media_drafts` | Direct CRUD with RLS assumptions | Ops-only review and moderation traceability | Local source/migration evidence | Verify production policy |

## 26. Duplicated / Split / Legacy Abuse-Moderation Surfaces

| Surface / helper / RPC / table | Observed role | Current / legacy / unknown | Risk if still active or authoritative | Evidence type | Recommendation |
|---|---|---|---|---|---|
| `block_user_v1` / `unblock_user_v1` / `get_my_blocked_users_v1` | User block system | Current | Cross-domain block effects may be undocumented | Mobile wrapper + migration evidence | Document block contract |
| Social mute vs feed-video mute | Different meanings of mute | Split terminology | UX mute could be confused with abuse mute | Local source | Keep concepts separate |
| `hidden_by_user` vs `hidden_by_host` | Owner hide versus host moderation | Current | Owner controls may be mistaken for moderator authority | Mobile/media evidence | Document boundaries |
| `host_moderate_media_v1` vs dashboard gallery comments | Host media moderation | Split/possibly stale comments | Direct-table wording can obscure active authority path | Dashboard + prior audit | Reconcile evidence |
| `ops_media_drafts` direct CRUD | Ops media workflow | Current / unknown | RLS-reliant ops edits may lack audit trail | Dashboard source + migration evidence | Verify RLS and auditability |
| Event memory comments v1/v2 | Comment read/write/moderation candidates | Split / unknown | Public/authenticated comment visibility may differ by function | Local migration evidence | Verify active production path |
| DM blocked errors vs report flow | Abuse prevention but no report flow confirmed | Current / incomplete | Blocked sending may be mistaken for full abuse system | Mobile wrapper | Needs report-flow decision |
| Public highlights/media visibility filters | Public suppression | Split across media/public/search | Moderated content could diverge across surfaces if filters differ | Prior audits + local evidence | Reconcile visibility parity |

## 27. Abuse-Moderation-Critical Invariants

- Report submission is not moderation decision authority.
- Block, mute, and report are separate product concepts.
- Host moderation is event-scoped.
- Staff/scanner authority does not imply moderation.
- Owner/uploader controls are not ops/admin moderation.
- Private report evidence is not public or user-visible by default.
- Moderation visibility changes are backend-authoritative.
- Notification suppression does not replace content access checks.
- Deep links re-check canonical resource authority after moderation.
- Support/admin report review is gated, auditable, and payload-minimized.
- Moderation logs are traceability evidence, not permission checks.
- Public/feed/search surfaces do not show moderated content unless product explicitly allows it.
- Local Edge Function source is not deployment evidence.

## 28. Unknown / Needs Verification Surfaces

- Whether a formal report submission product exists.
- Whether report target types are accepted and documented.
- Whether report review/resolution UI or RPCs exist outside inspected paths.
- Whether support/admin report readers exist and are audited.
- Whether social mute exists separately from block.
- Whether block effects cover notifications, public profile, host contact, event visibility, and feed/search.
- Whether private-message reports exist.
- Whether comment moderation is active in production.
- Whether host media moderation production body and grants match desired authority.
- Whether moderation actions have audit logs.
- Whether report evidence has retention, redaction, and appeal rules.
- Whether public/feed/search suppression is consistent across all surfaces.

## 29. Abuse / Reporting / Moderation Gaps / Risk Register Seeds

| Gap ID | Domain | Current issue | Expected clean abuse/reporting/moderation contract | Risk | Priority candidate | Blocked by | Recommended next action |
|---|---|---|---|---|---|---|---|
| ARM-GAP-001 | Report system | Formal report submission/review surfaces were not confirmed | Report targets, submission, review, and resolution are documented and backend-authoritative | Privacy-sensitive; compliance/audit-sensitive | Candidate P2 | Product decision and source verification | Decide accepted report target model |
| ARM-GAP-002 | Block effects | Block RPCs exist, but full cross-domain effects are not verified | Block effects across DM, follows, host contact, profile, notifications, and discovery are explicit | Privacy-sensitive; product correctness | Candidate P1 | Production body/policy review | Verify block RPC behavior and document effects |
| ARM-GAP-003 | Mute semantics | Social mute was not confirmed separately from block | Mute suppresses notifications/visibility only as accepted, without changing content authority unless defined | Product correctness; privacy-sensitive | Unknown | Product decision | Define whether social mute exists |
| ARM-GAP-004 | Media moderation | `host_moderate_media_v1` evidence exists, but production body/grants/auditability are incomplete | Host media moderation is event-scoped, backend-enforced, and auditable | Privacy-sensitive; operational/admin-sensitive | Candidate P1 | Production function review | Verify host moderation authority and logs |
| ARM-GAP-005 | Owner vs moderator controls | `hidden_by_user` and `hidden_by_host` coexist without a fully documented contract here | Owner hide/delete and moderator takedown are separate state machines | Product correctness; privacy-sensitive | Candidate P2 | Media contract decision | Document media moderation state model |
| ARM-GAP-006 | Comments/memory wall | Comment moderation/report behavior is local-source-only in this audit | Comment author, host, ops/admin, and report workflows are explicit | Privacy-sensitive | Candidate P2 | Active production path verification | Verify comment moderation functions and policies |
| ARM-GAP-007 | DM abuse reporting | DM blocked errors exist, but report/review flow was not confirmed | Private-message abuse reporting is membership-scoped, private, and support-gated | Privacy-sensitive | Unknown | Messaging product decision | Verify or define DM report workflow |
| ARM-GAP-008 | Ops media drafts | Direct `ops_media_drafts` CRUD relies on RLS and auditability is unclear | Ops media review/edit actions are ops-gated and auditable | Operational/admin-sensitive | Candidate P2 | Production policy and audit review | Verify ops media draft RLS/audit contract |
| ARM-GAP-009 | Public suppression parity | Moderation flags may need consistent public/feed/search/share suppression | Moderated content visibility is backend-authoritative and parity-tested across surfaces | Privacy-sensitive; product correctness | Candidate P1 | Public/search/media authority review | Reconcile moderation visibility rules |
| ARM-GAP-010 | Retention/appeal | Retention, redaction, appeal, restore, and reversal rules are not confirmed | Report and moderation evidence lifecycle is documented | Compliance/audit-sensitive | Candidate P2 | Trust/safety policy decision | Run privacy/data retention and trust-safety policy audit |

## 30. Product Decisions Required

- Does JoinFolk have a formal report system, or only block and moderation controls?
- Which report target types are accepted?
- Are anonymous reports allowed?
- Does social mute exist separately from block?
- What exact product effects should block have?
- Who may review report evidence?
- Who may resolve reports?
- Can hosts moderate comments, or only media?
- Can staff or managers moderate anything?
- What distinction should exist between owner hide, host hide, delete, takedown, reject, restore, and appeal?
- Are moderation decisions visible to the reported user?
- What report/moderation data must be retained or redacted?
- What audit trail is required for host, support, and ops moderation?

## 31. Recommended Next Audits

1. Payments / Refunds / Disputes Operations Audit.
2. Privacy / Data Retention / Deletion Contract Audit.
3. Legal / Trust & Safety Policy Mapping Audit.

These follow from unresolved moderation evidence retention, safety review, revenue-sensitive support actions, and policy-level abuse / trust-and-safety requirements.

## 32. Non-Goals

- This audit does not authorize backend, RLS, RPC, storage, auth, Edge Function, or Supabase changes.
- This audit does not create migrations or database statements.
- This audit does not verify production directly.
- This audit does not claim a production vulnerability without production evidence.
- This audit does not claim moderation/reporting is unsafe solely because it exists.
- This audit does not claim RLS is correct solely because it is enabled.
- This audit does not claim support/admin report visibility exists unless evidence supports it.
- This audit does not claim local Edge Function source is active production moderation/reporting.

## 33. Open Questions

- Is there an accepted report table or report RPC family?
- Which app surfaces can submit reports?
- Are report reasons freeform, enum-based, or both?
- Are screenshots or attachments supported?
- Is reporter identity visible to support only, ops/admin only, or ever to the reported user?
- Does blocking suppress notifications, messages, profile visibility, follows, host followers, or all future contact?
- Does unblocking restore anything?
- Does social mute exist?
- Are event memory comments public, authenticated-only, participant-only, or host-moderated?
- Are media moderation decisions logged?
- Can ops/admin restore host-hidden or user-hidden media?
- How should moderated content behave in public web, feed, search, public profile, and notification previews?

## 34. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `07_Audits/AbuseReportingModerationContractAudit.md` was created/modified.

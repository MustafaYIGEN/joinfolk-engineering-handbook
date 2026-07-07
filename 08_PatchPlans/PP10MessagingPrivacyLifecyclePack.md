# PP-10 Messaging / Direct Conversation Privacy Lifecycle Pack

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook audit synthesis only
- canonical: false
- Execution status: Not executed
- Legal status: Engineering planning only; not legal advice
- Messaging status: Not executed / Messaging behavior not verified
- Privacy lifecycle status: Not executed / Deletion and export behavior not verified

## 2. Purpose

This is a decision-pack for defining JoinFolk messaging and direct conversation privacy lifecycle semantics before implementation work begins.

It is not messaging implementation, not message deletion execution, not message inspection, not production verification, not legal advice, and not patch authorization.

## 3. Evidence Boundary

This document is based only on handbook audits, the Release Readiness / Production Hardening Gap Register, PP-01, PP-02, PP-03, PP-04, PP-05, PP-06, PP-07, PP-08, and PP-09.

No source-code inspection, production connection, Supabase CLI, SQL, builds, tests, messaging verification, private message inspection, message deletion, message export, realtime verification, attachment inspection, legal review, policy-copy modification, or final copy drafting was performed.

## 4. PP-10 Scope Summary

PP-10 covers:

- Messaging surface inventory.
- Conversation and message data domain inventory.
- Messaging lifecycle taxonomy.
- Conversation creation and participant authority model.
- Message send, edit, and delete model.
- Local hide, archive, and conversation deletion model.
- Read, unread, delivery, and presence model.
- Block and mute messaging model.
- Notification, private preview, and deep-link model.
- Attachment, media, and signed URL model.
- Report, safety, and evidence model.
- Support/admin DM access model.
- Account deletion and data request messaging model.
- Export, redaction, and retention model.
- Public/private visibility interactions.
- Commerce, event, reservation, claim, and support conversation dependencies where applicable.
- Realtime, delivery, and sync model.
- Diagnostics, audit, and delivery log model.
- Security and access expectation model.
- Backend/RPC/RLS/realtime/storage verification dependencies.
- PP-02 policy, PP-03 deletion, PP-04 commerce, PP-05 visibility, PP-06 notification, PP-07 moderation, PP-08 support/admin, and PP-09 media dependencies.
- Final patch-plan closure for the current release hardening sequence.

PP-10 does not execute PP-01 and does not authorize messaging changes, RLS changes, RPC changes, realtime changes, message deletion changes, notification changes, report/moderation changes, support/admin access changes, storage/media changes, copy changes, database changes, or production changes.

## 5. Source Register Coverage

| Release gap | Why PP-10 covers it | PP-10 limitation |
|---|---|---|
| RR-GAP-003 | Direct message, participant, read state, notification, and attachment tables may rely on RLS and direct-access policies. | PP-10 does not verify production policies. |
| RR-GAP-004 | Conversation participant, sender, recipient, blocked user, support/admin, and host/persona authority must remain distinct. | PP-10 defines decisions, not implementation. |
| RR-GAP-010 | DM notifications, snippets, private preview, deep links, delivery logs, and push payloads affect message privacy. | PP-06 owns notification delivery verification. |
| RR-GAP-011 | Diagnostics and delivery logs may include message refs, channel refs, or private context. | PP-10 does not inspect diagnostics payloads. |
| RR-GAP-014 | Block, mute, follow, group, profile, and social visibility effects on messaging remain unresolved. | PP-07/PP-05 own broader safety and visibility decisions. |
| RR-GAP-017 | DM membership, delete/archive semantics, notification previews, reports, deep links, and support visibility are core PP-10 scope. | PP-10 does not confirm messaging exists in production. |
| RR-GAP-018 | Support/admin access to DM evidence or metadata requires authority and auditability. | PP-08 owns support/admin role detail. |
| RR-GAP-019 | DM report, moderation, evidence retention, and safety review require workflow decisions. | PP-07 owns abuse/moderation process detail. |
| RR-GAP-020 | Account deletion, data export, retention, redaction, and support evidence policies affect messages. | PP-10 does not provide legal advice. |
| RR-GAP-021 | Messaging/privacy/support/report/deletion copy must not overpromise behavior. | PP-02 owns public/legal copy review. |
| RR-GAP-022 | Commerce, ticket, reservation, refund, dispute, or support conversations may create revenue/audit retention constraints. | Applies only if commerce messaging exists. |
| RR-GAP-023 | Messaging decisions must be accepted before patches. | PP-10 recommends decision records only. |

## 6. Messaging Privacy Lifecycle Problem Statement

Messaging lifecycle is not one delete button. It crosses:

- Conversation.
- Participant.
- Sender.
- Recipient.
- Message row.
- Message body.
- Attachment/media object.
- Unread/read state.
- Delivery state.
- Realtime channel.
- Push notification.
- Private preview.
- Deep link.
- Block.
- Mute.
- Report.
- Safety evidence.
- Support/admin access.
- Account deletion.
- Data export.
- Retention.
- Redaction.
- Audit log.
- Diagnostics.
- Legal/privacy copy.

Message deletion, local hide/archive, redaction, retention, export, and support evidence are separate decisions.

## 7. Messaging Surface Inventory

| Surface | Example user expectation | Data/function/storage domains affected | Owner | PP-01 evidence dependency | PP-02 copy dependency | PP-03 deletion dependency | PP-06 notification dependency | PP-07 moderation dependency | PP-08 support/admin dependency | PP-09 media dependency | Current status | Recommended PP-10 decision need |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Direct conversation list | Only conversations the user belongs to appear. | Conversations, participants, unread state | Messaging / Backend | Tables, RLS, RPCs | Messaging privacy copy | Account deletion effects | Unread badge behavior | Block/mute effects | Support visibility | None unless attachments | Unknown / Needs verification | Define list authority and lifecycle. |
| Direct conversation detail | Only participants can read messages. | Messages, participants, profiles | Messaging / Security | Message RLS and fetch behavior | Privacy/support copy | Retention/redaction | Deep-link reauthorization | Report message workflow | Evidence access audit | Attachment handling | Unknown / Needs verification | Define participant read authority. |
| Message send | Sender can message only accepted recipients. | Send RPC/functions, participants | Messaging / Backend | Send authority and grants | User expectations | Sender deletion | Send notifications | Abuse controls | Support correction not default | Attachment upload if any | Unknown / Needs verification | Define send authority. |
| Message edit if applicable | Edits are transparent or not supported. | Message body, edit metadata | Product / Messaging | Edit functions if present | Copy constraints | Export/redaction | Notification updates | Evidence snapshots | Audit visibility | Attachment edits | Unknown / Needs verification | Decide whether edit exists. |
| Message delete | Deletion meaning is clear. | Message row/body, markers | Messaging / Privacy | Delete behavior and policies | Deletion copy | Account/data request model | Notification stale state | Evidence retention | Support access | Attachment deletion | Unknown / Needs verification | Define local, sender, recipient, and full deletion. |
| Conversation archive/hide | Archive/hide is local unless explicitly accepted otherwise. | Archive/hide state | Messaging / Product | Archive/hide functions | User copy | Export/deletion | Badge behavior | Block/mute relationship | Support view limits | None known | Unknown / Needs verification | Separate UI state from backend deletion. |
| Read/unread state | Read state is accurate and private. | Read receipts, unread counts | Messaging / Privacy | Read-state policies | Privacy copy | Export/deletion | Badge and notifications | Block/mute effects | Diagnostics access | None known | Unknown / Needs verification | Decide read receipt visibility and retention. |
| Typing/presence if applicable | Presence is not exposed beyond accepted audience. | Realtime, presence state | Messaging / Privacy | Realtime authorization | Privacy copy | Retention | Notification none or minimal | Block/mute effects | Diagnostics logs | None known | Unknown / Needs verification | Decide whether presence exists and who sees it. |
| DM notification | Push/in-app notifications do not leak private content. | Notifications, push payloads | Notification / Privacy | Delivery behavior | Notification copy | Token deletion | Private preview | Report/safety notifications | Delivery log access | Preview media if any | Unknown / Needs verification | Define snippet and payload rules. |
| Message deep link | Opening a DM link re-checks access. | Deep links, conversation routing | Mobile/Web / Security | Route/backend checks | Copy constraints | Deleted user fallback | Reauthorization | Block/report state | Support links | Attachment links | Unknown / Needs verification | Define reauthorization model. |
| Block user from DM | Block prevents accepted future interaction. | Block RPCs/tables, conversations | Safety / Social | Block behavior | Block copy | Retention | Notification suppression | Safety model | Support override | None known | Unknown / Needs verification | Define block effects on history and future DMs. |
| Mute conversation/user | Mute controls notifications or visibility as accepted. | Mute state, notifications | Messaging / Social | Mute evidence | Mute copy | Retention | Delivery suppression | Safety model | Support none or limited | None known | Unknown / Needs verification | Define mute semantics. |
| Report message | Users can report accepted message surfaces. | Reports, evidence refs | Trust/Safety | Report tables/functions | Report policy copy | Evidence retention | Report notifications | Review workflow | Evidence access audit | Evidence media if any | Unknown / Needs verification | Define report/evidence contract. |
| DM attachment/media | Attachments follow private media rules. | Storage, signed URLs, refs | Media / Messaging | Storage policies and refs | Media/privacy copy | Attachment deletion/export | Preview behavior | Evidence retention | Support access | Full PP-09 dependency | Unknown / Needs verification | Define media lifecycle. |
| Support/admin DM evidence review | Privileged review is limited and audited. | Message bodies/metadata, audit logs | Support / Security | Access paths and audit effects | Support copy | Retention exceptions | Notifications none or case-based | Evidence workflow | Least privilege | Attachments/evidence | Unknown / Needs verification | Define support/admin access boundary. |
| Account deletion effect on DMs | Deletion effects are predictable. | Message bodies, attribution, refs | Privacy / Product | Production behavior | Deletion copy | Core PP-03 dependency | Stale notifications | Evidence retention | Support logs | Attachments | Unknown / Needs verification | Define delete/redact/retain model. |
| Data export for DMs | Export handles third-party content safely. | Messages, metadata, attachments | Privacy / Legal | Schema and access evidence | Export copy | Core PP-03 dependency | Notification refs | Report evidence exclusions | Support logs | Attachment export | Unknown / Needs verification | Define export scope. |
| Realtime delivery channel | Realtime access is participant-authorized. | Realtime channels, policies | Backend / Security | Channel authorization | Privacy/security copy | Retention logs | Delivery sync | Block effects | Diagnostics access | None known | Unknown / Needs verification | Define realtime authority. |
| Diagnostics/delivery logs | Logs do not expose private message content unnecessarily. | Logs, diagnostics, delivery refs | Observability / Privacy | Payload evidence | Diagnostics disclosure | Retention/export | Delivery logs | Report context | Admin access audit | Media refs if any | Unknown / Needs verification | Define minimization and retention. |

## 8. Conversation / Message Data Domain Inventory

| Data/domain | Example data | Sensitivity | User expectation | Current status | Decision needed |
|---|---|---|---|---|---|
| Conversations | Conversation id, type, last activity | Privacy-sensitive | Only participants see conversation metadata. | Unknown / Needs verification | Define membership and metadata access. |
| Conversation participants | User/persona refs, membership state | Privacy-sensitive | Participant membership is authoritative. | Unknown / Needs verification | Define add/remove/read authority. |
| Message rows | Message id, conversation id | Privacy-sensitive | Only authorized parties can access. | Unknown / Needs verification | Define row lifecycle. |
| Message body | Text content | Highly sensitive | Private to accepted participants except accepted safety/support paths. | Unknown / Needs verification | Define access, deletion, export, evidence. |
| Sender/recipient refs | User/profile/persona ids | Privacy-sensitive | Attribution is accurate or redacted. | Unknown / Needs verification | Define de-attribution and retention. |
| Timestamps | Sent/edited/read times | Privacy-sensitive | Time data is scoped to participants. | Unknown / Needs verification | Define export and retention. |
| Read/unread state | Read receipts, badges | Privacy-sensitive | Behavior is predictable and private. | Unknown / Needs verification | Define visibility and lifecycle. |
| Delivery state | Sent/delivered/failed | Privacy-sensitive | Delivery status is scoped. | Unknown / Needs verification | Define logs and export. |
| Typing/presence if any | Active/typing indicators | Privacy-sensitive | Not exposed beyond accepted audience. | Unknown / Needs verification | Decide if supported. |
| Attachments/media refs | Object paths, signed URLs | Highly sensitive | Access follows message authority. | Unknown / Needs verification | Link PP-09 storage lifecycle. |
| Deleted/redacted markers | Tombstones, redaction flags | Privacy-sensitive | Deletion meaning is clear. | Unknown / Needs verification | Define marker semantics. |
| Archive/hide state | Local hidden/archive flags | Privacy-sensitive | Local action does not surprise other party. | Unknown / Needs verification | Define local-only versus shared state. |
| Block/mute state | Blocked/muted user or thread | Safety-sensitive | Effects are clear. | Unknown / Needs verification | Define interaction and notification effects. |
| Report refs | Report ids, evidence pointers | Trust/safety-sensitive | Reporter privacy and evidence retention are protected. | Unknown / Needs verification | Link PP-07. |
| Support/admin notes | Case notes, review markers | Highly sensitive | Access is limited and audited. | Unknown / Needs verification | Link PP-08. |
| Notification refs | Push/in-app notification ids | Privacy-sensitive | Snippets do not leak content. | Unknown / Needs verification | Link PP-06. |
| Diagnostics/realtime logs | Channel ids, errors, message refs | Privacy-sensitive | Logs are minimized and retained intentionally. | Unknown / Needs verification | Define payload and retention. |

## 9. Messaging State / Lifecycle Taxonomy

- Conversation active: conversation is visible and usable by accepted participants.
- Conversation hidden locally: one participant hides the thread from their own UI.
- Conversation archived locally: one participant archives the thread without deleting messages for others.
- Conversation deleted locally: local conversation reference is removed or hidden without accepted full deletion.
- Message sent: sender submitted a message accepted by backend authority.
- Message delivered: recipient device or inbox delivery state if supported.
- Message read: read state exists if supported.
- Message edited: message body or metadata changed if supported.
- Message deleted for sender: sender no longer sees message under accepted semantics.
- Message deleted for recipient: recipient no longer sees message under accepted semantics.
- Message deleted for all if supported: message is removed or redacted for all participants if accepted.
- Message redacted: body or identifying fields are replaced or removed while retaining a record.
- Message retained as evidence: content or metadata is preserved for safety/support/audit reasons.
- Message exportable: data is included in an accepted data export model.
- Message non-exportable retained log: retained log excluded or redacted under accepted policy.
- Participant active: member has current access.
- Participant removed if applicable: member no longer has accepted access.
- Blocked: interaction is prevented or limited by accepted block rules.
- Muted: notifications or visibility are reduced by accepted mute rules.
- Reported: message/conversation is linked to a safety report.
- Under review: safety/support review is pending.
- Support/admin accessed: privileged access occurred under accepted audit model.
- Attachment active: attachment is available under message authority.
- Attachment suppressed: attachment is hidden, removed, or retained only as evidence.

Do not collapse local UI hide with backend deletion.

## 10. Conversation Creation / Participant Authority Decision Model

PP-10 must define:

- Who can start a direct conversation.
- Whether conversations are one-to-one only.
- Whether event, host, participant, ticket, reservation, support, or group conversations exist.
- Whether persona scope affects display only or also authority.
- How blocked/muted relationships affect new conversation creation.
- How profile/persona visibility affects participant display.
- How participant membership is created, retained, removed, or restored.

Decision required: accepted conversation creation and participant authority model.

## 11. Message Send / Edit / Delete Decision Model

PP-10 must map:

- Send authority.
- Edit support if any.
- Delete own message.
- Delete for self.
- Delete for both/all if supported.
- Redaction marker or tombstone behavior.
- Attachment deletion link.
- Recipient visibility after deletion.
- Evidence retention after report or support review.

Decision required: exact message mutation and retention model.

## 12. Local Hide / Archive / Conversation Deletion Decision Model

PP-10 must distinguish:

- Archive conversation.
- Hide conversation.
- Delete local conversation.
- Delete conversation for both if supported.
- Unread state reset.
- Restored conversation.
- New incoming message behavior.
- Effects on other participant.

Decision required: local UI state versus backend message lifecycle.

## 13. Read Receipt / Unread / Presence Decision Model

PP-10 must map:

- Unread count.
- Read receipts.
- Delivered status.
- Typing status if any.
- Online/presence if any.
- Privacy settings if any.
- Exportability and retention of these states.

Decision required: which presence/read signals are shown, retained, exportable, and user-controllable.

## 14. Block / Mute / Social Safety Messaging Decision Model

PP-10 must map:

- Block sender.
- Unblock sender.
- Mute sender or conversation.
- Unmute.
- Existing conversation history.
- Future messages.
- DM notifications.
- Profile visibility.
- Event/group shared context.
- Report and support review interactions.

Decision required: block/mute effects across messaging, history, notifications, discovery, and future contact.

## 15. Notification / Private Preview / Deep Link Decision Model

PP-10 must map:

- DM push notification.
- Message snippet.
- Sender name/avatar.
- Private preview setting.
- Deep link to message or conversation.
- Deleted message state.
- Blocked state.
- Reported or moderated state.

Decision required: snippet masking, payload minimization, and reauthorization rules.

## 16. Attachment / Media / Signed URL Decision Model

PP-10 must map, if attachments exist:

- Message attachments.
- Image, video, audio, or file classes.
- Signed URLs.
- Public/private buckets.
- Attachment deletion.
- Attachment export.
- Report evidence media.
- Support/admin attachment access.

Decision required: message media lifecycle and PP-09 dependency.

## 17. Report / Safety / Evidence Decision Model

PP-10 must map:

- Report message.
- Report conversation.
- Evidence snapshot or pointer.
- Reporter privacy.
- Reported user privacy.
- Evidence retention.
- Takedown/redaction.
- Appeal/restoration if applicable.

Decision required: DM safety evidence contract and PP-07 dependency.

## 18. Support / Ops / Admin DM Access Decision Model

PP-10 must decide:

- Whether support/admin can view message bodies.
- Whether support/admin can view metadata only.
- Whether a case/evidence requirement is mandatory.
- Whether approval/escalation is required.
- Whether access is read-only.
- What audit log is required.
- Which roles have least-privilege access.

Decision required: support/admin DM access boundary and auditability.

## 19. Account Deletion / Data Request Messaging Decision Model

PP-10 must map:

- Deleted sender.
- Deleted recipient.
- Both users deleted.
- Message body retention.
- Sender attribution redaction.
- Attachments.
- Reports/evidence.
- Support records.
- Conversation membership.
- Notification and diagnostics references.

Decision required: deletion, redaction, de-attribution, retention, and export model.

## 20. Export / Redaction / Retention Decision Model

PP-10 must map:

- Message body.
- Message metadata.
- Attachments.
- Read receipts.
- Conversation membership.
- Support/admin access logs.
- Report evidence.
- Third-party message content.
- Delivery and diagnostics logs.

Decision required: what is exportable, redacted, retained, or excluded.

## 21. Public / Private Visibility Interaction Decision Model

PP-10 must map:

- Public profile removed.
- Private profile.
- Blocked profile.
- Event/host profile context.
- Deleted user fallback.
- Deep links from outside DM.
- Attachment/media visibility.

Decision required: messaging privacy must not leak public-suppressed profile, event, venue, or media data.

## 22. Commerce / Event / Reservation Messaging Decision Model

PP-10 must map only if such messaging exists or is later confirmed:

- Host-buyer conversation.
- Reservation support message.
- Ticket/order support message.
- Claim/transfer support message.
- Commerce dispute evidence.
- Event cancellation or refund context.

Decision required: commerce-related messaging retention and privacy boundaries.

## 23. Realtime / Delivery / Sync Decision Model

PP-10 must map:

- Realtime channel authorization.
- Message delivery state.
- Offline sync.
- Device sync.
- Pagination/history fetch.
- Participant removal effects.
- Block effects.
- Message deletion/redaction propagation.

Decision required: backend-authoritative realtime and fetch authority.

## 24. Diagnostics / Audit / Delivery Logs Decision Model

PP-10 must map:

- Delivery logs.
- Realtime logs.
- Notification logs.
- App diagnostics with message references.
- Error logs containing message ids or snippets.
- Support/admin access logs.
- Report/evidence access logs.

Decision required: payload minimization, private-data boundaries, and audit retention.

## 25. Encryption / Security / Access Expectation Decision Model

PP-10 must define which security and privacy claims are allowed based on evidence:

- Server-side access expectations.
- Stored message access boundaries.
- Support/admin access boundaries.
- Backup/log retention expectations.
- Whether any encryption claim is supported by evidence.
- Whether any end-to-end encryption claim is supported by evidence.

Current status: Unknown / Needs verification. PP-10 does not make encryption, ephemeral-message, both-sided deletion, or exportability claims.

## 26. Backend / RPC / RLS / Realtime / Storage Verification Dependencies

PP-01 must provide production evidence for:

- Conversations tables if present.
- Conversation participants tables if present.
- Messages tables if present.
- Message metadata/read state tables if present.
- Archive/hide/delete state tables if present.
- Block/mute tables/RPCs if present.
- Report/message evidence tables if present.
- Message attachment/storage references if present.
- RLS policies for conversations/messages/participants.
- RPCs/functions for sending, deleting, archiving, marking read, or reporting messages.
- Realtime channel authorization.
- Grants, search path, and `SECURITY DEFINER` posture where relevant.
- Notification functions for DMs.
- Diagnostics/audit/delivery logs.
- Support/admin DM access paths.

No executable SQL, messaging commands, realtime commands, storage commands, or implementation steps are authorized by this pack.

## 27. Messaging / Conversation Data Domain Inventory Matrix

| Domain | Example data | User expectation | Lifecycle decision needed | Legal/product/security review need | PP-01 evidence need | PP-03/PP-06/PP-07/PP-08/PP-09 dependency | Later dependency |
|---|---|---|---|---|---|---|---|
| Conversations | Thread id, last message ref | Only participants see threads. | Creation, hide/archive, deletion | Product/security/privacy | Table and RLS evidence | Deletion, notifications, support | Final patch-plan pack / none currently defined |
| Conversation participants | User/persona membership | Membership controls access. | Add/remove/retain behavior | Product/security | RLS and RPC evidence | Deletion, block/mute, support | Final patch-plan pack / none currently defined |
| Messages | Message rows | Only authorized participants read. | Send/delete/redact/retain | Product/security/privacy | Table, RLS, function evidence | Deletion, report, support | Final patch-plan pack / none currently defined |
| Message bodies | Text content | Private except accepted support/safety paths. | Access, deletion, export | Legal/privacy/security | Body access policies | Report evidence and support audit | Final patch-plan pack / none currently defined |
| Message metadata | Timestamps, sender refs | Metadata remains scoped. | Retention/export/redaction | Privacy/security | Schema and policy evidence | Deletion and diagnostics | Final patch-plan pack / none currently defined |
| Read/unread state | Read receipts, badges | Privacy behavior is clear. | Visibility and retention | Product/privacy | Read state evidence | Notifications and export | Final patch-plan pack / none currently defined |
| Delivery/realtime state | Delivered/sync/channel refs | Delivery is authorized. | Channel and log lifecycle | Security/privacy | Realtime evidence | Diagnostics and support | Final patch-plan pack / none currently defined |
| Block/mute state | User/thread mute, block | Effects are predictable. | History, future sends, notifications | Product/trust-safety | Tables/RPC evidence | Notification and abuse dependencies | Final patch-plan pack / none currently defined |
| Archive/hide state | Local archive flags | Local actions do not delete for others unless accepted. | Local/shared state | Product/privacy | Table/function evidence | Deletion/export | Final patch-plan pack / none currently defined |
| Report/evidence refs | Report ids, evidence pointers | Evidence is protected. | Retention/redaction/export | Legal/trust-safety/security | Report tables/functions | PP-07 and PP-08 | Final patch-plan pack / none currently defined |
| Message attachments/media | Object refs, signed URLs | Attachments follow message authority. | Storage lifecycle | Privacy/security/media | Storage refs and policies | PP-09 | Final patch-plan pack / none currently defined |
| Notification/deep-link refs | Push ids, route refs | Push/deep link does not leak content. | Snippet and reauthorization | Privacy/security | Delivery evidence | PP-06 | Final patch-plan pack / none currently defined |
| Diagnostics/realtime logs | Error logs, channel ids | Logs are minimized. | Payload and retention | Privacy/security | Diagnostics evidence | PP-06 and PP-08 | Final patch-plan pack / none currently defined |
| Support/admin access logs | Case ids, actor ids | Privileged access is audited. | Audit retention/export | Legal/privacy/security | Audit evidence | PP-08 | Final patch-plan pack / none currently defined |

## 28. Policy-to-Messaging Mismatch Register

| Copy/policy signal | Missing messaging lifecycle decision | Risk | Owner | Recommended next action |
|---|---|---|---|---|
| Delete message/account deletion | Backend retention, redaction, and both-sided deletion are unknown. | Candidate P1 | Product / Privacy | Decide message deletion and account deletion effects after PP-01. |
| DM privacy copy | Support/admin access to message bodies or metadata is unknown. | Candidate P1 | Legal / Security / Support | Define access boundary and auditability. |
| Private preview copy | DM notification snippets and push payloads are unknown. | Candidate P1 | Notification / Privacy | Link PP-06 and define snippet rules. |
| Block/mute copy | DM side effects are unknown. | Candidate P2 | Product / Trust-Safety | Define block/mute interaction matrix. |
| Report message copy | Evidence retention and review process are unknown. | Candidate P1 | Trust/Safety / Legal | Link PP-07 report workflow. |
| Data export copy | Third-party message content handling is unknown. | Candidate P2 | Privacy / Legal | Decide export/redaction model. |
| Attachment deletion | Storage object retention is unknown. | Candidate P2 | Media / Privacy | Link PP-09 attachment lifecycle. |
| Security/encryption copy | Evidence for encryption claims is unknown. | Candidate P1 | Security / Legal | Do not publish claims without evidence and review. |

## 29. Implementation-without-Messaging-Contract Register

| Existing technical/product surface | Missing messaging privacy lifecycle contract | Risk | Owner | Recommended next action |
|---|---|---|---|---|
| Conversation routes | Participant authority and deletion behavior. | Candidate P1 | Messaging / Backend | Verify and decide conversation authority. |
| Message table/RPCs if referenced in audits | Send/list/delete/archive semantics. | Candidate P1 | Backend / Security | Verify function bodies, grants, and policies. |
| Realtime channels | Channel authorization and sync behavior. | Candidate P1 | Backend / Security | Verify realtime authority. |
| DM notifications | Snippet, private preview, and deep-link rules. | Candidate P1 | Notification / Privacy | Create DM notification decision. |
| Block/mute surfaces | Effects across messaging and notifications. | Candidate P2 | Product / Trust-Safety | Decide safety interaction model. |
| Report message surfaces | Evidence and support review model. | Candidate P1 | Trust/Safety | Define report workflow. |
| Support/admin evidence views | Private-data access and auditability. | Candidate P1 | Support / Security | Link PP-08 authority model. |
| Message attachments/media | Storage lifecycle and signed URL behavior. | Candidate P2 | Media / Storage | Link PP-09. |
| Account deletion/data export pathway | Message retention, redaction, and third-party content. | Candidate P1 | Privacy / Legal | Link PP-03 decision. |

## 30. PP-01 Evidence Dependencies

PP-10 needs PP-01 to provide:

- Production messaging tables and schemas.
- Message RLS policies.
- Conversation participant RLS.
- RPC bodies, grants, search path, and security mode.
- Realtime channel authorization.
- Message notification delivery behavior.
- Message deletion/archive/hide behavior.
- Block/mute production behavior.
- Report/evidence production behavior.
- Support/admin DM access paths.
- Attachment/media storage behavior.
- Diagnostics/audit/log behavior.

Until PP-01 evidence exists, production messaging behavior remains Unknown / Needs verification.

## 31. PP-02 Policy Copy Dependencies

PP-10 is constrained by PP-02 because:

- Messaging privacy copy must match actual access and retention.
- Deletion copy must not overpromise message removal.
- Notification preview copy must match payload behavior.
- Block/mute copy must match actual side effects.
- Report/support copy must match evidence workflow.
- No final messaging/privacy/legal copy is approved by PP-10.

## 32. PP-03 Deletion / Data Request Dependencies

PP-10 is constrained by PP-03 because:

- Account deletion must define sender/recipient message behavior.
- Data export must handle third-party conversation content carefully.
- Redaction and de-attribution must be explicit.
- Support/admin logs and report evidence may require retention exceptions.
- Attachments/media require PP-09 alignment.

## 33. PP-04 Commerce / Refund / Payment Dependencies

PP-10 depends on PP-04 only where commerce messaging exists:

- Ticket/order/reservation/support messages may require commerce retention.
- Refund/dispute evidence may include message content or references.
- Commerce support access must be audited.
- Commerce deep links must re-check entitlement and state.

## 34. PP-05 Public Visibility Dependencies

PP-10 is constrained by PP-05 because:

- Public profile/event/media suppression must not leak through DM deep links.
- Deleted, private, or blocked users need safe fallback.
- Public persona/profile fields in DM must use public-safe rules.
- DM privacy is not public visibility and must be scoped separately.

## 35. PP-06 Notification / Diagnostics Dependencies

PP-10 is constrained by PP-06 because:

- DM notification snippets must respect private preview.
- Push payloads must not include sensitive message content unless accepted.
- Deep links must reauthorize.
- Diagnostics and delivery logs with message references are privacy-sensitive.

## 36. PP-07 Abuse / Moderation Dependencies

PP-10 is constrained by PP-07 because:

- Message reports require evidence workflow.
- Block/mute effects must be accepted.
- Support review of reported messages requires least privilege.
- Report/moderation notifications must not leak message evidence.

## 37. PP-08 Ops / Admin Support Dependencies

PP-10 is constrained by PP-08 because:

- Support/admin DM access must be audited.
- Private-data access requires reason code and case/evidence reference.
- Break-glass DM access requires strict process.
- Support notes and access logs need retention decisions.

## 38. PP-09 Media / Storage Dependencies

PP-10 is constrained by PP-09 because:

- Message attachments/media require storage lifecycle decisions.
- Signed URL and public/private bucket behavior must match PP-09.
- Attachment deletion is not automatically storage object deletion.
- Report evidence media may require retention exceptions.

## 39. Product Decision Dependency Checklist

- Messaging surface model.
- Conversation participant authority model.
- Message send/edit/delete model.
- Local hide/archive model.
- Read/unread/presence model.
- Block/mute DM effects.
- Notification/private preview model.
- Attachment/media model.
- Report/evidence model.
- Support/admin DM access model.
- Account deletion messaging model.
- Export/redaction/retention model.
- Realtime/delivery/sync model.
- Security/access expectation model.
- Beta versus public launch scope.

## 40. Legal / Privacy / Security Review Dependency Checklist

- Messaging privacy wording.
- Message deletion/account deletion promises.
- Third-party conversation data export.
- Support/admin message access.
- Report/evidence retention.
- Notification snippet privacy.
- Block/mute user expectations.
- Attachment/media retention.
- Security/encryption claims.
- Minors/guardian issues if applicable.

This checklist is for review routing only and is not legal advice.

## 41. Risk Priority Matrix

| Risk class | Candidate items | Notes |
|---|---|---|
| Candidate P0 | None unless prior evidence supports it. | PP-10 does not assert a P0 production vulnerability. |
| Candidate P1 | Message access without verified RLS/participant authority; deletion promise versus backend retention mismatch; support/admin DM body access without audit; DM notification snippet leakage; report evidence retention/deletion mismatch; realtime channel authorization unknown. | Requires PP-01 evidence and product/legal/security decisions. |
| Candidate P2 | Block/mute side-effect ambiguity; archive/hide/delete confusion; read receipt/presence privacy; attachment storage lifecycle; data export third-party content handling; commerce message retention. | Likely beta hardening unless launch scope makes it earlier. |
| Candidate P3 | Copy polish and documentation after decisions. | Not a substitute for verification. |
| Unknown / Needs verification | Production messaging tables, RLS, RPCs, realtime channels, deletion behavior, report evidence, support/admin access, attachments, diagnostics, and export behavior. | Use this when evidence is incomplete. |

## 42. Recommended Decision Records

- MessagingPrivacyLifecycleDecision
- ConversationParticipantAuthorityDecision
- MessageDeletionRetentionDecision
- ConversationArchiveHideDecision
- BlockMuteMessagingEffectsDecision
- DMNotificationPrivatePreviewDecision
- MessageAttachmentMediaDecision
- DMReportEvidenceRetentionDecision
- SupportAdminDMAccessDecision
- MessagingExportRedactionDecision
- RealtimeMessagingAuthorityDecision

## 43. Dependency Map to Later Patch Plan Groups

PP-10 is the final recommended patch-plan group in the current release hardening sequence.

Outputs that require future work should become decision records, verification reports, or implementation readiness checklists, not new patch-plan groups unless the release register is amended.

PP-10 depends on PP-01, PP-02, PP-03, PP-04 where commerce messaging exists, PP-05, PP-06, PP-07, PP-08, and PP-09.

## 44. PP-10 Output Artifacts

Recommended documents after PP-10 execution, not created by this pack:

- `MessagingPrivacyLifecycleDecision.md`
- `ConversationParticipantAuthorityDecision.md`
- `MessageDeletionRetentionDecision.md`
- `ConversationArchiveHideDecision.md`
- `BlockMuteMessagingEffectsDecision.md`
- `DMNotificationPrivatePreviewDecision.md`
- `MessageAttachmentMediaDecision.md`
- `DMReportEvidenceRetentionDecision.md`
- `SupportAdminDMAccessDecision.md`
- `MessagingExportRedactionDecision.md`
- `RealtimeMessagingAuthorityDecision.md`
- `MessagingPrivacyImplementationReadinessChecklist.md`

## 45. Execution Preconditions

Before executing PP-10:

- Product owner assigned.
- Legal/privacy owner assigned.
- Security owner assigned.
- Messaging owner assigned.
- Backend/realtime owner assigned.
- Notification owner assigned where DM notifications exist.
- Media/storage owner assigned where attachments/media exist.
- Trust/safety owner assigned where reporting exists.
- Support/admin owner assigned where support access exists.
- Commerce owner assigned where commerce messaging exists.
- PP-01 production evidence available where needed.
- PP-02 copy constraints available.
- PP-03 deletion/data-request constraints available.
- PP-04 commerce constraints available where commerce messaging exists.
- PP-05 public visibility constraints available.
- PP-06 notification/diagnostics constraints available.
- PP-07 abuse/moderation constraints available.
- PP-08 support/admin constraints available.
- PP-09 media/storage constraints available.
- Launch scope defined.
- No production changes.
- No private message inspection.
- No message deletion/export.
- No SQL/RLS/RPC/realtime/storage changes.
- No final legal claims.
- Sanitized evidence rules accepted.

## 46. Explicitly Blocked Actions

- No message sending.
- No message reading/private inspection.
- No message deletion/archive/hide/redaction.
- No conversation mutation.
- No support/admin DM access.
- No report/moderation action.
- No attachment/media inspection.
- No signed URL generation execution.
- No data export.
- No production access.
- No SQL/Supabase CLI.
- No migrations.
- No source code changes.
- No RLS/RPC/realtime/storage changes.
- No notification behavior changes.
- No policy publication.
- No legal advice.
- No compliance claim.
- No launch readiness claim.
- No final messaging/privacy/deletion/export/security copy.
- No immediate patch authorization.
- No source-code re-audit.

## 47. Unknown / Needs Verification Items

- Whether production messaging tables exist.
- Which conversation/message surfaces are deployed.
- Which roles or personas can start conversations.
- Which users can read each conversation.
- Whether message edit exists.
- What message delete does.
- Whether delete is local-only, sender-only, recipient-only, both-sided, or redaction.
- Whether archive/hide exists and whether it is local-only.
- Whether read receipts, delivery state, typing, or presence exist.
- What block does to existing and future DMs.
- What mute does to DM notifications and visibility.
- Whether DM snippets are sent in push payloads.
- Whether deep links reauthorize conversation access.
- Whether message attachments/media are supported.
- Whether support/admin can view message bodies or metadata.
- How message reports are stored and reviewed.
- What happens to DMs after account deletion.
- Whether messages or attachments are exportable.
- How third-party message content is handled in data export.
- Whether realtime channels are backend-authorized.
- What security or encryption claims are evidence-backed.

## 48. Acceptance Criteria for PP-10 Completion

PP-10 is complete only when:

- Messaging surface inventory is confirmed.
- Conversation/message data domain inventory is confirmed.
- Messaging lifecycle taxonomy is accepted or explicitly deferred.
- Conversation creation/participant authority model is accepted or explicitly deferred.
- Message send/edit/delete model is accepted or explicitly deferred.
- Local hide/archive/conversation deletion model is accepted or explicitly deferred.
- Read/unread/presence model is accepted or explicitly deferred.
- Block/mute messaging model is accepted or explicitly deferred.
- Notification/private preview/deep-link model is accepted or explicitly deferred.
- Attachment/media/signed URL model is accepted or explicitly deferred.
- Report/safety/evidence model is accepted or explicitly deferred.
- Support/ops/admin DM access model is accepted or explicitly deferred.
- Account deletion/data request messaging model is accepted or explicitly deferred.
- Export/redaction/retention model is accepted or explicitly deferred.
- Public/private visibility interaction model is accepted or explicitly deferred.
- Commerce/event/reservation messaging model is accepted or explicitly deferred or explicitly out of scope.
- Realtime/delivery/sync model is accepted or explicitly deferred.
- Diagnostics/audit/delivery logs model is accepted or explicitly deferred.
- Security/access expectation model is accepted or explicitly deferred.
- PP-01 evidence dependencies are linked.
- PP-02 copy constraints are linked.
- PP-03 deletion constraints are linked.
- PP-04 commerce constraints are linked or explicitly out of scope.
- PP-05 visibility constraints are linked.
- PP-06 notification/diagnostics constraints are linked.
- PP-07 abuse/moderation constraints are linked.
- PP-08 ops/admin/support constraints are linked.
- PP-09 media/storage constraints are linked.
- Product owner decisions are assigned.
- Legal/privacy/security review dependencies are assigned.
- Messaging owner is assigned.
- Backend/realtime owner is assigned.
- Notification owner is assigned where DM notifications exist.
- Media/storage owner is assigned where attachments/media exist.
- Trust/safety owner is assigned where reporting exists.
- Support/admin owner is assigned where support access exists.
- Commerce owner is assigned where commerce messaging exists.
- Final hardening sequence status is updated or explicitly marked unchanged.
- No final legal/messaging/privacy/deletion/export/security text is treated as approved unless the responsible owner confirms it.

## 49. Recommended Follow-Up Reports

Recommended reports after PP-10 execution, not created now:

- `MessagingPrivacyLifecycleDecision.md`
- `ConversationParticipantAuthorityDecision.md`
- `MessageDeletionRetentionDecision.md`
- `ConversationArchiveHideDecision.md`
- `BlockMuteMessagingEffectsDecision.md`
- `DMNotificationPrivatePreviewDecision.md`
- `MessageAttachmentMediaDecision.md`
- `DMReportEvidenceRetentionDecision.md`
- `SupportAdminDMAccessDecision.md`
- `MessagingExportRedactionDecision.md`
- `RealtimeMessagingAuthorityDecision.md`
- `MessagingRpcRlsRealtimeVerificationReport.md`
- `MessagingPrivacyImplementationReadinessChecklist.md`

## 50. Non-Goals

- No code changes.
- No SQL/migrations.
- No production execution.
- No message sending.
- No message reading/private inspection.
- No message deletion/archive/hide/redaction.
- No conversation mutation.
- No support/admin DM access.
- No report/moderation action.
- No attachment/media inspection.
- No signed URL generation execution.
- No data export.
- No RLS/RPC/realtime/storage changes.
- No notification behavior changes.
- No legal advice.
- No compliance claim.
- No launch readiness claim.
- No final messaging/privacy/deletion/export/security copy.
- No immediate patch authorization.
- No source-code re-audit.

## 51. Open Questions

- Do production messaging tables exist?
- Which conversation/message surfaces are shipped?
- Who can start a direct conversation?
- Who can read a conversation?
- What does message delete do?
- Is delete local-only or both-sided?
- Does archive/hide exist?
- Are read receipts or presence shown?
- What does block do to existing and future DMs?
- What does mute do to DM notifications?
- Are DM snippets sent in push payloads?
- Do deep links reauthorize conversation access?
- Are message attachments/media supported?
- Can support/admin view message bodies?
- How are message reports stored and reviewed?
- What happens to DMs after account deletion?
- Are messages exportable?
- How are third-party message contents handled in data export?
- Are realtime channels backend-authorized?
- What security/encryption claims are allowed?
- What is beta launch scope for messaging privacy lifecycle?

## 52. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No messaging/conversation/private-message/deletion/archive/hide/redaction/export/report/moderation/support-admin/notification/realtime/RLS/RPC/storage action was executed.
- No files were staged or committed.
- Only `08_PatchPlans/PP10MessagingPrivacyLifecyclePack.md` was created/modified.

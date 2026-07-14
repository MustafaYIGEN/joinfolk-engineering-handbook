# PP-03 Account Deletion / Data Request Decision Pack

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook audit synthesis only
- canonical: false
- Execution status: Not executed
- Legal status: Engineering planning only; not legal advice

## 2. Purpose

This is a decision-pack for defining JoinFolk account deletion, data request, export, retention, redaction, suppression, and support-process semantics before implementation work begins.

This pack is not legal advice, not final policy copy, not production verification, and not patch authorization. It does not authorize code changes, database changes, storage changes, deletion execution, export execution, or policy publication.

## 3. Evidence Boundary

This document is based only on handbook audits, the Release Readiness / Production Hardening Gap Register, PP-01, and PP-02.

No source-code inspection, production connection, Supabase CLI, SQL, builds, tests, legal review, deletion execution, data export execution, or final copy drafting was performed. Policy text is treated as product/legal promise evidence, not implementation proof. Engineering evidence is not legal compliance proof.

## 4. PP-03 Scope Summary

PP-03 covers:

- Account deletion model options.
- Data request and export model options.
- Retention, redaction, deletion, archival, suppression, and export vocabulary.
- Domain-by-domain deletion and retention decision needs.
- Product, legal, support, and ops/admin dependencies.
- PP-01 evidence dependencies for production data surfaces.
- PP-02 copy constraints around settings deletion, support-mediated deletion, 30-day removal, exceptions, and data export.
- Dependency mapping to PP-04 through PP-10.

PP-03 does not execute PP-01, does not create final policy copy, and does not modify app, web, mobile, dashboard, Supabase, storage, auth, or production systems.

## 5. Source Register Coverage

| Release gap | Why PP-03 covers it | PP-03 limitation |
|---|---|---|
| RR-GAP-010 | Notification preference, private preview, push token deletion, and reminder retention affect account deletion. | PP-03 defines decision needs; PP-06 verifies and designs notification/diagnostics privacy. |
| RR-GAP-011 | Diagnostics payloads, user linkage, support visibility, retention, and redaction need deletion-request decisions. | PP-03 does not verify `app_diagnostics` production policy. |
| RR-GAP-012 | Media deletion, storage objects, public URL invalidation, and content-license policy need deletion decisions. | PP-03 does not verify buckets or object lifecycle. |
| RR-GAP-013 | Profile, persona, public identity, avatar exposure, and host identity transfer retention are core deletion concerns. | PP-03 defines decision model; PP-01 verifies production schema/policies. |
| RR-GAP-017 | Messaging delete/archive/report/support visibility affects deletion and export expectations. | PP-03 does not define final DM implementation semantics. |
| RR-GAP-018 | Support/admin privacy request processing and auditability are required for support-mediated deletion/export. | PP-03 defines process decisions; PP-08 owns support/admin authority. |
| RR-GAP-019 | Report/moderation evidence retention and redaction affect deletion exceptions. | PP-03 flags retention decisions; PP-07 owns moderation workflow. |
| RR-GAP-020 | Account deletion, data export, retention, redaction, storage deletion, diagnostics, commerce, and report evidence are unresolved. | PP-03 is the primary decision pack, not implementation. |
| RR-GAP-021 | Legal/public copy promises constrain deletion and data request decisions. | PP-03 depends on PP-02 and legal review. |
| RR-GAP-022 | Commerce/payment/ticket/order retention and provider payload policy may override ordinary product deletion. | PP-03 identifies retention decisions; PP-04 owns commerce/refund/payment contract. |
| RR-GAP-023 | Multiple product decisions must be accepted before patches. | PP-03 recommends decision records but does not finalize them. |

## 6. Account Deletion Decision Problem

Account deletion is not one operation. It affects multiple concepts with different owners, risks, retention expectations, and public visibility behavior:

- Auth account and login identifiers.
- Profile and user profile records.
- Personal persona and host persona.
- Public profile fields.
- Avatars, media, and storage objects.
- Events and venues.
- Tickets, orders, reservations, claims, transfers, and check-in proof.
- Messages and conversations.
- Notifications, reminders, push tokens, and notification settings.
- Diagnostics and telemetry.
- Reports, moderation evidence, and safety records.
- Audit/admin/support logs.
- Social graph, groups, blocks, mutes, follows, and visibility edges.
- Support requests.
- Legal, payment, revenue, and safety records.

The decision must separate user-facing deletion from backend deletion, redaction, retention, archival, public suppression, and support/admin auditability.

## 7. Data Request Surface Inventory

| Request type | Example user expectation | Data domains affected | Owner | PP-01 evidence dependency | PP-02 copy dependency | Current status | Recommended PP-03 decision need |
|---|---|---|---|---|---|---|---|
| Delete account | Remove my account and personal data. | Auth, profiles, personas, media, messages, notifications, commerce, logs. | Product + legal + support | Auth/profile/table/storage evidence | Settings deletion, support deletion, 30-day copy | Unknown / Needs verification | Choose deletion model and domain exceptions. |
| Delete profile data | Remove name, bio, avatar, public identity. | Profiles, user_profiles, avatars, search/feed. | Product + legal | Profile and public route evidence | Privacy/profile visibility copy | Unknown / Needs verification | Decide delete vs redact vs suppress. |
| Delete host persona | Remove host identity. | Host persona, events, venues, transfers, audit logs. | Product + legal + ops | Profile/persona/transfer evidence | Host identity and retention copy | Unknown / Needs verification | Decide whether host attribution can be removed. |
| Delete uploaded media | Remove my photos/videos. | Event media, venue media, storage objects, public URLs. | Product + legal + media | Bucket/object and DB row evidence | Media deletion/public content copy | Unknown / Needs verification | Decide DB row vs object deletion and URL caveats. |
| Delete event participation history | Remove attendance/participation traces. | Tickets, reservations, check-in, event attendees, notifications. | Product + legal + commerce | Ticket/reservation/check-in evidence | Privacy and ticket copy | Unknown / Needs verification | Decide what is retained for event/revenue/safety. |
| Delete messages | Remove private messages. | Conversations, messages, reads, notifications, reports. | Product + legal | DM schema/RPC evidence | DM privacy/delete copy | Unknown / Needs verification | Decide requester-only vs both-side vs redaction semantics. |
| Delete notifications | Remove notification history. | Notifications, reminders, unread state. | Product | Notification table evidence | Notification settings copy | Unknown / Needs verification | Decide history deletion vs reference redaction. |
| Delete push token/device data | Stop device delivery and remove token. | Push tokens, device metadata, settings. | Product + privacy | Push token evidence | Push/privacy copy | Unknown / Needs verification | Decide immediate token deletion and retention exceptions. |
| Delete diagnostics data | Remove telemetry linked to me. | `app_diagnostics`, crash/analytics, logs. | Product + legal + support | Diagnostics payload/linkage evidence | Diagnostics disclosure copy | Unknown / Needs verification | Decide retention/redaction for low-trust telemetry. |
| Delete payment/ticket history | Remove purchases and tickets. | Orders, payment attempts, tickets, claims, receipts, provider refs. | Legal + commerce | Commerce/provider evidence | Payment/retention copy | Unknown / Needs verification | Decide legal/revenue retention exceptions. |
| Delete report/moderation evidence | Remove reports involving me. | Reports, moderation logs, evidence, appeals. | Trust/safety + legal | Report/moderation evidence | Safety/report copy | Unknown / Needs verification | Decide safety retention and redaction rules. |
| Export my data | Give me a copy of my data. | Profile, media metadata, tickets, messages, notifications, diagnostics, support. | Product + legal + support | Domain inventories and access evidence | Data export copy | Not confirmed | Decide support-mediated, self-service, or deferred export. |
| Correct/update my data | Fix inaccurate profile/account data. | Profile, persona, contact, support record. | Product + support | Profile/update evidence | Privacy/support copy | Unknown / Needs verification | Define correction request scope. |
| Suppress public visibility | Hide me or my content from public surfaces. | Profile, media, events, feed/search/share. | Product + legal | Public route/storage evidence | Public visibility copy | Unknown / Needs verification | Decide suppression vs deletion behavior. |
| Revoke public media/profile visibility | Stop public display of specific media/profile. | Storage, public URLs, profile/media records. | Product + media | Bucket/URL evidence | Media/profile copy | Unknown / Needs verification | Decide public URL limitations and cache caveats. |
| Support privacy request | Ask support to process privacy request. | Support queue, identity verification, audit logs. | Support + legal + ops | Support/admin authority evidence | Support-mediated copy | Unknown / Needs verification | Define intake, verification, audit, and escalation. |

## 8. Current Policy Promise Inventory

| Policy/copy signal | Source audit signal | Engineering evidence status | Risk | Decision needed |
|---|---|---|---|---|
| Account deletion through settings | Mobile terms reportedly mention app settings deletion. | Account deletion implementation Unknown / Needs verification. | Privacy-sensitive | Decide self-service, support-mediated, both, or deferred. |
| Support-mediated deletion | Mobile privacy reportedly mentions deletion through support. | Support authority/process not confirmed. | Privacy-sensitive, operational/admin-sensitive | Define support intake, authorization, audit, and execution model. |
| 30-day removal | Mobile privacy reportedly mentions 30-day removal with exceptions. | Per-domain retention/deletion not verified. | Legal/compliance-sensitive | Decide whether promise is accepted and how exceptions are expressed. |
| Legal/security/business exceptions | Mobile privacy references exceptions. | Domain exceptions not mapped. | Legal/compliance-sensitive | Define exception categories with legal review. |
| Data export absence | Export/portability not confirmed. | Not confirmed. | Privacy-sensitive | Decide whether export is supported, support-mediated, or deferred. |
| Diagnostics/analytics disclosure | Analytics/crash may be described as anonymous; diagnostics may be user-linked. | `app_diagnostics` linkage/payload incomplete. | Privacy-sensitive | Decide diagnostics disclosure and deletion/redaction model. |
| Media deletion/public URL behavior | Media deletion/storage behavior unresolved. | DB row vs object vs public URL evidence incomplete. | Privacy-sensitive | Decide storage deletion, metadata redaction, and public URL caveats. |
| DM deletion/archive ambiguity | DM delete/archive semantics unresolved. | Production DM structural evidence is now complete, but exact body authorization, caller-body parity, and retention semantics remain unresolved. | Privacy-sensitive | Decide message deletion and retention semantics. |
| Commerce/ticket/payment retention | Commerce retention unresolved. | Provider/order/ticket retention incomplete. | Revenue-sensitive, compliance/audit-sensitive | Decide retention exceptions and redaction model. |
| Report/moderation retention | Report/moderation workflow and evidence retention unresolved. | Formal system not confirmed. | Trust/safety-sensitive | Decide safety retention, redaction, appeal, and disclosure boundaries. |

## 9. Deletion Model Options

| Option | Pros | Risks | Required evidence | Legal/product decision need | Implementation complexity | Copy impact |
|---|---|---|---|---|---|---|
| Option A: support-mediated deletion only | Controlled review, easier exception handling, support can verify identity. | Manual burden, slower user experience, support authority/audit gaps. | Support/admin authority, audit logs, domain retention matrix. | High | Medium | Copy must say request is processed by support, not automatic deletion. |
| Option B: self-service request intake + support execution | User can initiate in app while preserving manual review. | Users may expect immediate deletion; support SLA must be defined. | Request intake, support queue, auditability, status tracking. | High | Medium | Copy must distinguish request submission from completion. |
| Option C: self-service automated deletion with retained exceptions | Strong user experience and deterministic behavior. | High risk without complete domain policy, storage, commerce, safety, and audit decisions. | Full PP-01 evidence, accepted retention model, implementation design. | Very high | High | Copy can be stronger only after implementation evidence exists. |
| Option D: staged deletion / deactivate first / hard-delete later | Allows recovery, fraud/safety review, and delayed retention handling. | May conflict with immediate deletion expectations if copy is vague. | Account state model, suppression behavior, retention schedule. | High | Medium/High | Copy must explain deactivation vs removal without overpromising. |
| Option E: deferred deletion for beta with explicit copy restriction | Avoids false promises before capability exists. | May be unacceptable for public launch or legal review. | Legal/product launch-scope acceptance. | Very high | Low initially | Copy must avoid deletion promises or clearly state available request path. |

No option is selected by this pack. Decision required.

## 10. Data Request Model Options

| Option | Pros | Risks and dependencies |
|---|---|---|
| Support email/manual workflow | Simple to start, allows case-by-case review. | Requires identity verification, auditability, SLA decision, and copy that does not overpromise automation. |
| In-app request form | Better user experience and structured intake. | Requires request records, support routing, status semantics, and privacy-safe payload handling. |
| Dashboard/admin queue | Operationally trackable. | Requires ops/admin authority gates, audit logs, and support process owner. |
| Self-service export | Strong user access model. | Requires export scoping, third-party data exclusion, private data filtering, and security review. |
| Manual export | Easier to constrain initially. | Labor-intensive; risks inconsistent handling without process/audit. |
| Deferred export with policy restriction | Avoids unsupported public promise. | Requires legal/product acceptance and careful copy. |

## 11. Retention / Redaction / Suppression Taxonomy

Decision vocabulary:

- Delete row: remove a database record.
- Delete storage object: remove a file/object from storage.
- Redact fields: remove or replace selected fields while retaining the record.
- Anonymize/pseudonymize: reduce direct identifiability while retaining operational value.
- Deactivate account: prevent login/use without deleting data.
- Hide from public: remove from public UI while retaining backend record.
- Suppress from search/feed/share: prevent listing or public route exposure.
- Archive: retain inactive historical state outside active product flows.
- Retain for audit: keep traceability records with access controls.
- Retain for legal/payment/safety: preserve records needed for accepted exceptions.
- Retain admin log but remove user-facing identity: keep action history while redacting display fields.
- Export: provide an approved copy of eligible user data.
- Correct/update: modify inaccurate user-owned fields.

These terms must not be collapsed. Product deletion, public suppression, storage deletion, audit retention, and legal retention are separate decisions.

## 12. Data Domain Inventory Matrix

| Domain | Example data | User expectation | Default candidate treatment | Legal/product review need | PP-01 evidence need | Later pack dependency |
|---|---|---|---|---|---|---|
| Auth account | Login account, auth user, sessions | Account removed or disabled | Deactivate/delete decision required | High | Auth/account evidence | PP-03 / PP-08 |
| profiles | Name, avatar, public profile fields | Delete/redact personal profile | Redact or delete depending field | High | Profile RLS/schema evidence | PP-03 / PP-05 |
| user_profiles | Mirror/profile fields | Delete/redact duplicate profile data | Reconcile with profiles | High | Table relationship evidence | PP-03 |
| Personal persona | Personal display identity | Remove personal identity | Redact/suppress candidate | High | Persona field evidence | PP-03 |
| Host persona | Organizer identity, host avatar/bio | Remove or transfer host identity | Preserve/redact/transfer decision | High | Transfer/profile evidence | PP-03 / PP-08 |
| Avatars | Personal/organizer avatar URLs | Remove image | Delete object or detach URL | Medium/High | Bucket/URL evidence | PP-09 |
| Event ownership | Hosted events | Remove host association | Transfer/archive/redact/block decision | High | Event/host evidence | PP-05 / PP-08 |
| Event participation | Attendance, tickets, reservations | Remove history | Redact visible profile; retain entitlement where needed | High | Ticket/reservation evidence | PP-04 |
| Venues | Business listings, venue media | Remove if user-owned | Ownership and business-policy decision | High | Venue evidence | PP-05 |
| Tickets | Ticket records, QR/check-in | Preserve entitlement/history or redact identity | Retain with redaction candidate | High | Ticket/check-in evidence | PP-04 |
| Reservations | Event/venue reservations | Remove request or retain history | Retain/redact decision | High | Reservation evidence | PP-04 |
| Claims/gift transfers | Claim sender/recipient, transfer state | Remove personal trace | Retain entitlement; redact identity decision | High | Claim/transfer evidence | PP-04 |
| commerce_orders/payment_attempts/provider_event_log | Orders, provider refs, amounts | Remove payment data | Retain with minimization/redaction candidate | Very high | Provider/order evidence | PP-04 |
| event_media/venue_media/storage objects | Uploaded media, storage paths | Delete my uploads | Object delete vs hide vs retain decision | High | Bucket/object evidence | PP-09 |
| messages/conversations | Message bodies, participants, reads | Delete private messages | Requester-only, redaction, or retention decision | High | DM evidence | PP-10 |
| notifications_v2 | Notification history | Delete notifications | Delete/redact history candidate | Medium | Notification table evidence | PP-06 |
| push_tokens_v1 | Device tokens | Stop pushes/remove device token | Delete token candidate | High | Push token evidence | PP-06 |
| user_notification_settings_v1 | Preference settings | Remove settings | Delete with account candidate | Medium | Settings evidence | PP-06 |
| app_diagnostics | Runtime diagnostics | Remove linked telemetry | Redact/retain/minimize decision | High | Payload/linkage evidence | PP-06 |
| audit/admin/transfer logs | Admin actions, transfer logs | Not usually user-visible | Retain with access controls/redaction | Very high | Audit log evidence | PP-08 |
| reports/moderation evidence | Reports, reasons, evidence | Remove report references | Retain/redact safety decision | High | Report/moderation evidence | PP-07 |
| social graph/friendships/follows/blocks/mutes | Edges and safety blocks | Remove relationships | Delete edges; retain safety blocks decision | Medium/High | Social graph evidence | PP-07 |
| share groups/group visibility | Group membership, private visibility | Remove memberships | Delete/suppress memberships | Medium/High | Group evidence | PP-05 / PP-07 |
| support requests | Tickets/emails/requests | Remove support history | Retain/redact/process decision | High | Support process evidence | PP-08 |

## 13. Identity / Profile / Persona Decision Model

Decisions needed:

- Whether public profile records are deleted, redacted, hidden, or deactivated.
- Whether personal avatar objects are deleted from storage or only detached from profile rows.
- Whether host persona display name, avatar, bio, and organizer identity are retained for event attribution, redacted, or transferred.
- Whether profile mirror tables require synchronized redaction/deletion.
- Whether host identity transfer audit records retain prior identity details.
- Whether public feed/search/profile routes suppress deleted/redacted identities.

The key product decision is whether deleting a user removes identity attribution from hosted events and public content, or preserves event/commerce/safety context with minimized personal fields.

## 14. Auth Account / Login / Device / Push Token Decision Model

Decision areas:

- Auth user deletion, deactivation, or login disablement.
- Login identifiers and session invalidation.
- Device records and push token removal.
- Notification settings removal or retention.
- Account state labels, if any.

This pack does not claim auth deletion implementation exists. PP-01 evidence is required before implementation planning.

## 15. Events / Venues / Host Content Decision Model

Deleting a host account can affect:

- User-created events.
- Published events with tickets, reservations, attendees, media, and public pages.
- Venue/business content and venue media.
- Event participants and ticket holders.
- Cancellation, archive, delete, and public suppression behavior.

Decision required: deleting a host could delete events, redact organizer identity, transfer ownership, archive events, cancel events, or block deletion until events are resolved. This requires product, legal, commerce, and support review.

## 16. Commerce / Tickets / Reservations / Claims Decision Model

Commerce deletion decisions must cover:

- Tickets and ticket status.
- Orders and payment-like records.
- Reservations.
- Claims, gifts, and transfers.
- Payment attempts and provider logs if present.
- Receipts/invoices if any.
- QR/check-in proof and public verification.

Revenue, audit, legal, refund, dispute, and safety needs may require retaining records longer than ordinary profile data. This pack provides no legal advice and does not decide the retention period.

## 17. Media / Storage / Public URL Decision Model

Media decisions must separate:

- Event media.
- Venue media.
- Avatar and poster objects.
- Storage object deletion.
- Database row deletion.
- Metadata redaction.
- Public URLs and signed URLs.
- Cache/public URL caveats.
- Uploader delete controls.
- Host moderation controls.
- Ops/admin moderation controls.

PP-01 and PP-09 must verify bucket policies, object paths, public/private status, and URL behavior before deletion copy or implementation is finalized.

## 18. Messaging / Direct Conversations Decision Model

Messaging deletion decisions must cover:

- Conversation membership.
- Message body retention.
- Sender/recipient identity redaction.
- Read receipts and read state.
- Conversation archive/delete semantics.
- Notification previews.
- Report/support visibility.
- Safety retention for abuse reports if adopted.

Decision required: deletion may remove messages only for the requester, redact sender identity, hide a conversation, delete both sides, or retain records for safety/legal reasons. No final model is selected here.

## 19. Notifications / Reminders Decision Model

Notification decisions must cover:

- Notification history.
- Unread state.
- Reminder records.
- Push tokens.
- Private previews.
- Linked event/ticket/message/profile references.

Likely decision split: push tokens may require immediate deletion or invalidation, while notification history may be deleted, retained, or redacted depending on product and audit needs. PP-06 should refine after PP-01 evidence.

## 20. Diagnostics / Audit Logs / Admin Logs Decision Model

Diagnostics and logs require separate treatment:

- `app_diagnostics` and client telemetry are low-trust and privacy-sensitive.
- Crash/analytics claims must match actual payload and linkage.
- Transfer logs, admin actions, support audit trails, moderation logs, and revenue logs may need retention.
- Audit records may require access control and payload minimization rather than deletion.

Decision required: retention/redaction/pseudonymization model, support visibility, and whether user-facing deletion affects diagnostics or audit logs.

## 21. Abuse / Reports / Moderation Evidence Decision Model

Safety evidence decisions must cover:

- User reports, if implemented.
- Moderation logs.
- Takedown and appeal evidence, if implemented.
- Blocked/reported users.
- Reported content references.
- Reporter identity and reported-user identity.

Deletion requests may conflict with safety evidence retention. Product/legal/trust-safety owners must decide what is retained, redacted, visible, appealable, and exportable.

## 22. Social Graph / Groups / Blocks / Visibility Decision Model

Social deletion decisions must cover:

- Friendships and follows.
- Host followers.
- Blocks and mutes.
- Share groups and group memberships.
- Private/invite visibility.
- Search/feed/discovery side effects.

Decision required: ordinary social edges may be deleted with the account, while block/safety state may need special handling. Public/private suppression must be backend-authoritative.

## 23. Support / Ops / Admin Process Decision Model

Support-mediated deletion or export requires decisions for:

- Intake channel.
- Identity verification.
- Request validation and scope.
- Support/admin permissions.
- Audit trail.
- SLA or 30-day promise.
- Escalation and legal review.
- Status communication.
- Exceptions for commerce, safety, audit, support, and legal records.

Support contact is not process proof. Support read visibility is not deletion/export authority.

## 24. Data Export / Portability Decision Model

Potential export scope:

- Profile and persona data.
- Events and venue content owned by the requester.
- Tickets and reservations.
- Messages, subject to third-party/private conversation constraints.
- Media metadata and possibly media files.
- Notifications and reminders.
- Diagnostics, if user-linked and accepted.
- Payment/order records, subject to provider/legal constraints.
- Support/report data, subject to third-party and safety constraints.

Options:

- No export promise yet.
- Support-mediated export.
- Self-service export later.

Do not promise export without an accepted process, scope, and privacy review.

## 25. Policy-to-Decision Mismatch Register

| Copy/policy signal | Missing decision | Risk | Owner | Recommended next action |
|---|---|---|---|---|
| Settings deletion vs unknown implementation | Self-service deletion model. | Privacy-sensitive | Product + legal | Decide whether settings starts request or completes deletion. |
| Support deletion vs support authority unknown | Support intake, authority, audit, SLA. | Privacy-sensitive, operational/admin-sensitive | Support + legal + ops | Define process before preserving promise. |
| 30-day removal vs domain retention unknown | Per-domain retention and exception scope. | Legal/compliance-sensitive | Legal + product | Decide whether 30-day language is accepted. |
| Data export absence | Export/portability stance and process. | Privacy-sensitive | Legal + product + support | Decide no-export, manual export, or future self-service. |
| Diagnostics anonymous claim vs possible user linkage | Diagnostics disclosure and deletion/redaction model. | Privacy-sensitive | Legal + diagnostics | Verify payload/linkage and decide disclosure. |
| Media deletion vs storage behavior | Storage object/public URL/cache behavior. | Privacy-sensitive | Product + media + legal | Wait for bucket/object evidence. |
| DM deletion/archive ambiguity | Private message lifecycle model. | Privacy-sensitive | Product + legal | Decide requester-only, both-side, redaction, or retention semantics. |
| Commerce retention exceptions | Payment/order/ticket retention scope. | Revenue-sensitive, compliance/audit-sensitive | Commerce + legal | Define retained records and redaction limits. |
| Report/moderation retention | Safety evidence retention, appeal, redaction. | Trust/safety-sensitive | Trust/safety + legal | Decide safety exceptions and support visibility. |

## 26. Implementation-without-Deletion-Decision Register

| Existing technical/product surface | Missing deletion/request decision | Risk | Owner | Recommended next action |
|---|---|---|---|---|
| `app_diagnostics` | Whether user-linked telemetry is deleted, redacted, or retained. | Privacy-sensitive | Diagnostics + legal | Verify payloads and decide retention. |
| Public storage buckets | Whether objects are deleted and URLs invalidated. | Privacy-sensitive | Media + product | Verify bucket/URL behavior. |
| Host identity transfer | Whether prior host/persona data is retained/redacted. | Operational/admin-sensitive | Ops + legal | Decide transfer audit retention. |
| Check-in proof | Whether proof survives account deletion. | Revenue-sensitive | Commerce + staff/host | Define entitlement/audit retention. |
| Ops/admin transfer logs | Whether admin logs retain identity. | Compliance/audit-sensitive | Ops + legal | Decide redaction/access model. |
| Commerce/provider logs | Whether provider/order refs are retained. | Revenue-sensitive | Commerce + legal | Define financial retention exception. |
| Public share/claim/check-in pages | Whether public routes suppress deleted identities. | Privacy-sensitive | Product + public web | Include in PP-05. |
| Notifications/push tokens | Whether tokens are deleted and history retained. | Privacy-sensitive | Notifications | Include in PP-06. |
| DM/conversations | Whether message bodies or memberships are retained. | Privacy-sensitive | Messaging + legal | Include in PP-10. |
| Reports/moderation tables if present | Whether evidence survives deletion. | Trust/safety-sensitive | Trust/safety + legal | Include in PP-07. |

## 27. PP-01 Evidence Dependencies

PP-03 requires PP-01 evidence for:

- Production auth/profile/user_profiles relationship.
- RLS, update, delete, and direct access permissions.
- Storage bucket and object behavior.
- Public URL and signed URL behavior.
- Commerce, ticket, order, reservation, claim, and provider-related tables.
- Notification and push token tables.
- Diagnostics payload and user linkage.
- DM schemas, RPCs, and policies if present.
- Report/moderation schemas and policies if present.
- Admin/support functions and logs.
- Edge Functions if deletion, export, push, reporting, or support workflows exist.

## 28. PP-02 Policy Copy Dependencies

PP-03 must respect PP-02 copy constraints:

- Account deletion through settings is a user-facing signal, not implementation proof.
- Support-mediated deletion copy requires support authority and process evidence.
- 30-day removal copy requires accepted per-domain retention/exceptions.
- Legal/security/business exceptions must be defined by legal/product owners.
- Data export/portability copy is absent or unconfirmed.
- Privacy, terms, support, and checkout copy must be consistent.
- No final legal copy should be treated as approved until legal owner review.

## 29. Product Decision Dependency Checklist

- Deletion model.
- Export model.
- Retention taxonomy.
- Public suppression behavior.
- Host/event ownership after deletion.
- Commerce/ticket retention.
- Media object deletion.
- DM deletion semantics.
- Diagnostics retention.
- Report/evidence retention.
- Support process owner.
- Beta vs public launch scope.

## 30. Legal Review Dependency Checklist

- Privacy deletion claims.
- Retention exceptions.
- Account erasure/data request process.
- Data export/portability.
- Payment/commerce retention.
- Safety/moderation evidence retention.
- Minor/guardian data.
- User content/license/media retention.
- Support verification and escalation.
- Launch-market requirements.

## 31. Risk Priority Matrix

| Priority candidate | Items | Rationale |
|---|---|---|
| Candidate P0 | None assigned by this pack. | Current handbook evidence does not support P0 without production/legal verification. |
| Candidate P1 | Account deletion promise mismatch; 30-day removal promise; support deletion process; public storage/media deletion; diagnostics user linkage; commerce retention. | These affect public privacy promises, support readiness, and sensitive data handling. |
| Candidate P2 | Data export; DM deletion/archive; report/moderation evidence; social graph/block effects; host identity transfer redaction. | Important beta/pre-scale decisions, but some depend on feature scope and production evidence. |
| Candidate P3 | Documentation, cross-linking, and cleanup after decisions. | Lower-risk once model and evidence are accepted. |
| Unknown / Needs verification | Account deletion implementation, data export, active schemas, storage behavior, support/admin process, diagnostics payloads. | Do not convert to patch work before PP-01 evidence and owner decisions. |

## 32. Recommended Decision Records

- Account Deletion Model Decision.
- Data Request / Export Model Decision.
- Retention / Redaction Taxonomy Decision.
- Commerce Retention Exception Decision.
- Media Storage Deletion Decision.
- Messaging Deletion Semantics Decision.
- Diagnostics/Audit Retention Decision.
- Support Privacy Request Process Decision.

## 33. Dependency Map to Later Patch Plan Groups

| Later pack | PP-03 dependency |
|---|---|
| PP-04 Commerce/Refund/Payment Contract Pack | Commerce/order/ticket/payment retention exceptions and redaction limits. |
| PP-05 Public Visibility Suppression Pack | Public suppression behavior after deletion, redaction, archive, and moderation. |
| PP-06 Notification/Diagnostics Privacy Pack | Push token deletion, notification history, diagnostics retention, payload minimization. |
| PP-07 Abuse/Moderation Workflow Pack | Report/moderation evidence retention, redaction, appeal, and support visibility. |
| PP-08 Ops/Admin Support Auditability Pack | Support request processing, admin authority, audit trails, manual privacy workflows. |
| PP-09 Media Storage Lifecycle Pack | Storage object deletion, public URL behavior, media metadata redaction. |
| PP-10 Messaging Privacy Lifecycle Pack | DM delete/archive/redaction/retention semantics and support/report visibility. |

## 34. PP-03 Output Artifacts

Recommended artifacts after execution, not created now:

- `AccountDeletionModelDecision.md`
- `DataRequestExportModelDecision.md`
- `RetentionRedactionTaxonomyDecision.md`
- `AccountDeletionImplementationReadinessChecklist.md`
- `DataDomainDeletionMatrix.md`
- `SupportPrivacyRequestProcessReview.md`
- `PublicDeletionCopyReconciliationReport.md`

## 35. Execution Preconditions

Before executing PP-03:

- Product owner assigned.
- Legal owner assigned.
- Support/admin owner assigned.
- PP-01 evidence available where needed.
- PP-02 copy constraints available.
- Launch scope defined.
- No production changes planned as part of decision work.
- No final legal claims made.
- Sanitized evidence rules accepted.

## 36. Explicitly Blocked Actions

PP-03 blocks:

- Data deletion.
- User export.
- Production access.
- SQL or Supabase CLI.
- Migrations.
- Source code changes.
- Storage object deletion.
- Auth user deletion.
- Policy publication.
- Legal compliance claims.
- Support SLA commitments.
- Immediate patch authorization.

## 37. Unknown / Needs Verification Items

- Whether account deletion exists in app settings.
- Whether deletion is self-service, support-mediated, both, or absent.
- Whether deletion request records or support queues exist.
- Whether 30-day removal is accepted by legal/product owners.
- Which domains are retained for legal, audit, payment, safety, or support reasons.
- Whether data export is supported.
- Whether storage objects are deleted when DB rows are deleted or hidden.
- Whether public URLs can remain reachable after media deletion.
- How hosted events behave when a host deletes an account.
- How tickets, orders, reservations, claims, provider refs, and check-in proof are retained.
- How DMs are deleted, archived, or redacted.
- How diagnostics and audit logs are redacted.
- How reports and moderation evidence are retained.
- Who owns support privacy requests.

## 38. Acceptance Criteria for PP-03 Completion

PP-03 is complete only when:

- Deletion model options are reviewed.
- A selected model is accepted or explicitly deferred.
- Data request/export stance is accepted or deferred.
- Domain deletion matrix is reviewed.
- Retention/redaction taxonomy is accepted.
- Product owner decisions are assigned.
- Legal review dependencies are assigned.
- Support/admin process owner is assigned.
- PP-01 evidence dependencies are linked.
- PP-02 copy constraints are linked.
- Follow-up PP-04 through PP-10 groups are updated or explicitly marked unchanged based on deletion/data-request decisions.
- No final legal text is treated as approved unless the legal owner confirms it.

## 39. Recommended Follow-Up Reports

Recommended follow-up reports after execution:

- Account Deletion Model Decision.
- Data Request / Export Model Decision.
- Data Domain Deletion Matrix.
- Retention / Redaction Taxonomy Decision.
- Support Privacy Request Process Review.
- Account Deletion Implementation Readiness Checklist.
- Public Deletion Copy Reconciliation Report.

## 40. Non-Goals

- No code changes.
- No SQL or migrations.
- No production execution.
- No deletion execution.
- No data export execution.
- No legal advice.
- No compliance claim.
- No launch readiness claim.
- No final legal copy.
- No immediate patch authorization.
- No source-code re-audit.

## 41. Open Questions

- Is deletion self-service, support-mediated, both, or deferred?
- Is there an in-app deletion request path?
- What does 30-day removal mean per data domain?
- Which data is retained for legal, audit, payment, safety, or support reasons?
- Is data export supported?
- How are public media URLs handled after deletion?
- How are hosted events handled if the host deletes the account?
- How are tickets, orders, and reservations retained?
- How are DMs handled?
- How are diagnostics and audit logs redacted?
- How are reports and moderation evidence retained?
- Who owns support privacy requests?

## 42. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `08_PatchPlans/PP03AccountDeletionDataRequestDecisionPack.md` was created/modified.

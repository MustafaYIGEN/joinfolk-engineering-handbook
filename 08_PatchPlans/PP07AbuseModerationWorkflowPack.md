# PP-07 Abuse / Moderation Workflow Pack

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook audit synthesis only
- canonical: false
- Execution status: Not executed
- Legal status: Engineering planning only; not legal advice
- Trust/safety status: Not executed / Workflow not verified
- Moderation status: Not executed / No action performed

## 2. Purpose

This is a decision-pack for defining JoinFolk abuse reporting, moderation, takedown, appeal, restoration, block, mute, safety evidence, public suppression, notification privacy, support/admin review, retention, redaction, and export semantics before implementation work begins.

It is not moderation execution, not report processing, not legal advice, not production verification, and not patch authorization.

## 3. Evidence Boundary

This document is based only on handbook audits, the Release Readiness / Production Hardening Gap Register, PP-01, PP-02, PP-03, PP-04, PP-05, and PP-06.

No source-code inspection, production connection, Supabase CLI, SQL, builds, tests, report workflow verification, moderation execution, takedown execution, evidence export, legal review, policy-copy modification, or final copy drafting was performed.

## 4. PP-07 Scope Summary

PP-07 covers:

- Abuse/report surface inventory.
- Reportable content, user, event, media, message, commerce, and support domains.
- Report intake and triage model.
- Moderation action model.
- Takedown, restore, and appeal model.
- Block, mute, and social safety model.
- Media, message, event, venue, host, commerce, ticket, claim, and check-in abuse models.
- Public suppression model.
- Notification privacy model.
- Evidence, diagnostics, audit, retention, redaction, deletion, and export model.
- Support/admin authority model.
- Backend/RPC/RLS/storage dependencies.
- PP-02 policy, PP-03 deletion, PP-04 commerce, PP-05 public visibility, and PP-06 notification/diagnostics dependencies.
- Dependency mapping to PP-08 through PP-10.

PP-07 does not execute PP-01 and does not authorize report, moderation, takedown, appeal, block, mute, RLS, RPC, storage, notification, copy, database, or production changes.

## 5. Source Register Coverage

| Release gap | Why PP-07 covers it | PP-07 limitation |
|---|---|---|
| RR-GAP-003 | Report, moderation, social, media, and message surfaces may rely on RLS/direct access correctness. | PP-07 does not verify production policies. |
| RR-GAP-004 | ViewerRole, host/staff/support/admin authority affects moderation powers. | PP-07 defines decisions, not implementation. |
| RR-GAP-009 | Public suppression after takedown/moderation must match public route/feed/search behavior. | PP-05 owns full public visibility contract. |
| RR-GAP-012 | Media moderation and storage behavior affect takedown and evidence retention. | PP-09 owns media storage lifecycle details. |
| RR-GAP-014 | Block/mute/group/follow side effects affect visibility, notifications, and interactions. | PP-07 defines safety effects; production verification remains needed. |
| RR-GAP-015 | Feed/search results must suppress moderated/taken-down content where accepted. | PP-05 owns feed/search parity. |
| RR-GAP-017 | DM abuse reporting, private-message evidence, delete/archive, and support visibility remain unresolved. | PP-10 owns full messaging lifecycle. |
| RR-GAP-018 | Support/admin review authority and auditability are central to moderation operations. | PP-08 owns support/admin authority model. |
| RR-GAP-019 | Formal report, review, resolution, appeal, takedown, evidence privacy, and moderation logs are central PP-07 scope. | PP-07 does not confirm a workflow exists. |
| RR-GAP-020 | Report/moderation evidence retention, redaction, deletion exceptions, and export need privacy decisions. | PP-03 owns account deletion/data request model. |
| RR-GAP-021 | Trust/safety policy copy must not overpromise workflow. | PP-02 owns final copy review. |
| RR-GAP-022 | Commerce abuse/fraud, refunds, disputes, ticket/claim/check-in abuse, and revenue evidence require contract alignment. | PP-04 owns commerce contract. |
| RR-GAP-023 | Product decisions must be accepted before patches. | PP-07 recommends decision records only. |

## 6. Abuse / Moderation Workflow Problem Statement

Abuse and moderation is not one button. It crosses:

- Report intake.
- Report reason/category.
- Reported content.
- Reported user.
- Reporter privacy.
- Evidence payload.
- Review queue.
- Moderator, support, and admin authority.
- Host moderation.
- Media moderation.
- DM safety.
- Event/venue safety.
- Commerce fraud and safety.
- Block/mute.
- Public suppression.
- Takedown.
- Appeal.
- Restoration.
- Notifications.
- Diagnostics and audit logs.
- Deletion, retention, export, and redaction exceptions.
- Policy/legal copy.

Report submission, moderation decision, block, mute, takedown, deletion, and public suppression are separate concepts.

## 7. Abuse / Moderation Surface Inventory

| Surface | Example user expectation | Data/function/storage domains affected | Owner | PP-01 evidence dependency | PP-02 copy dependency | PP-03 retention dependency | PP-05 visibility dependency | PP-06 notification dependency | Current status | Recommended PP-07 decision need |
|---|---|---|---|---|---|---|---|---|---|---|
| Report event | Unsafe/fake event can be reported. | Events, reports, support/admin. | Trust/safety + product | Report/event evidence | Safety policy copy | Evidence retention | Event suppression | Status notification | Not confirmed | Decide event report workflow. |
| Report profile/user | Abusive account can be reported. | Profiles, user_profiles, reports. | Trust/safety | Report/profile evidence | Conduct copy | Identity retention/redaction | Profile suppression | Report status privacy | Not confirmed | Decide user/profile report model. |
| Report host/persona | Abusive host persona can be reported. | Host persona, events, transfers. | Trust/safety + ops | Persona/report evidence | Host policy copy | Persona retention | Host public fallback | Support notification | Not confirmed | Decide host persona review. |
| Report venue/business | Misleading/unsafe venue can be reported. | Venues, venue media, reservations. | Venue + trust/safety | Venue/report evidence | Venue policy copy | Venue retention | Venue suppression | Status notification | Not confirmed | Decide venue report owner. |
| Report media | Media can be reported or hidden. | Event_media, venue_media, storage. | Media + trust/safety | Media moderation evidence | Media policy copy | Evidence/storage retention | Media takedown | Uploader/host notice | Partial host moderation evidence | Decide media report/takedown flow. |
| Report message/DM | Private message abuse can be reported. | Messages, conversations, evidence. | Messaging + trust/safety | DM/report evidence | DM safety copy | Private evidence retention | Deep-link suppression | DM notification privacy | Not confirmed | Decide DM evidence boundaries. |
| Report ticket/claim/check-in abuse | Fraudulent claim/QR can be reported. | Tickets, claims, proof, commerce. | Commerce + trust/safety | Commerce/proof evidence | Ticket policy copy | Commerce evidence retention | Public verification suppression | Commerce status notice | Unknown | Decide fraud/safety review. |
| Report spam/fake event | Spam/fake content can be flagged. | Events, profiles, feeds/search. | Trust/safety + product | Report/feed evidence | Conduct copy | Evidence retention | Feed suppression | Status notification | Not confirmed | Decide spam/fake category. |
| Report harassment/threat | Harassment can be reported. | Users, messages, events, evidence. | Trust/safety + legal | Report/message/profile evidence | Safety copy | Evidence exception | Public suppression | Sensitive notification | Not confirmed | Decide priority/escalation. |
| Report illegal/hateful/violent content | Severe content can be escalated. | Content, users, reports, logs. | Trust/safety + legal | Report/moderation evidence | Legal/safety copy | Retention exception | Takedown | Escalation notice | Not confirmed | Decide escalation path. |
| Block user | User stops interaction. | Social graph, DMs, notifications. | Product + trust/safety | Block RPC/table evidence | Block copy | Block retention | Visibility effects | Notification suppression | May exist | Decide block effects. |
| Mute user/content | User suppresses notifications/content. | Notifications, feed, social. | Product | Mute evidence | Mute copy | Mute retention | Visibility or interaction | Delivery suppression | Not confirmed | Decide mute semantics. |
| Host hide/moderate media | Host can hide event media. | Event media, storage, audit. | Host/media | Host moderation evidence | Media copy | Storage/evidence retention | Public media suppression | Uploader notice | Partial evidence | Define host-scoped authority. |
| Support/admin takedown | Support can suppress content. | Reports, content, audit. | Support/admin + trust/safety | Admin/moderation evidence | Support/safety copy | Audit retention | Public suppression | Takedown notice | Unknown | Define authority and audit. |
| Appeal/restoration | User can request restoration. | Moderation actions, evidence. | Trust/safety + legal | Appeal evidence | Appeal copy | Appeal retention | Restore behavior | Appeal notification | Not confirmed | Decide if supported. |
| Report status notification | Reporter may get status. | Notifications, reports. | Trust/safety + notifications | Notification evidence | Report copy | Evidence privacy | Public state | Payload rules | Unknown | Decide what can be communicated. |
| Moderator/admin review queue | Reports are reviewed. | Reports, evidence, support/admin. | Support/admin | Queue/evidence evidence | Support process copy | Audit retention | Suppression powers | Admin notifications | Not confirmed | Decide review roles. |
| Evidence/audit log | Actions are traceable. | Evidence, logs, diagnostics. | Trust/safety + ops | Audit/log evidence | Retention copy | Deletion exceptions | Restore/takedown trail | Notification logs | Unknown | Decide audit model. |

## 8. Reportable Surface Inventory

| Reportable object | Potential abuse category | Public/private sensitivity | Evidence needed | Owner | Current status | Decision needed |
|---|---|---|---|---|---|---|
| Event | Fake, unsafe, misleading, illegal, spam. | Public/private depending event. | Event id, reporter, reason, non-secret details. | Product + trust/safety | Not confirmed | Decide event report categories. |
| Venue | Misleading business, unsafe venue, abusive venue media. | Public/business-sensitive. | Venue id, reason, media refs if any. | Venue + trust/safety | Not confirmed | Decide venue report path. |
| Profile | Harassment, impersonation, abusive identity. | Identity-sensitive. | Profile id, reason, public fields. | Trust/safety + profile | Not confirmed | Decide user/profile reporting. |
| Host persona | Fake host, abusive organizer, misleading bio. | Identity/operations-sensitive. | Persona/event refs. | Trust/safety + ops | Not confirmed | Decide host persona handling. |
| Media upload | Nudity, abuse, illegal, harassment, copyright-like issue if accepted. | Storage/privacy-sensitive. | Media id/path refs, reason. | Media + trust/safety | Partial host moderation evidence | Decide media evidence and takedown. |
| Comment/caption if applicable | Harassment, spam, illegal content. | Public/private unknown. | Comment id, reason. | Product + trust/safety | Unknown | Decide if comments are reportable. |
| DM/message | Harassment, threats, spam. | Private communication. | Message id, minimal excerpt/pointer if accepted. | Messaging + trust/safety | Not confirmed | Decide evidence boundaries. |
| Ticket/claim | Fraud, abuse, duplicate claim, resale if accepted. | Revenue-sensitive. | Ticket/claim id, event id, non-secret state. | Commerce + trust/safety | Unknown | Decide fraud reporting. |
| Check-in/QR abuse | Fake QR, misuse, scanner issue. | Revenue/security-sensitive. | Proof/ticket refs, staff context. | Staff/host + commerce | Unknown | Decide proof evidence. |
| Group/invite | Abuse in private group or invite spam. | Private/social-sensitive. | Group/invite refs. | Social + trust/safety | Unknown | Decide group report access. |
| Notification/deep link | Harassing or leaking notification. | Privacy-sensitive. | Notification id/type, payload metadata. | Notifications + trust/safety | Unknown | Decide notification report path. |
| Support/admin action if applicable | Abuse of support/admin action. | Operational/admin-sensitive. | Audit log refs. | Ops/admin | Unknown | Decide escalation path. |

## 9. Trust / Safety State Taxonomy

- Reported.
- Under review.
- Actioned.
- Dismissed.
- Escalated.
- Hidden.
- Suppressed.
- Taken down.
- Restored.
- Appealed.
- Appeal accepted.
- Appeal denied.
- Blocked.
- Muted.
- Reporter.
- Reported user.
- Moderator.
- Host moderator.
- Support reviewer.
- Admin reviewer.
- Evidence retained.
- Evidence redacted.
- Public placeholder.
- Not found / suppressed.

Do not collapse report, takedown, block, mute, deletion, and public suppression.

## 10. Report Intake Decision Model

Decision areas:

- Who can report.
- Anonymous vs authenticated reporting.
- Participant, ticket-holder, host, and staff reporting.
- Report categories.
- Required fields.
- Evidence payload.
- Reporter privacy.
- Rate limiting and abuse prevention as decision items only.
- Duplicate reports.

Decision needed: accepted report intake contract and report target types before any workflow or copy is treated as final.

## 11. Report Review / Triage Decision Model

Decision areas:

- Review queue.
- Priority/severity.
- Triage roles.
- Support/admin/trust-safety owner.
- Escalation.
- SLA if any.
- Audit trail.

Decision needed: who reviews reports, who can see evidence, who can resolve, and what is audited.

## 12. Moderation Action Decision Model

Possible actions:

- No action.
- Warning.
- Hide.
- Suppress.
- Takedown.
- Restore.
- Account, event, media, profile, or commerce restriction.
- Host moderation.
- Support/admin moderation.

Decision needed: allowed action matrix by role and object type. Host moderation must be event-scoped and not treated as ops/admin authority.

## 13. Takedown / Restore / Appeal Decision Model

Decision areas:

- Immediate takedown.
- Pending review.
- Appeal request.
- Appeal review.
- Restoration.
- Final action.
- Public placeholder vs not found.

Decision needed: whether appeal exists, who owns it, what timelines are promised, and what public state appears during appeal. No legal wording is provided.

## 14. Block / Mute / Social Safety Decision Model

Decision areas:

- Block.
- Unblock.
- Mute.
- Unmute.
- Follow/friend/host follower effects.
- DM effects.
- Notification effects.
- Feed/search/profile/event visibility effects.

Decision needed: block and mute effects across interactions, visibility, notifications, history, and future discovery.

## 15. Media Moderation Decision Model

Decision areas:

- Event media.
- Venue media.
- Avatars.
- Posters.
- Memory wall.
- Uploader delete.
- Host hide/unhide.
- Support/admin takedown.
- Storage object retention.

Decision needed: moderation vs deletion vs storage object handling, including whether public URLs remain reachable after moderation.

## 16. Messaging / DM Safety Decision Model

Decision areas:

- Report message.
- Block sender.
- Mute conversation.
- Hide/archive/delete message.
- Support/admin review.
- Notification snippets.
- Evidence retention.

Decision needed: private communication safety model, message-body evidence boundaries, and whether support/admin review exists.

## 17. Event / Venue / Host Abuse Decision Model

Decision areas:

- Fake event.
- Unsafe event.
- Illegal event.
- Misleading venue.
- Abusive host profile/persona.
- Event cancellation/takedown.
- Venue/business claim abuse.

Decision needed: when event, venue, profile, or host persona is hidden, suppressed, escalated, restored, or retained for evidence.

## 18. Commerce / Ticket / Claim / Check-In Abuse Decision Model

Decision areas:

- Ticket fraud.
- Fake claim link.
- QR/check-in abuse.
- Chargeback/dispute-related abuse.
- Refund abuse.
- Reservation abuse.
- Host sales abuse.

Decision needed: fraud/safety review boundaries and interaction with PP-04 commerce/refund/payment contract.

## 19. Public Visibility / Suppression Decision Model

Decision areas:

- Report pending.
- Takedown.
- Moderation hidden.
- Appeal pending.
- Restored.
- Blocked/muted.
- Deleted/redacted.

Decision needed: what remains public, what is suppressed, what fallback is shown, and how feed/search/share/detail routes react.

## 20. Notification / Communication Privacy Decision Model

Decision areas:

- Report received notification.
- Report status notification.
- Takedown notification.
- Appeal notification.
- Restoration notification.
- Reporter/reported user privacy.
- Sensitive evidence payloads.

Decision needed: what can be sent via push, what is in-app only, what is support/admin-only, and what must never include private evidence.

## 21. Diagnostics / Audit / Evidence Decision Model

Decision areas:

- Report evidence.
- Moderation action logs.
- Support/admin audit logs.
- Diagnostics tied to report.
- Evidence attachments if any.
- Read access.
- Redaction.

Decision needed: auditability, least privilege, evidence retention, and payload minimization.

## 22. Support / Ops / Admin Authority Decision Model

Decision areas:

- Support reviewer.
- Ops/admin reviewer.
- Host moderator.
- Staff authority.
- Escalation/legal review.
- Manual suppression.
- Restore.
- Audit trail.

Decision needed: role/action matrix and approval boundaries. Staff/scanner authority does not imply moderation authority unless explicitly accepted.

## 23. Retention / Redaction / Deletion / Export Decision Model

Decision areas:

- Reports.
- Moderation evidence.
- Action logs.
- Appeal records.
- Block/mute state.
- Reporter identity.
- Reported user identity.
- Deleted account interactions.

Decision needed: what survives account deletion, what is exportable, what is redacted, and what is retained for safety/audit.

## 24. Backend / RPC / RLS / Storage Verification Dependencies

PP-07 requires PP-01 evidence for:

- Report tables if present.
- Moderation tables/logs if present.
- Block/unblock RPCs.
- Mute tables/RPCs if present.
- Media moderation RPCs/functions.
- Host moderation functions.
- Support/admin moderation functions.
- Public suppression flags.
- Storage policies for moderated media.
- RLS policies for reports, moderation, social, media, messages, and support/admin tables.
- Grants, `search_path`, and `SECURITY DEFINER` posture where relevant.
- Notification functions for report/moderation status.
- Audit/diagnostics tables.

No SQL is included in this pack.

## 25. Abuse / Moderation Data Domain Inventory Matrix

| Domain | Example data | User expectation | Workflow decision needed | Legal/product/trust-safety review need | PP-01 evidence need | PP-03/PP-05/PP-06 dependency | Later pack dependency |
|---|---|---|---|---|---|---|---|
| Reports if present | Target, reason, reporter | Report is private and reviewed | Intake/review model | High | Table/RPC evidence | Retention/visibility/notification | PP-08 |
| Report evidence | Text, refs, attachments if any | Evidence protected | Payload minimization | High | Evidence schema | Redaction/export | PP-08 |
| Moderation actions | Hide/takedown/restore | Action is consistent | Action matrix | High | Logs/RPC evidence | Public suppression | PP-08 |
| Appeals | Appeal request/outcome | Fair review if promised | Appeal process | High | Appeal evidence | Notification/retention | PP-08 |
| Blocks | Block edges | Stop interaction | Effects matrix | Medium/High | Block RPC/table evidence | Visibility/notification | PP-10 |
| Mutes | Mute prefs/edges | Suppress notifications/content | Effects matrix | Medium/High | Mute evidence | Notification behavior | PP-06 |
| Event_media moderation state | Hidden/taken-down media | Unsafe media removed | Host/support authority | High | Media RPC/storage evidence | Public URL behavior | PP-09 |
| Venue_media moderation state | Venue media status | Unsafe media removed | Venue/support authority | High | Media evidence | Storage retention | PP-09 |
| Messages/DM reports | Message refs/evidence | Private abuse handled | Evidence and support access | High | DM/report evidence | DM privacy | PP-10 |
| Events/venues takedown | Suppressed public pages | Unsafe/fake content removed | Takedown/restore | High | Public route/RLS evidence | Public suppression | PP-05 |
| Profiles/personas restrictions | Restricted profile/persona | Abusive identity handled | Restriction/redaction | High | Profile/persona evidence | Deletion/redaction | PP-03 |
| Tickets/claims/check-in abuse | Fraud evidence | Fraud handled safely | Commerce safety model | High | Commerce/proof evidence | Retention/notification | PP-04 |
| Notifications for report/moderation | Status/takedown notices | No evidence leakage | Payload model | High | Notification evidence | Private preview | PP-06 |
| Support/admin audit logs | Reviewer/action trail | Actions auditable | Audit model | High | Audit evidence | Retention | PP-08 |
| Diagnostics tied to moderation | Error/support data | Debugging minimal | Access/retention | Medium/High | Diagnostics evidence | Redaction | PP-06 |

## 26. Policy-to-Workflow Mismatch Register

| Copy/policy signal | Missing workflow decision | Risk | Owner | Recommended next action |
|---|---|---|---|---|
| Prohibited conduct copy vs no formal workflow confirmed | Report intake, review, action matrix. | Trust/safety-sensitive | Product + trust/safety + legal | Decide workflow before final policy promises. |
| Report/review/appeal/takedown copy absent/unknown | Whether process exists. | Trust/safety-sensitive | Trust/safety + legal | Define or avoid claims. |
| Block/unblock behavior vs visibility/notification effects unknown | Block effects matrix. | Privacy/trust-sensitive | Product + social | Decide block semantics. |
| Mute behavior unknown | Mute effects matrix. | Product correctness | Product + notifications | Decide mute semantics. |
| Media moderation vs storage deletion unknown | DB/storage/public URL behavior. | Privacy-sensitive | Media + trust/safety | Link to PP-09. |
| DM report/support review unknown | Private message evidence and support access. | Privacy-sensitive | Messaging + trust/safety | Link to PP-10. |
| Support/admin takedown authority unknown | Admin action powers and audit. | Operational/admin-sensitive | Ops + support | Link to PP-08. |
| Report evidence retention vs deletion/export unknown | Safety exception model. | Privacy/legal-sensitive | Legal + trust/safety | Link to PP-03. |
| Report/moderation notifications privacy unknown | Status and evidence payload. | Privacy-sensitive | Notifications + trust/safety | Link to PP-06. |

## 27. Implementation-without-Moderation-Contract Register

| Existing technical/product surface | Missing abuse/moderation contract | Risk | Owner | Recommended next action |
|---|---|---|---|---|
| Block/unblock RPCs | Cross-surface block effects. | Privacy/trust-sensitive | Product + social | Define block effects. |
| Media hide/unhide/moderate helpers | Host/support/admin boundaries. | Privacy-sensitive | Media + trust/safety | Define media moderation workflow. |
| Public suppression flags | Report/takedown/appeal public behavior. | Privacy/trust-sensitive | Product + public web | Link to PP-05. |
| Support/admin tools | Review/takedown/restore authority and audit. | Operational/admin-sensitive | Ops + support | Link to PP-08. |
| Diagnostics/audit logs | Moderation evidence and action traceability. | Compliance/audit-sensitive | Diagnostics + ops | Define audit model. |
| Notification/report status pathways | Payload and recipient privacy. | Privacy-sensitive | Notifications | Link to PP-06. |
| Message/DM surfaces | Private report/evidence handling. | Privacy-sensitive | Messaging | Link to PP-10. |
| Event/venue/profile public pages | Suppression/takedown behavior. | Trust/safety-sensitive | Product | Define public suppression contract. |
| Ticket/claim/check-in public verification | Fraud/abuse handling. | Revenue-sensitive | Commerce + trust/safety | Link to PP-04. |

## 28. PP-01 Evidence Dependencies

PP-07 needs PP-01 evidence for:

- Production report/moderation/block/mute tables and RPCs.
- RLS for reports, moderation, social, media, message tables.
- Media moderation RPC bodies, grants, and security mode.
- Support/admin moderation functions and audit logs.
- Public suppression behavior.
- Storage object behavior for moderated media.
- Notification/report delivery behavior.
- Diagnostics/evidence access.
- Production deployed public routes.

## 29. PP-02 Policy Copy Dependencies

PP-07 must respect PP-02 constraints:

- Prohibited conduct copy must match workflow.
- Report, appeal, and takedown copy must not overpromise process.
- Support copy must not imply unverified review or SLA.
- Public safety/legal copy needs owner approval.
- No final trust/safety copy should be treated as approved until legal/product owner approval.

## 30. PP-03 Deletion / Data Request Dependencies

PP-07 must respect PP-03 constraints:

- Report/moderation evidence may need retention exceptions.
- Reporter and reported-user identity may need redaction.
- Account deletion must define effects on reports, blocks, mutes, appeals, and evidence.
- Data export may exclude or redact third-party/safety evidence if accepted.
- Support privacy request process may affect report visibility.

## 31. PP-04 Commerce / Refund / Payment Dependencies

PP-07 must respect PP-04 constraints:

- Commerce fraud/abuse may require ticket, order, claim, and check-in evidence retention.
- Refund/dispute abuse may require support/admin review.
- Claim/QR/check-in abuse affects public verification and entitlement.
- Support/admin commerce authority must be auditable.

## 32. PP-05 Public Visibility Dependencies

PP-07 must respect PP-05 constraints:

- Takedown/moderation must suppress public visibility where accepted.
- Report pending may or may not suppress content.
- Block/mute may affect visibility or only interaction.
- Public fallback/not-found behavior must be decided.
- Public-safe fields for moderated/taken-down content must be defined.

## 33. PP-06 Notification / Diagnostics Dependencies

PP-07 must respect PP-06 constraints:

- Report/moderation notifications must not leak evidence.
- Appeal/takedown notifications need payload boundaries.
- Diagnostics tied to moderation need access and retention rules.
- Deep links in report/moderation notifications must reauthorize.

## 34. Product Decision Dependency Checklist

- Report intake model.
- Report categories.
- Reportable surfaces.
- Reviewer role model.
- Moderation action matrix.
- Takedown/restore/appeal model.
- Block/mute effects.
- Media moderation model.
- DM safety model.
- Event/venue/host abuse model.
- Commerce abuse model.
- Public suppression model.
- Evidence retention/export/redaction model.
- Support/admin authority model.
- Beta vs public launch scope.

## 35. Legal / Trust-Safety Review Dependency Checklist

- Prohibited conduct/community rules.
- Report/review/appeal/takedown policy.
- Reporter/reported user privacy.
- Evidence retention.
- Safety/legal exceptions to deletion/export.
- Moderation notification wording.
- Support escalation/contact copy.
- Illegal/hateful/violent content handling.
- Minors/guardian issues if applicable.
- Public takedown/restoration language.

## 36. Risk Priority Matrix

| Priority candidate | Items | Rationale |
|---|---|---|
| Candidate P0 | None assigned by this pack. | Current handbook evidence does not support P0 without production/legal verification. |
| Candidate P1 | Formal report workflow absent/unknown; support/admin takedown authority unknown; public suppression after takedown unknown; report evidence retention/deletion unknown; DM/report privacy unknown; media moderation/storage mismatch. | These affect trust/safety readiness and privacy-sensitive evidence handling. |
| Candidate P2 | Block/mute side effects; appeal/restoration process; report notifications; commerce abuse/fraud workflow; diagnostics evidence access. | Important beta/pre-scale workflow decisions. |
| Candidate P3 | Copy polish and documentation after decisions. | Lower-risk after workflow model is accepted. |
| Unknown / Needs verification | Existing report/moderation tables, formal workflow, support/admin tools, mute behavior, DM reports, evidence logs. | Do not convert to patch work before PP-01 evidence and owner decisions. |

## 37. Recommended Decision Records

- Abuse Reporting Workflow Decision.
- Moderation Action Matrix Decision.
- Takedown Restore Appeal Decision.
- Block Mute Safety Effects Decision.
- Media Moderation Workflow Decision.
- Messaging Safety Workflow Decision.
- Report Evidence Retention Decision.
- Support Admin Moderation Authority Decision.
- Report Moderation Notification Privacy Decision.

## 38. Dependency Map to Later Patch Plan Groups

PP-07 depends on PP-01, PP-02, PP-03, PP-04, PP-05, and PP-06.

| Later pack | PP-07 dependency |
|---|---|
| PP-08 Ops/Admin Support Auditability Pack | Support/admin review, takedown, restore, audit logs, escalation, private-data access. |
| PP-09 Media Storage Lifecycle Pack | Media takedown, host moderation, storage object retention/deletion, public URL behavior. |
| PP-10 Messaging Privacy Lifecycle Pack | DM reports, message evidence, support/admin review, block/mute, DM notification privacy. |

## 39. PP-07 Output Artifacts

Recommended artifacts after execution, not created now:

- `AbuseReportingWorkflowDecision.md`
- `ModerationActionMatrix.md`
- `TakedownRestoreAppealDecision.md`
- `BlockMuteVisibilityEffectsDecision.md`
- `MediaModerationWorkflowDecision.md`
- `MessagingSafetyWorkflowDecision.md`
- `ReportEvidenceRetentionDecision.md`
- `SupportAdminModerationAuthorityReview.md`
- `AbuseModerationImplementationReadinessChecklist.md`

## 40. Execution Preconditions

Before executing PP-07:

- Product owner assigned.
- Legal/privacy owner assigned.
- Trust/safety owner assigned.
- Support/admin owner assigned.
- Backend/security owner assigned.
- Media owner assigned.
- Messaging owner assigned if DM scope exists.
- PP-01 production evidence available where needed.
- PP-02 copy constraints available.
- PP-03 deletion/data-request constraints available.
- PP-04 commerce constraints available.
- PP-05 visibility constraints available.
- PP-06 notification/diagnostics constraints available.
- Launch scope defined.
- No production changes planned as part of decision work.
- No moderation/takedown/report execution.
- No SQL/RLS/RPC/storage changes.
- No final legal claims made.
- Sanitized evidence rules accepted.

## 41. Explicitly Blocked Actions

PP-07 blocks:

- Report execution.
- Moderation execution.
- Takedown execution.
- Appeal/restoration execution.
- Block/mute execution.
- Evidence export.
- User data collection into docs.
- Public route changes.
- Notification behavior changes.
- Storage object deletion.
- RLS/RPC/storage changes.
- Production access.
- SQL or Supabase CLI.
- Migrations.
- Source code changes.
- Policy publication.
- Legal compliance claims.
- Immediate patch authorization.

## 42. Unknown / Needs Verification Items

- Whether a formal report system exists.
- Which objects can be reported.
- Whether report tables/RPCs exist in production.
- Whether mute behavior exists.
- Which block/unblock effects are active.
- Whether support/admin review queues exist.
- Whether appeal/restoration exists.
- Whether media moderation deletes storage objects or only hides records.
- Whether DM report/support review exists.
- How report/moderation notifications are delivered and masked.
- How report/moderation evidence is retained, redacted, exported, or excluded.
- Who can suppress or restore public content.
- Whether moderation actions are audited.
- Beta launch trust/safety scope.

## 43. Acceptance Criteria for PP-07 Completion

PP-07 is complete only when:

- Abuse/moderation surface inventory is confirmed.
- Reportable surface inventory is confirmed.
- Report intake model is accepted or explicitly deferred.
- Report review/triage model is accepted or explicitly deferred.
- Moderation action matrix is accepted or explicitly deferred.
- Takedown/restore/appeal model is accepted or explicitly deferred.
- Block/mute effects model is accepted or explicitly deferred.
- Media moderation model is accepted or explicitly deferred.
- Messaging safety model is accepted or explicitly deferred.
- Event/venue/host abuse model is accepted or explicitly deferred.
- Commerce/ticket/claim/check-in abuse model is accepted or explicitly deferred.
- Public suppression model is accepted or explicitly deferred.
- Report/moderation notification privacy model is accepted or explicitly deferred.
- Evidence/audit/retention model is accepted or explicitly deferred.
- Support/admin authority model is accepted or explicitly deferred.
- PP-01 evidence dependencies are linked.
- PP-02 copy constraints are linked.
- PP-03 deletion constraints are linked.
- PP-04 commerce constraints are linked.
- PP-05 visibility constraints are linked.
- PP-06 notification/diagnostics constraints are linked.
- Product owner decisions are assigned.
- Legal/privacy/trust-safety review dependencies are assigned.
- Support/admin owner is assigned.
- Backend/security owner is assigned.
- Media owner is assigned.
- Messaging owner is assigned or explicitly out of scope.
- Follow-up PP-08 through PP-10 groups are updated or explicitly marked unchanged based on abuse/moderation workflow decisions.
- No final legal/trust-safety/moderation text is treated as approved unless the responsible owner confirms it.

## 44. Recommended Follow-Up Reports

Recommended follow-up reports after execution:

- Abuse Reporting Workflow Decision.
- Moderation Action Matrix.
- Takedown / Restore / Appeal Decision.
- Block / Mute Visibility Effects Decision.
- Media Moderation Workflow Decision.
- Messaging Safety Workflow Decision.
- Report Evidence Retention Decision.
- Support/Admin Moderation Authority Review.
- Abuse Moderation Implementation Readiness Checklist.

## 45. Non-Goals

- No code changes.
- No SQL or migrations.
- No production execution.
- No report execution.
- No moderation execution.
- No takedown execution.
- No appeal/restoration execution.
- No block/mute execution.
- No evidence export.
- No user data collection into docs.
- No public route changes.
- No notification behavior changes.
- No RLS/RPC/storage changes.
- No storage object deletion.
- No legal advice.
- No compliance claim.
- No launch readiness claim.
- No final community/trust-safety/moderation policy copy.
- No immediate patch authorization.
- No source-code re-audit.

## 46. Open Questions

- Does a formal report system exist?
- Which objects can be reported?
- Who can report?
- What report categories are accepted?
- Who reviews reports?
- What actions can host, support, or admin take?
- Does appeal/restoration exist?
- What does block do?
- What does mute do?
- Does block affect visibility, interaction, notifications, or all of them?
- How is media moderation handled?
- How are DMs reported or reviewed?
- How is report evidence retained or redacted?
- What happens to reports after account deletion?
- How are report/moderation notifications masked?
- Who can suppress or restore public content?
- What is beta launch scope for trust/safety?

## 47. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No report/moderation/takedown/appeal/block/mute/public-route/notification/RLS/RPC/storage action was executed.
- No files were staged or committed.
- Only `08_PatchPlans/PP07AbuseModerationWorkflowPack.md` was created/modified.

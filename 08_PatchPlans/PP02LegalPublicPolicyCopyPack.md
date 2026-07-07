# PP-02 Legal / Public Policy Copy Pack

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

This pack defines how JoinFolk should reconcile public, mobile, web, support, checkout, privacy, terms, trust/safety, and legal policy copy with verified product and backend behavior before launch.

This is a planning pack. It is not legal advice, does not draft final legal terms, does not publish policy copy, and does not authorize application, web, mobile, dashboard, backend, database, storage, or Supabase changes.

## 3. Evidence Boundary

This document is based only on existing handbook audits, the Release Readiness / Production Hardening Gap Register, and PP-01 Production Verification Pack.

No app, dashboard, mobile, web, or Supabase source was inspected for this pack. No production connection, Supabase CLI, SQL, builds, tests, installs, legal review, final policy drafting, or external verification were performed. User-facing policy text is treated as product/legal promise evidence, not implementation proof. Implementation evidence is treated as engineering evidence, not legal compliance proof.

## 4. PP-02 Scope Summary

PP-02 covers:

- Mobile and web privacy, terms, support, refund, notification, diagnostics, reporting, public content, and legal-contact copy reconciliation.
- A policy copy source-of-truth model for later legal/product review.
- Policy-to-implementation mismatch tracking.
- Implementation-without-policy tracking.
- Legal review and product decision dependencies.
- PP-01 production evidence dependencies for copy claims.
- Public launch copy freeze criteria.
- Dependency mapping to later packs, especially PP-03 through PP-10.

PP-02 does not execute PP-01, does not write final legal copy, and does not modify source code or production systems.

## 5. Source Register Coverage

| Release gap | Why PP-02 covers it | PP-02 limitation |
|---|---|---|
| RR-GAP-006 | Refund/payment policy text conflicts with checkout copy and provider/refund/dispute behavior is not confirmed. | PP-02 can define copy reconciliation needs; PP-04 and legal/product decisions decide the payment contract. |
| RR-GAP-009 | Public web/share/feed/search visibility promises must match backend/public route behavior. | PP-02 cannot verify route behavior; PP-01 and PP-05 provide evidence. |
| RR-GAP-010 | Notification settings and private-preview copy may be ahead of delivery enforcement. | PP-02 identifies copy risk; PP-01 and PP-06 verify delivery behavior. |
| RR-GAP-011 | Diagnostics disclosure may not match `app_diagnostics`, payload, retention, or support visibility. | PP-02 defines disclosure dependencies; PP-06 handles diagnostics/privacy details. |
| RR-GAP-012 | Media deletion, public URL behavior, and content-license copy need product/legal alignment. | PP-02 does not verify storage deletion; PP-01 and PP-09 provide evidence. |
| RR-GAP-013 | Profile/persona/public identity and host identity transfer retention need policy alignment. | PP-02 flags policy copy needs; PP-03 decides deletion/retention details. |
| RR-GAP-018 | Support/admin process promises require process evidence and auditability. | PP-02 maps support copy; PP-08 defines operational authority/auditability. |
| RR-GAP-019 | Reporting/moderation/appeal/takedown policy needs alignment with implementation evidence. | PP-02 maps copy and legal review needs; PP-07 defines workflow. |
| RR-GAP-020 | Account deletion, data export, retention, redaction, and privacy policy promises are unresolved. | PP-02 cannot resolve implementation; PP-03 owns deletion/data-request decisions. |
| RR-GAP-021 | Web legal placeholders, account deletion promises, refund mismatch, notification copy, diagnostics disclosure, and legal identity remain unresolved. | PP-02 is the direct planning pack for policy copy reconciliation, not legal approval. |
| RR-GAP-022 | Commerce/payment/ticket/order retention, receipt, provider payload, and revenue audit policy need alignment. | PP-02 maps copy; PP-04 and legal review define the payment/refund contract. |
| RR-GAP-023 | Multiple product/legal decisions must be accepted before copy can be final. | PP-02 lists decision dependencies but does not create binding decisions. |

## 6. Policy Surface Inventory

| Policy surface / copy domain | Current policy signal from audits | Technical backing status | Required owner | PP-01 evidence dependency | Risk class | PP-02 recommendation |
|---|---|---|---|---|---|---|
| Mobile privacy policy | Substantive privacy copy exists and references collection, public profile/media, support deletion, analytics/crash, children, and support contact. | Partially supported; reconciliation required. | Legal + product | Diagnostics, deletion, public route, storage, notification evidence | Privacy-sensitive, legal/compliance-sensitive | Reconcile against verified behavior before treating as canonical. |
| Mobile terms | Terms include age/guardian, account responsibility, prohibited conduct, host/venue obligations, suspension/termination, and account deletion through settings. | Partially supported; account deletion and enforcement evidence incomplete. | Legal + product | Account deletion and support process evidence | Legal/compliance-sensitive, product correctness | Review against product capabilities and legal owner decisions. |
| Web privacy page | Prior audit flagged web privacy as placeholder/generalized. | Policy ahead of verified launch copy. | Legal + web/product | Public web legal route deployment and current copy evidence | Legal/compliance-sensitive | Do not treat as launch-ready without legal/product review. |
| Web terms page | Prior audit flagged web terms as placeholder/generalized. | Policy ahead of verified launch copy. | Legal + web/product | Public web legal route deployment and current copy evidence | Legal/compliance-sensitive | Replace or explicitly defer only after legal/product decision. |
| Web support page / FAQ | Support email and FAQ-style copy were observed in audits. | Support process backing incomplete. | Support + product + legal | Current public support route evidence | Operational/admin-sensitive | Avoid process promises beyond supported workflows. |
| Checkout refund copy | Checkout copy says all sales final/no refunds/exchanges. | Conflicts with host refund policy signal in terms. | Product + legal + commerce | Payment/provider/refund evidence | Revenue-sensitive, legal/compliance-sensitive | Decide canonical purchase-moment refund copy. |
| Host refund policy text | Terms indicate ticket purchases are subject to host refund policy. | Refund/provider implementation not confirmed. | Product + legal + host operations | Payment/refund/support process evidence | Revenue-sensitive | Reconcile with checkout copy and support process. |
| Account deletion/settings copy | Mobile terms mention account deletion through app settings. | Self-service implementation not confirmed by privacy audit. | Product + legal | Account deletion implementation evidence | Privacy-sensitive | Treat as high-risk until verified or revised by owner. |
| Support-mediated deletion copy | Mobile privacy says deletion can be requested through support. | Support process and authority not confirmed. | Support + legal + product | Support/admin route and deletion process evidence | Privacy-sensitive, operational/admin-sensitive | Define process before final copy. |
| 30-day deletion/removal copy | Mobile privacy references 30-day removal with exceptions. | Backend deletion/retention evidence incomplete. | Legal + product | Account deletion, retention, audit, commerce evidence | Legal/compliance-sensitive, privacy-sensitive | Do not overstate until deletion/retention model is accepted. |
| Notification settings/private preview copy | Settings/private-preview behavior appears user-facing. | Delivery preference consumption not fully verified. | Product + notification owner | Push delivery and preference enforcement evidence | Privacy-sensitive | Align copy with actual enforcement or scope the claim. |
| Analytics/diagnostics disclosure | Privacy copy suggests anonymous analytics/crash behavior; `app_diagnostics` may be user-linked. | Possible mismatch; evidence incomplete. | Legal + product + diagnostics owner | `app_diagnostics` payload and linkage evidence | Privacy-sensitive | Verify payload/linkage before final disclosure. |
| Public profile/media/event visibility copy | Public-safe profile, event, share, and media visibility claims intersect public routes. | Public/private parity incomplete. | Product + legal | Public route, RLS, storage, search/feed evidence | Privacy-sensitive | Tie claims to verified visibility contract. |
| Media deletion/marketing license/user content copy | Media/user content and public display behavior intersects storage and host moderation. | License/deletion/storage behavior incomplete. | Legal + product + media owner | Bucket visibility and object deletion evidence | Privacy-sensitive, legal/compliance-sensitive | Define public content and removal terms after storage verification. |
| Ticket/reservation/claim copy | Ticket entitlement, reservation, claim, wallet, and QR copy intersects commerce authority. | Payment/order/reservation boundary incomplete. | Product + legal + commerce | Commerce/RPC/provider evidence | Revenue-sensitive | Separate ticket, reservation, claim, payment, and refund copy. |
| Community/prohibited conduct copy | Terms include prohibited conduct-style language; formal community guidelines not confirmed. | Partially supported; workflow incomplete. | Legal + trust/safety + product | Report/moderation evidence | Trust/safety-sensitive | Create reviewed policy source before launch claims. |
| Report/moderation/appeal/takedown copy | Formal report/review/appeal/takedown process not confirmed. | Not confirmed. | Trust/safety + legal + support | Report/moderation/support evidence | Trust/safety-sensitive, privacy-sensitive | Avoid process claims until workflow exists or is accepted. |
| DM/private communication policy copy | Messaging privacy, support visibility, report, archive/delete semantics unresolved. | Implementation evidence incomplete. | Product + legal + support | DM RPC/table and support visibility evidence | Privacy-sensitive | Define private communication policy only after verification. |
| Legal entity/imprint/jurisdiction/contact copy | Support contact found; legal entity, jurisdiction, imprint/impressum not confirmed. | Not confirmed. | Legal | Public legal route/copy evidence | Legal/compliance-sensitive | Requires legal owner before public launch copy freeze. |
| Data export/portability copy | Data export/portability was not confirmed. | Not confirmed. | Legal + product + support | Data export/account request implementation evidence | Privacy-sensitive | Decide stance and process before copy is final. |

## 7. Policy Copy Source-of-Truth Model

Recommended non-binding model:

- A legal owner/source document should become canonical only after legal review and product acceptance.
- App, mobile, web, dashboard, checkout, support, and FAQ copy should reference or mirror reviewed canonical policy text.
- Public web legal pages should not remain placeholder/generalized pages for launch unless that risk is explicitly accepted by the product/legal owner.
- Product copy may summarize policy only after legal/product sign-off.
- Support FAQ and support-contact copy should not promise deletion, refund, report, export, appeal, or escalation process capability that has not been accepted.
- Checkout copy should be treated as high-salience purchase-moment copy and must be consistent with terms, refund policy, host policy, and commerce implementation.
- Policy copy changes should have date/version ownership and a review trail.

This model is not final policy text.

## 8. Privacy Policy Copy Reconciliation Plan

Privacy copy needs reconciliation against:

- Collected data categories, including account/profile fields, event participation, tickets/reservations, media, notifications, diagnostics, and support requests.
- Public profile, public event, public share, public media, and public storage visibility.
- Support-mediated deletion claims and the account deletion implementation status.
- Analytics, crash reporting, diagnostics, `app_diagnostics`, user linkage, payload minimization, and retention.
- Children/minors/guardian text, which requires legal review and launch-market context.
- Support contact and support process scope.
- Service provider, law enforcement, business transfer, security, and no-sale-style statements if present.
- Profile visibility, update, delete, and public/private control claims.

PP-01 evidence is needed before final copy can safely state implementation-backed behavior. Legal review is required before any compliance or legal-position language is treated as approved.

## 9. Terms of Service / User Agreement Copy Reconciliation Plan

Terms/user agreement copy needs reconciliation against:

- Age, minors, and guardian consent assumptions.
- Account responsibility, account termination, and suspension behavior.
- User content license, public media, marketing use, and content removal semantics.
- Prohibited conduct, safety, moderation, and report workflows.
- Host event accuracy, local-law responsibility, venue obligations, and event cancellation obligations.
- Ticket, reservation, claim, wallet, QR, and refund/dispute semantics.
- Platform liability/disclaimer-style copy, which requires legal review.
- Support contact and escalation language.

This pack does not provide legal wording.

## 10. Account Deletion / Data Request Copy Reconciliation Plan

Known copy risks:

- Mobile terms mention account deletion through settings.
- Mobile privacy mentions support-mediated deletion.
- Mobile privacy references 30-day removal with legal/security/business exceptions.
- Data export/portability was not confirmed.

Required reconciliation:

- Decide whether account deletion is self-service, support-mediated, both, or deferred.
- Define what deletion means for profiles, personas, events, venues, media/storage, messages, notifications, push tokens, diagnostics, audit logs, tickets, reservations, claims, commerce records, reports, and support records.
- Distinguish product deletion from legal/audit retention.
- Distinguish deletion, redaction, archive, hide, and suppression.
- Decide whether data export/portability is supported, support-mediated, or not yet offered.

No account deletion promise should be treated as final until product/legal owners accept the model and implementation evidence is available.

## 11. Data Retention / Redaction / Export Copy Reconciliation Plan

Retention/redaction/export copy must account for:

- Profiles and personas, including public identity and host identity transfer retention.
- Media/storage records and objects, including public URLs and signed URLs.
- Messages and private conversations, including archive/delete semantics.
- Notifications, reminders, push tokens, and notification history.
- Diagnostics and `app_diagnostics`.
- Audit logs, transfer logs, admin actions, moderation logs, and commerce logs.
- Commerce/payment/order/ticket/reservation/claim records.
- Report/moderation evidence and appeal/takedown records if adopted.

Product deletion, legal retention, audit retention, public suppression, and support visibility are separate concepts. Copy must not collapse them into one promise.

## 12. Payments / Refunds / Disputes Copy Reconciliation Plan

Current audit signals:

- Checkout says all ticket sales are final with no refunds/exchanges.
- Terms say ticket purchases are subject to host refund policy.
- Refund, dispute, chargeback, provider, receipt, and webhook behavior are not fully confirmed.

Required reconciliation:

- Decide canonical refund policy and purchase-moment copy.
- Decide whether host refund policy exists, how it is expressed, and who enforces it.
- Separate ticket cancellation, event cancellation, reservation cancellation, refund, dispute, and chargeback.
- Decide receipt/invoice semantics and whether notifications or emails are receipts.
- Identify payment provider/deployment facts through PP-01 before provider/payment claims are finalized.

PP-02 does not decide the refund policy and does not provide final payment terms.

## 13. Tickets / Reservations / Claims Copy Reconciliation Plan

Ticket/reservation/claim copy must distinguish:

- Ticket as entry entitlement.
- Commerce order/payment state as financial state.
- Reservation as booth/table/request state unless product explicitly defines paid reservations.
- Gift/claim/transfer as entitlement transfer or pending claim, not necessarily new payment.
- Wallet/QR/check-in as entitlement verification, not refund or payment authority.
- Venue reservation terms and availability/capacity claims.

Final copy depends on PP-01 and PP-04 evidence for active purchase/order/reservation/claim flows.

## 14. Community Rules / Abuse / Reporting / Moderation Copy Reconciliation Plan

Community and safety copy needs a product/legal decision for:

- Prohibited conduct categories such as harassment, threats, illegal content, hateful content, violent content, spam, exploitation, hacking, fake events, and abuse.
- Block/unblock and mute behavior.
- Media moderation, public content takedown, host moderation, and support/admin moderation.
- Report submission, review, resolution, appeal, restoration, and takedown process.
- Reporter/reported-user privacy and report evidence handling.

Formal report/review/appeal/takedown workflows were not confirmed in prior audits. Copy should not promise a process until the workflow and owner are accepted.

## 15. Messaging / Private Communication Safety Copy Reconciliation Plan

Messaging/private communication copy should not overstate:

- DM reporting or support review unless the workflow is verified.
- Support/admin visibility into private conversations unless explicitly found and accepted.
- Private message archive/delete semantics.
- Notification preview privacy and deep-link behavior.
- Block/mute effects on messaging and notifications.

DM safety and privacy copy depends on PP-01 and PP-10 evidence.

## 16. Media / Public Content / Storage Copy Reconciliation Plan

Media/public content copy must reconcile:

- Public profile/avatar/media visibility.
- User-uploaded event media, venue media, public highlights, and memory wall behavior.
- Host featuring or moderating uploaded media.
- Uploader delete/hide controls versus host moderation versus ops/admin moderation.
- User content license and marketing/promotional use.
- Public URL, signed URL, storage object deletion, and database record deletion.

Storage public/private status and object deletion behavior require PP-01 and PP-09 evidence before public copy is finalized.

## 17. Notifications / Push / Diagnostics / Analytics Disclosure Copy Reconciliation Plan

Notification and diagnostics copy must reconcile:

- Push notification categories and opt-in/opt-out behavior.
- Notification preference enforcement by delivery systems.
- Private content preview masking.
- Notification history, unread counts, reminders, and push token storage/deletion.
- Analytics/crash reporting claims.
- `app_diagnostics`, remote diagnostics, payload content, user linkage, support/admin access, retention, and redaction.

If diagnostics are user-linked or include sensitive payloads, anonymous analytics/crash copy must be reviewed and scoped. PP-01 and PP-06 must provide evidence before final disclosure.

## 18. Public Web / Share / Profile / Event Visibility Copy Reconciliation Plan

Public web copy must match:

- Public profile, event, venue, share, claim, ticket verification, and support/FAQ route behavior.
- Public/private visibility rules for groups, invite-only events, hidden/deleted/archived/moderated content, search, feed, and share pages.
- Public-safe field lists for profiles, personas, events, venues, media, and claims.
- Web legal route deployment and current web copy status.
- Marketing claims that may imply public availability, verified hosts, organizer panels, or app behavior.

PP-02 should treat web legal placeholder pages as non-canonical until legal/product review.

## 19. Host / Venue / Business Policy Copy Reconciliation Plan

Host/venue/business copy must reconcile:

- Host responsibility for event accuracy, safety, cancellation, refund handling, and local-law obligations.
- Venue reservation terms and venue/business listing responsibility.
- Official business badge or verified organizer claims.
- Organizer web panel claims versus actual dashboard/mobile authority.
- Host moderation rights for media and event-scoped content.
- Host visibility into tickets, reservations, and participant data.
- Support liability and escalation boundaries.

Host authority should not be overstated beyond backend/event-scoped evidence.

## 20. Support / Ops / Admin Process Copy Reconciliation Plan

Support/admin copy must not treat contact availability as process proof.

Required reconciliation:

- Support email and public support routes.
- Deletion request processing.
- Data export request processing.
- Refund/dispute/report/takedown/appeal processing.
- Support/admin private-data visibility.
- Support/admin mutation authority.
- Auditability of support/admin actions.
- Manual override process boundaries.

Support read visibility is not deletion, refund, dispute, report resolution, or admin mutation authority.

## 21. Legal Contact / Imprint / Jurisdiction / Escalation Copy Reconciliation Plan

Prior audits found support contact evidence, but did not confirm:

- Legal entity.
- Jurisdiction/governing law.
- Imprint/impressum.
- Legal contact distinct from support contact.
- Formal complaint channel.
- Escalation process.
- Appeal/takedown channel.
- Launch-market-specific legal requirements.

This pack does not invent legal entity, jurisdiction, or imprint language. Legal owner review is required.

## 22. Policy-to-Implementation Mismatch Register

| Copy area | Current signal | Implementation evidence status | Mismatch type | Risk | Required owner | Recommended next action |
|---|---|---|---|---|---|---|
| Account deletion through settings | Mobile terms mention settings-based deletion. | Self-service implementation not confirmed. | Policy ahead of implementation | Privacy-sensitive | Product + legal | Verify implementation or revise promise through legal/product review. |
| Support deletion + 30-day removal | Mobile privacy references support deletion and 30-day removal. | Support process and retention behavior incomplete. | Policy ahead of implementation | Privacy-sensitive, legal/compliance-sensitive | Legal + support + product | Define deletion/retention model before final copy. |
| Refund host policy vs all-sales-final checkout | Checkout says final/no refunds; terms reference host refund policy. | Refund/provider behavior not confirmed. | Contradiction / possible mismatch | Revenue-sensitive | Product + legal + commerce | Decide canonical refund/cancellation copy. |
| Notification preferences/private preview | User-facing settings/private-preview copy exists. | Delivery preference consumption incomplete. | Policy ahead of implementation | Privacy-sensitive | Product + notifications | Verify enforcement or scope copy. |
| Anonymous analytics/crash vs diagnostics | Privacy copy may imply anonymous analytics/crash. | `app_diagnostics` may be user-linked. | Possible mismatch | Privacy-sensitive | Legal + diagnostics | Verify payload/linkage and update disclosure plan. |
| Media deletion vs storage behavior | Deletion/hide/moderation copy intersects media storage. | DB row vs storage object behavior incomplete. | Unknown / Needs verification | Privacy-sensitive | Product + legal + media | Verify storage lifecycle before final removal copy. |
| Report/safety text without formal workflow | Prohibited conduct exists; formal report/review/appeal not confirmed. | Workflow not confirmed. | Policy ahead of implementation | Trust/safety-sensitive | Trust/safety + legal | Define report/moderation process before process promises. |
| Web legal placeholders | Web privacy/terms are placeholder/generalized. | Launch route/copy status incomplete. | Policy not launch-ready | Legal/compliance-sensitive | Legal + web/product | Replace or explicitly accept risk after review. |
| Legal identity/imprint absence | Legal entity/imprint/jurisdiction not confirmed. | Not confirmed. | Missing policy/legal source | Legal/compliance-sensitive | Legal | Assign legal owner and source-of-truth decision. |

## 23. Implementation-without-Policy Register

| Technical/product surface | Missing or unclear public policy | Risk | Owner | Recommended next action |
|---|---|---|---|---|
| `app_diagnostics` | Diagnostics, payload, linkage, support visibility, retention disclosure. | Privacy-sensitive | Product + legal + diagnostics | Map payload and disclosure after PP-01/PP-06 evidence. |
| DM archive/delete | Private message lifecycle and user-visible deletion/archive semantics. | Privacy-sensitive | Product + legal | Include in PP-10 policy requirements. |
| Block/unblock/social graph side effects | Visibility, notifications, DM, search/feed, and discovery side effects. | Trust/safety-sensitive | Product + trust/safety | Decide block/mute policy wording with PP-07. |
| Host identity transfer/persona retention | Public identity transfer, audit retention, and persona field retention. | Privacy-sensitive, operational-sensitive | Product + legal + ops | Include in deletion/retention and host policy review. |
| Public claim/share/check-in links | Public-safe details, token possession, entitlement checks, verification visibility. | Privacy-sensitive, revenue-sensitive | Product + legal | Map to public visibility and commerce packs. |
| Public storage buckets | Public URL behavior, media deletion, license, caching, and object retention. | Privacy-sensitive | Product + legal + media | Verify storage behavior before final media policy. |
| Ops/admin/support tools | Support visibility, manual overrides, deletion/refund/report/export process. | Operational/admin-sensitive | Support + legal + ops | Define process copy and auditability with PP-08. |
| Commerce/provider logs | Payment provider, receipt, refund/dispute, provider payload, retention policy. | Revenue-sensitive, compliance/audit-sensitive | Commerce + legal | Include in PP-04 and legal review. |
| Check-in proof/public verification | Public verification, QR, proof retention, ticket holder visibility. | Privacy-sensitive, revenue-sensitive | Product + legal | Align with public visibility and ticket policy. |

## 24. Copy Risk Priority Matrix

| Priority candidate | Copy risks | Rationale |
|---|---|---|
| Candidate P0 | None assigned by this pack. | Current handbook evidence does not justify a P0 policy-copy classification without legal review or production verification. |
| Candidate P1 copy blocker | Web legal placeholders; account deletion/settings/support/30-day promise mismatch; refund contradiction at checkout/terms; notification preference/private-preview claims before enforcement; diagnostics/analytics disclosure if diagnostics are user-linked. | These can affect public launch trust, privacy, revenue, or legal review readiness. |
| Candidate P2 beta hardening | Formal report/moderation/appeal/takedown copy; media/storage/delete/license copy; DM/private communication policy; support/admin process wording; legal entity/imprint/jurisdiction depending launch market; public visibility copy; data export copy. | Important for beta/pre-scale or launch-market readiness, but evidence is incomplete or depends on product/legal scope. |
| Candidate P3 copy polish | Cross-links, FAQ wording, version/date formatting, redundant copy cleanup after canonical policy is selected. | Lower-risk once P1/P2 decisions are complete. |
| Unknown / Needs verification | Current deployed copy, launch markets, legal entity requirements, production behavior for deletion/export/notification/provider/reporting/storage. | Do not convert to copy changes or patch work before evidence and owners are assigned. |

## 25. PP-01 Evidence Dependencies

PP-02 needs PP-01 or follow-up verification evidence for:

- Public legal route deployment and current public web legal pages.
- Current mobile/web/dashboard deployed copy if launch copy differs from repository copy.
- Notification delivery preference enforcement and private-preview behavior.
- `app_diagnostics` payload categories, user linkage, read/write authority, and retention.
- Payment provider, refund, dispute, receipt, and webhook state.
- Public visibility route/backend behavior for profile, event, share, claim, media, feed, and search surfaces.
- Storage bucket public/private status, public URL behavior, signed URL behavior, object deletion, and DB row deletion.
- Account deletion and data export implementation evidence, if any.
- Support/admin route authority and auditability.
- Report/moderation implementation evidence, if any.

## 26. Legal Review Dependency Checklist

- Privacy policy.
- Terms of service.
- Refund, cancellation, dispute, and chargeback policy.
- Account deletion, data request, export, retention, redaction, and legal exceptions.
- Diagnostics, analytics, crash reporting, push notification, and reminder disclosures.
- User content, media license, marketing use, and content removal.
- Minors, age limits, and guardian consent.
- Prohibited conduct and community rules.
- Report, appeal, takedown, moderation, and restoration process.
- Legal entity, jurisdiction, imprint/impressum, and legal contact.
- Support, complaint, escalation, and process promises.
- Public marketing claims.

## 27. Product Decision Dependency Checklist

- Canonical refund policy.
- Account deletion model.
- Support process model.
- Notification preference and private-preview semantics.
- Diagnostics disclosure and retention model.
- Media deletion/storage behavior.
- Public visibility promises.
- Report/moderation workflow.
- Host/venue responsibility model.
- Data export/portability stance.
- Launch market and legal-entity assumptions.

## 28. Public Launch Copy Freeze Criteria

Public launch copy should not be considered freeze-ready until:

- A canonical reviewed policy source is selected.
- Web, mobile, checkout, support, and product copy are reconciled against that source.
- Placeholder legal pages are removed or explicitly accepted by legal/product owners.
- Refund copy is consistent across checkout, terms, support, host policy, and commerce behavior.
- Deletion copy matches implementation and support process.
- Notification and diagnostics claims match behavior or are clearly scoped.
- Support processes are documented and owned.
- Legal identity, imprint/impressum, jurisdiction, and contact decisions are made or explicitly deferred by legal owner.
- No compliance or launch-readiness claim is made unless the responsible owner approves it.

This pack does not claim these criteria are met.

## 29. Dependency Map to Later Patch Plan Groups

| Later pack | PP-02 dependency |
|---|---|
| PP-03 Account Deletion / Data Request Decision Pack | Deletion, support request, 30-day removal, retention exception, and export copy constraints. |
| PP-04 Commerce/Refund/Payment Contract Pack | Refund, cancellation, dispute, receipt, provider, host refund policy, ticket/reservation/claim copy constraints. |
| PP-05 Public Visibility Suppression Pack | Public profile/event/share/media/search/feed copy constraints and public-safe promises. |
| PP-06 Notification/Diagnostics Privacy Pack | Notification preference, private preview, push token, diagnostics, analytics/crash disclosure constraints. |
| PP-07 Abuse/Moderation Workflow Pack | Community rules, report, review, appeal, takedown, moderation, evidence privacy copy constraints. |
| PP-08 Ops/Admin Support Auditability Pack | Support process, private-data visibility, manual override, deletion/refund/report/export handling copy constraints. |
| PP-09 Media Storage Lifecycle Pack | Public media, user content license, storage, deletion, hide/unhide, moderation, URL behavior copy constraints. |
| PP-10 Messaging Privacy Lifecycle Pack | DM privacy, notification preview, archive/delete, report/support visibility, block/mute copy constraints. |

## 30. PP-02 Output Artifacts

Recommended artifacts after PP-02 execution, not created by this pack:

- `PublicPolicyCopyReviewReport.md`
- `LegalPolicySourceOfTruthDecision.md`
- `RefundCopyReconciliationReport.md`
- `AccountDeletionCopyReconciliationReport.md`
- `NotificationDiagnosticsDisclosureReview.md`
- `WebLegalRoutesCopyReview.md`
- `SupportProcessCopyReview.md`
- `PublicLaunchCopyFreezeChecklist.md`

## 31. Verification Execution Preconditions

Before executing PP-02:

- Legal and product owners are assigned.
- Launch market and legal entity assumptions are defined or explicitly marked Unknown / Needs verification.
- Current deployed copy and source copy are collected.
- PP-01 evidence is available where implementation-backed claims are needed.
- Draft pack language is not treated as final published policy.
- No legal claims are made without legal review.
- No secrets, private user data, private provider payloads, or raw personal data are included in review artifacts.

## 32. Explicitly Blocked Actions

PP-02 blocks:

- Final legal drafting.
- Legal compliance claims.
- Publication of policy pages.
- App, web, mobile, dashboard, backend, or Supabase source changes.
- Production access.
- Supabase CLI or SQL execution.
- User data collection or export into docs.
- Support process commitments without owner sign-off.
- Launch-readiness claims.
- Immediate patch authorization.

## 33. Unknown / Needs Verification Items

- Which copy source is canonical today.
- Whether web legal pages are public in production and what text they currently show.
- Whether mobile terms/privacy are the accepted legal source or draft app copy.
- Whether account deletion is actually self-service, support-mediated, both, or absent.
- Whether 30-day removal copy is accepted by legal/product owners.
- Whether data export/portability exists or is planned.
- Whether refund/dispute/chargeback/provider behavior exists in production.
- Whether notification preferences and private previews are enforced by delivery.
- Whether diagnostics are anonymous, pseudonymous, user-linked, or mixed.
- Whether support is ready for deletion, export, refund, dispute, report, takedown, and appeal requests.
- Whether public storage URLs, cached media, and signed URLs match media deletion copy.
- Whether formal report/review/appeal/takedown process exists.
- Which legal entity, jurisdiction, imprint/impressum, and legal contact apply.

## 34. Acceptance Criteria for PP-02 Completion

PP-02 is complete only when:

- The policy surface inventory is confirmed.
- Current copy sources are identified or marked Unknown / Needs verification.
- The mismatch register is reviewed by assigned product/legal owners.
- The implementation-without-policy register is reviewed by assigned product/legal owners.
- Legal review dependencies are assigned.
- Product decisions are assigned.
- PP-01 evidence dependencies are linked to copy claims.
- A public launch copy freeze checklist exists.
- No final legal text is treated as approved unless the legal owner confirms it.

## 35. Recommended Follow-Up Reports

Recommended follow-up reports after execution:

- Public Policy Copy Review Report.
- Legal Policy Source-of-Truth Decision.
- Account Deletion and Data Request Copy Review.
- Refund and Payment Copy Reconciliation Report.
- Notification and Diagnostics Disclosure Review.
- Public Web Legal Route Verification Report.
- Support Process Copy Review.
- Public Launch Copy Freeze Checklist.

## 36. Non-Goals

- No code changes.
- No SQL or migrations.
- No production execution.
- No legal advice.
- No compliance claim.
- No launch readiness claim.
- No final legal copy.
- No immediate patch authorization.
- No source-code re-audit.

## 37. Open Questions

- Which copy source is canonical?
- Are web legal pages public, and are they launch-facing?
- What legal entity, jurisdiction, imprint/impressum, and legal contact are required?
- What launch markets are targeted?
- Is account deletion self-service, support-mediated, both, or deferred?
- Is the 30-day deletion/removal promise accepted?
- What refund policy is canonical?
- Are notification preferences and private previews enforced?
- Are diagnostics anonymous or user-linked?
- Is support ready for deletion, export, refund, dispute, report, takedown, and appeal requests?
- What formal report/appeal/takedown process exists?

## 38. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `08_PatchPlans/PP02LegalPublicPolicyCopyPack.md` was created/modified.

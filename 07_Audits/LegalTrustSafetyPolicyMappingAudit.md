# Legal / Trust & Safety Policy Mapping Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false
- Legal status: Engineering audit only; not legal advice

## 2. Purpose

This audit maps user-facing legal, privacy, terms, refund, trust, safety, support, and policy copy against the technical contract evidence captured in the JoinFolk handbook and targeted local source inspection.

This is an engineering policy-mapping audit only. It is not legal advice, not a legal compliance assessment, not an implementation plan, and not authorization to change product behavior, backend authority, RLS, RPCs, storage, auth, app code, dashboard code, web code, or Supabase trees.

## 3. Audit Scope

In scope:

- Mobile privacy policy, mobile terms, mobile settings/legal/support copy, web privacy/terms/support pages, public footer legal links, checkout refund copy, notification preference copy, diagnostics/analytics disclosure evidence, and support contact copy.
- Mapping those policy signals to prior audits covering privacy, payments, abuse/moderation, diagnostics, ops/admin, messaging, notifications, public web/share, profile/persona, commerce, venue, lifecycle, media, social graph, search/discovery, and staff/host authority.
- Identifying policy ahead of implementation, implementation ahead of policy, and areas requiring legal/product review.

Out of scope:

- Legal advice or legal compliance claims.
- Drafting final legal terms.
- Backend, frontend, dashboard, mobile, web, Supabase, SQL, migration, RLS, RPC, storage, auth, payment-provider, or production changes.

## 4. Legal / Trust & Safety Policy Mapping Summary

JoinFolk has meaningful user-facing policy signals, but they are split and uneven:

- Mobile privacy policy has substantive copy covering collected account/profile/event/media/device/analytics data, public profile visibility, event/media visibility, support-mediated account deletion, 30-day personal-data removal with legal/business exceptions, analytics/crash reports, children under 13, and support contact.
- Mobile terms include age/guardian-consent language, account responsibility, prohibited conduct, host responsibility for event accuracy/local-law compliance, venue reservation responsibility, liability-style copy, suspension/termination language, and a statement that users may delete their account through app settings.
- Mobile checkout copy states that all ticket sales are final with no refunds or exchanges, while mobile terms say ticket purchases are subject to the refund policy established by each event host. This is a policy-copy mismatch unless product/legal intentionally distinguishes generic terms from checkout-time sales policy.
- Mobile settings expose notification preferences and a private-content-preview toggle. The code contains a TODO noting the delivery pipeline does not yet consume these preferences, so the settings copy may be ahead of delivery implementation.
- Mobile privacy copy says analytics/crash-report data is anonymous and not linked to personal identity, while local analytics helper code is development-console/TODO provider scaffolding and remote diagnostics writes to `app_diagnostics` with user-context evidence from prior audits. This needs product/legal and implementation reconciliation.
- Web public privacy and terms pages exist, but the inspected page bodies are placeholders or generalized localized stubs. Web support page exposes `support@join-folk.com` and FAQ copy, including a claim that promotional pages and the real app are intentionally separated for data security and speed.
- Formal community guidelines, report policy, appeal/takedown policy, refund/dispute policy, data export policy, legal imprint/impressum, and jurisdiction/legal entity details were not confirmed in targeted source inspection.

No legal compliance claim is made. Policy text is treated as product/legal promise evidence, not proof of implementation.

## 5. Policy Surface Inventory Matrix

| Policy surface / domain | User-facing promise or policy signal observed | Technical evidence mapped | Technical backing status | Expected owner | Risk class | Recommendation |
| --- | --- | --- | --- | --- | --- | --- |
| Mobile privacy policy | Data collection, public profile/media visibility, support-mediated deletion, 30-day removal, analytics/crash reports, support contact | Privacy, Profile, Media, Notification, Diagnostics audits | Partially supported; several capabilities unconfirmed | Product policy + legal review + backend | Legal/compliance-sensitive, privacy-sensitive | Reconcile policy and implementation |
| Mobile terms | Age, account responsibility, prohibited conduct, host/event responsibility, venue responsibility, suspension/termination, account deletion through settings | Abuse, Staff/Host, Venue, Privacy audits | Partially supported; account deletion through settings not confirmed | Product policy + legal review | Legal/compliance-sensitive, trust/safety-sensitive | Verify implementation |
| Web privacy page | Privacy/KVKK placeholder/generalized body | Web source only; privacy audit evidence | Policy ahead of final copy | Legal review + web | Legal/compliance-sensitive | Verify policy copy |
| Web terms page | Terms placeholder body | Web source only; terms mapping evidence | Policy ahead of final copy | Legal review + web | Legal/compliance-sensitive | Verify policy copy |
| Web support page | Support email and FAQ; separation of marketing/app for data security/speed | Web source; ops/admin/support audits | Partially supported as copy; process evidence incomplete | Support process + product | Operational/admin-sensitive | Document contract |
| Ticket checkout | All ticket sales final, no refunds/exchanges | Payments audit; commerce source evidence | Policy may conflict with host-refund terms; refund implementation not confirmed | Product policy + legal review + commerce | Revenue-sensitive | Reconcile policy and implementation |
| Terms refund statement | Ticket purchases subject to each host refund policy | Payments audit; no host refund-policy model confirmed | Policy ahead of implementation or mismatch | Legal review + product + commerce | Revenue-sensitive | Needs product decision |
| Notification settings | User chooses push categories; private previews can be masked | Notification audit; settings code TODO says delivery pipeline does not yet consume preferences | Possible mismatch / policy ahead of implementation | Mobile UI + backend delivery | Privacy-sensitive | Verify implementation |
| Analytics/diagnostics | Privacy copy says anonymous usage/crash data not linked to identity | Analytics helper TODO provider; remote diagnostics/app_diagnostics evidence | Possible mismatch / implementation ahead of policy | Product policy + diagnostics | Privacy-sensitive | Reconcile |
| Public profile/media | Public profile info visible; event photos visible to participants; media can be deleted by uploader | Profile, Media, Privacy, Public Web audits | Partially supported; storage/delete semantics incomplete | Product policy + backend/storage | Privacy-sensitive | Verify implementation |
| Report/moderation/safety | Prohibited behavior in terms; moderation controls in product surfaces | Abuse audit: formal report/review not confirmed | Implementation/policy incomplete | Trust/safety + legal review | Trust/safety-sensitive | Needs product decision |
| Account/data requests | Support-mediated deletion in privacy; delete through settings in terms | Privacy audit: account deletion implementation not confirmed | Policy ahead of implementation | Product policy + backend + support | Legal/compliance-sensitive | Verify implementation |
| Data export/portability | Not confirmed | Privacy audit: export not confirmed | Not confirmed | Legal review + product | Legal/compliance-sensitive | Needs product decision |
| Legal imprint/contact | Support email found; imprint/jurisdiction/legal entity not confirmed | Web/mobile source only | Incomplete | Legal review | Legal/compliance-sensitive | Unknown / Needs verification |

## 6. Role Vocabulary and Policy Boundary

- User: ordinary product account holder.
- Host: event organizer role; host policy obligations do not equal ops/admin authority.
- Venue/business owner: business/venue operator; separate from event host and ordinary user.
- Participant: event attendee or social participant.
- Ticket holder: entitlement holder; not payment/refund authority by itself.
- Reservation owner: user who made a reservation; reservation status is not automatically payment status.
- Reporter: user submitting an abuse/safety report, if product supports it.
- Reported user: user or actor targeted by a report.
- Moderator: host/support/ops actor with accepted moderation authority, if backend enforces it.
- Support: support process or support-facing user contact; support promises require process evidence.
- Ops/admin: internal privileged actor; ops/admin authority must be backend-gated and auditable.
- Payment provider: external payment system, if deployed and verified.
- Legal reviewer: qualified legal counsel or legal owner; separate from engineering audit.
- Public visitor: anonymous or unauthenticated public web/app viewer.

This audit is not legal advice. Policy text is not implementation proof. Implementation evidence is not legal compliance proof. Legal review is separate from engineering review. User-facing deletion, refund, report, safety, privacy, and support promises must not exceed verified capability unless product/legal explicitly accepts that launch risk.

## 7. Privacy Policy Mapping Assessment

Observed mobile privacy policy claims include:

- Account collection: name, email address, username, date of birth, and country.
- App-use collection: profile photos, event activity, media uploads, device information, and usage analytics.
- Use purposes: app operation, profile display, event discovery, ticketing, reservations, account notifications, platform safety/security, and legal obligations.
- Sharing/publicity: public profile name/username/avatar visible to other users; event activity and uploaded photos may be visible to event participants; no sale of personal information; sharing with service providers, law enforcement when required, and business partners during merger/acquisition.
- Storage/security: Supabase database hosting/authentication and a general encryption/security statement.
- Controls: profile visibility settings, profile update/delete, support-mediated account/data deletion.
- Media: uploaded media visible to event participants, hosts may feature uploads, and uploaders may delete media through the app.
- Analytics: anonymous usage data such as screen views, feature usage, and crash reports, not linked to personal identity.
- Children: not intended for children under 13; personal information from children under 13 will be deleted promptly if discovered.
- Retention: account data retained while active; if account is deleted, personal data removed within 30 days except law/business-purpose retention such as transaction records.

Technical mapping:

- Profile visibility and media visibility exist as product surfaces, but profile/user profile production RLS coverage is incomplete.
- Account deletion implementation was not confirmed.
- Media delete/hide exists in prior evidence, but storage-object deletion and public URL invalidation remain Needs verification.
- Diagnostics/analytics language needs reconciliation with `app_diagnostics` and remote diagnostics evidence.
- Support contact exists, but support deletion workflow/process evidence is not confirmed.

Status: Partially supported, with policy ahead of implementation for account deletion and possible mismatch for analytics/diagnostics.

## 8. Account Deletion / Data Request Policy Mapping

Observed policy signals:

- Mobile privacy policy says users can request deletion of account and associated data by contacting `support@join-folk.com`.
- Mobile terms say users may delete their account at any time through app settings.
- Mobile privacy policy says personal data will be removed within 30 days after account deletion, subject to legal/business retention exceptions.

Technical evidence:

- Privacy/Data Retention audit did not confirm self-service account deletion, backend erasure, export, portability, or full deletion cascade behavior.
- Mobile settings evidence confirmed profile editing, support email, notification preferences, and visibility controls; account deletion through settings was not confirmed in targeted inspection.

Mapping status: Policy ahead of implementation or possible policy/copy mismatch.

Required decision: whether deletion is self-service, support-mediated, both, or not yet launch-ready.

## 9. Data Retention / Redaction / Archival Policy Mapping

Observed policy signals:

- 30-day personal-data removal after account deletion, with retention exceptions for law and legitimate business purposes such as transaction records.
- Children under 13 data deletion promptly if discovered.
- No detailed retention schedule for diagnostics, messages, notifications, media, report evidence, audit logs, payment/order records, tickets, reservations, or public content was confirmed.

Technical mapping:

- Privacy audit flagged account deletion, profile/persona redaction, storage vs DB deletion, diagnostics retention, audit-log retention, commerce/legal retention, public suppression parity, and data export as unresolved.
- Diagnostics audit flagged `app_diagnostics` payload retention/read access as unresolved.
- Payments audit flagged financial retention and provider payload policy as unresolved.
- Abuse audit flagged report evidence retention/redaction/appeal as unresolved.

Status: broad policy promise exists, detailed retention/redaction contract remains Unknown / Needs verification.

## 10. Terms of Service / User Agreement Mapping

Observed mobile terms include:

- Acceptance of terms and updates by continued use.
- Minimum age 13; under-18 guardian consent representation.
- Account credential responsibility, accurate registration/profile information, no impersonation.
- User content license for app and related marketing materials.
- Prohibited behavior: harassment, threats, illegal/hateful/violent content, spam, exploitation/hacking/reverse engineering, unauthorized commercial use, fake events/misleading listings.
- Host responsibility for event accuracy, fulfillment, and local-law compliance.
- JoinFolk disclaims being event organizer and does not guarantee event quality, safety, or occurrence.
- Venue reservations subject to venue terms/availability.
- Suspension/termination for violations or at platform discretion.
- Support contact for questions.

Technical mapping:

- Event host authority, venue authority, content/moderation, report/review, and account termination workflows are only partially mapped technically.
- Formal suspension/ban implementation was not confirmed in abuse/ops audits.
- User content marketing-license implications should be legally reviewed against media/public-storage behavior and deletion policy.

Status: user agreement copy exists in mobile; final legal review and implementation mapping are incomplete.

## 11. Payments / Refunds / Disputes Policy Mapping

Observed policy signals:

- Mobile terms: ticket purchases are subject to the refund policy established by each event host.
- Mobile checkout/ticket locale: all ticket sales are final; no refunds or exchanges.
- Venue terms: reservations are subject to venue terms and availability; JoinFolk is not responsible for reservation fulfillment, venue conditions, or user/venue disputes.

Technical mapping:

- Payments audit did not confirm refund implementation, dispute/chargeback implementation, active external payment provider/webhook deployment, or support revenue tooling.
- Commerce source surfaces include order statuses that can represent refunded or canceled states, but status vocabulary is not refund/dispute implementation proof.
- Refund and cancellation are separate; reservation cancellation is not refund.

Status: policy mismatch risk. The host-refund-policy text and all-sales-final checkout notice need product/legal reconciliation.

## 12. Tickets / Reservations / Claims Policy Mapping

Observed policy signals:

- Support FAQ distinguishes ticket as an individual entry right and reservation as a specific booth/table request.
- Checkout copy says ticket goes to wallet instantly with QR code; gift ticket is added to wallet for sharing.
- Terms say venue reservations are subject to venue terms and availability.

Technical mapping:

- Commerce and Public Web audits identify tickets, claims, public claim links, reservations, QR/check-in, and wallet surfaces as revenue/privacy-sensitive.
- `tickets` and `event_ticket_claims_v1` had RLS enabled with zero direct policies in prior evidence and likely depend on RPC/default-deny assumptions.
- Public claim/share routes must not expose private payment state.

Status: partially supported; entitlement, payment, reservation, claim, and refund policy boundaries require formal product/legal copy.

## 13. Community Rules / Abuse / Reporting / Moderation Policy Mapping

Observed policy signals:

- Mobile terms prohibit harassment/threats, illegal/hateful/violent content, spam, exploitation/hacking/reverse engineering, unauthorized commercial use, fake events, and misleading listings.
- Product source includes block/unblock, host moderation, owner-hide/delete, and moderation UI strings.

Technical mapping:

- Abuse audit did not confirm a formal report submission/review/resolution system.
- Messaging audit did not confirm private message abuse reporting or support/admin DM visibility.
- Diagnostics audit did not confirm complete moderation auditability.
- Public/feed/search suppression after moderation remains a policy/backend parity question.

Status: community/safety rules exist, but formal report/review/appeal/takedown policy and implementation are Unknown / Needs verification.

## 14. Messaging / Private Communication Safety Policy Mapping

Observed policy signals:

- General harassment/spam/unauthorized communication prohibitions apply to messaging by implication.
- No dedicated private messaging policy, message report policy, or support/private-message review policy was confirmed.

Technical mapping:

- Messaging audit found DM send/read/list/archive/delete surfaces and blocked/relationship-required error behavior.
- Support/admin private conversation viewer was not confirmed.
- Notification previews and deep links remain privacy-sensitive.

Status: implementation ahead of explicit policy in some DM areas; private communication safety policy remains Needs verification.

## 15. Media / Public Content / Storage Policy Mapping

Observed policy signals:

- Privacy policy says profile info is visible, event activity/photos may be visible to participants, hosts may feature uploaded media, and users may delete uploaded media.
- Terms grant a license for user content in the app and related marketing materials and require user content not to violate laws or third-party rights.

Technical mapping:

- Media audit found owner hide/unhide/delete and host moderation concepts, but DB deletion vs storage deletion needs verification.
- Public Web audit confirmed public buckets for avatars, `venue-media`, and `venue-posters`; other media buckets require verification.
- Public URL/signed URL behavior must not be conflated with table visibility or deletion rights.

Status: partially supported; storage/deletion/marketing-license/public media semantics need legal/product review.

## 16. Notifications / Push / Diagnostics / Analytics Disclosure Mapping

Observed policy signals:

- Mobile privacy policy references important account notifications and anonymous usage analytics/crash reports not linked to personal identity.
- Mobile settings exposes push categories and private-content-preview masking.
- Settings source comment says server-side push delivery pipeline does not yet consume notification preferences.

Technical mapping:

- Notification audit confirmed notification/push-token surfaces and high-level RLS for some tables, but delivery/deployment and payload correctness remain incomplete.
- Diagnostics audit found mobile remote diagnostics inserts into `app_diagnostics`; production RLS/policy and retention are incomplete.
- Analytics helper is currently development console logging with TODO provider integration, which is not the same as deployed third-party analytics.

Status: possible mismatch. Disclosure and implementation need reconciliation before relying on privacy-copy statements.

## 17. Public Web / Share / Profile / Event Visibility Policy Mapping

Observed policy signals:

- Privacy policy says public profile name/username/avatar is visible and event activity/photos may be visible to event participants.
- Web support FAQ says promotional pages and the real application world are intentionally separated for data security and fast experience.
- Public web has privacy/terms/support links in footer.

Technical mapping:

- Public Web and Search audits identified public event, venue, claim, profile/avatar, relic/highlight, media, and feed/search surfaces.
- Public routes and direct reads rely on backend/RLS/storage correctness.
- Public/private visibility parity across feed, detail, share, media, and search remains Needs verification.

Status: partially supported, with backend parity verification required.

## 18. Host / Venue / Business Policy Mapping

Observed policy signals:

- Terms state hosts are responsible for event information, event promises, and local-law compliance.
- Terms state JoinFolk is not event organizer and does not guarantee event quality, safety, or occurrence.
- Terms state venue reservations are subject to venue terms and availability.
- Web support FAQ describes organizers, clubs, and venue assistants using the web panel for desktop operations.
- Public marketing copy references official business badge and visibility/authority for venues.

Technical mapping:

- Staff/Host and Venue audits separate host authority, venue/business owner authority, staff/scanner/manager roles, and ops/admin authority.
- Host authority is event-scoped; venue/business authority is separate.
- Refund, reservation fulfillment, cancellation, venue availability, and public listing obligations require policy/product decisions.

Status: partially supported; host/venue policy copy should not overstate backend authority or support liability processes.

## 19. Support / Ops / Admin Process Policy Mapping

Observed policy signals:

- Mobile privacy, mobile terms, mobile settings, and web support page all expose `support@join-folk.com`.
- Privacy policy uses support contact for account/data deletion requests.
- Web support page presents a support center/FAQ.

Technical mapping:

- Ops/Admin audit found some admin transfer authority and support/admin surfaces, but support deletion, refund/dispute, report review, private-data read, and auditability processes were not fully confirmed.
- Diagnostics audit found no dedicated diagnostics dashboard page in focused inspection.
- Support read visibility is not mutation authority.

Status: support contact exists; process evidence and auditability remain Unknown / Needs verification.

## 20. Legal Contact / Imprint / Jurisdiction / Escalation Mapping

Observed:

- Support email appears in mobile privacy, mobile terms, mobile settings, and web support page.
- Web privacy and terms pages exist.
- Legal footer links exist.

Not confirmed:

- Legal entity name.
- Jurisdiction/governing law.
- Imprint/impressum.
- Formal complaint/escalation process.
- DSA/GDPR/DSGVO/KVKK compliance statements reviewed by counsel.
- Data protection contact or officer.
- Formal appeal/takedown channel.

Status: Unknown / Needs verification. No jurisdiction or legal entity details are invented by this audit.

## 21. Policy-to-Implementation Mismatch Map

| Policy claim / copy area | Observed technical evidence | Mismatch type | Risk | Recommendation |
| --- | --- | --- | --- | --- |
| Users may delete account through settings | Account deletion through settings not confirmed | Policy ahead of implementation | Legal/compliance-sensitive | Verify implementation or revise copy through legal/product review |
| Support-mediated deletion + 30-day removal | Backend deletion/erasure not confirmed | Policy ahead of implementation | Privacy-sensitive | Define deletion process and technical owner |
| Analytics/crash data anonymous and not linked to identity | Remote diagnostics can include user-context evidence; analytics provider is TODO/local dev | Possible mismatch | Privacy-sensitive | Reconcile diagnostics/analytics disclosure |
| Notification preferences/private preview controls | Settings code notes delivery pipeline does not yet consume preferences | Policy ahead of delivery implementation | Privacy-sensitive | Verify delivery enforcement before launch claim |
| Ticket refunds follow each host policy | Checkout says all sales final/no refunds | Contradiction / possible mismatch | Revenue-sensitive | Product/legal decision on canonical refund policy |
| Users may delete uploaded media at any time | Media delete exists, but storage object deletion/public URL invalidation incomplete | Partially supported | Privacy-sensitive | Verify storage and DB deletion behavior |
| Formal safety rules imply enforcement | Report/review/appeal system not confirmed | Policy ahead of implementation | Trust/safety-sensitive | Define report/moderation process |
| Web privacy/terms pages | Page bodies are placeholders/generalized stubs | Policy copy incomplete | Legal/compliance-sensitive | Finalize legal copy before launch |

## 22. Implementation-without-Policy Map

| Technical behavior/surface | Missing or unclear policy disclosure | Risk | Recommendation |
| --- | --- | --- | --- |
| `app_diagnostics` remote diagnostics | Detailed diagnostic payload, retention, read access, and user linkage disclosure | Privacy-sensitive | Reconcile diagnostics policy |
| DM archive/delete/private messaging | Message retention, reporting, support access, notification previews | Privacy-sensitive | Add messaging policy mapping after product decision |
| Block/unblock and social visibility | Block/mute effects on messaging, discovery, notifications | Trust/safety-sensitive | Document safety behavior |
| Host identity transfer | Persona transfer, profile mirror, audit retention | Privacy-sensitive, operational-sensitive | Document transfer policy |
| Public claim/share/check-in links | Public-safe fields, token possession, verification limits | Revenue-sensitive, privacy-sensitive | Document public share/claim policy |
| Public storage buckets | Public avatar/venue media semantics and deletion behavior | Privacy-sensitive | Document media/storage policy |
| Ops/admin/support tools | Private-data visibility, manual overrides, auditability | Operational/admin-sensitive | Define support/admin process policy |
| Commerce/order/provider logs | Financial retention, receipt semantics, provider payload minimization | Revenue-sensitive | Define payments policy |

## 23. Policy Copy / Product Copy Risk Map

Potential copy risk areas:

- 30-day deletion promises without confirmed backend account deletion and erasure flow.
- Account deletion through settings without confirmed settings implementation.
- Refund text split between host-defined refunds and all-sales-final checkout copy.
- Report/safety/prohibited-conduct copy without confirmed formal report review, appeal, or takedown workflow.
- Analytics/diagnostics collection language that may not match `app_diagnostics` or future analytics provider behavior.
- Notification preference/private-preview copy while delivery pipeline consumption remains unresolved.
- Public/private profile, photo, event, and media wording without full feed/share/detail/storage parity proof.
- Support/admin action promises without confirmed process evidence and auditability.
- Web legal pages with placeholder/generalized body text.

Exact legal risk and launch acceptability require qualified legal review.

## 24. Launch-Critical Policy Dependencies

Candidate launch-blocker or release-hardening dependencies:

- Candidate P1: Account deletion/data request policy must not promise self-service or 30-day removal beyond accepted operational capability.
- Candidate P1: Refund/dispute/payment policy must be canonical across checkout, terms, host policy, support process, and provider implementation.
- Candidate P1: Web privacy/terms placeholder pages need final legal copy before public launch.
- Candidate P1: Notification preference/private-preview delivery behavior must match settings copy or be clearly scoped.
- Candidate P2: Diagnostics/analytics disclosure must match actual `app_diagnostics`, crash, analytics, and provider behavior.
- Candidate P2: Report/moderation/safety policy must map to real report/review/appeal/support workflow.
- Candidate P2: Public media/profile/event visibility copy must match backend/RLS/storage contracts.
- Candidate P2: Support/admin process promises require auditability and process evidence.

No Candidate P0 is assigned because this audit did not verify active production legal non-compliance or production exploitability.

## 25. Backend RPC / RLS Authority Evidence Map

Use prior handbook evidence only; no production connection was made for this audit.

- RLS enabled high-level evidence exists for events, `event_media`, `venue_media`, venues, reservations, `commerce_orders`, tickets, `event_ticket_claims_v1`, `notifications_v2`, `push_tokens_v1`, `user_notification_settings_v1`, and `event_staff_assignments`.
- `tickets` and `event_ticket_claims_v1` had zero direct policies and likely depend on RPC/default-deny assumptions.
- `commerce_orders` had deny-all style authenticated policy evidence.
- `app_diagnostics` production RLS/policy evidence was not fully covered.
- `profiles` and `user_profiles` production RLS/policy evidence was not fully covered.
- DM/conversation/message production evidence was not fully covered.
- Report/moderation tables/functions were not fully covered.
- `admin_execute_host_identity_transfer_v1` production evidence exists with `SECURITY DEFINER`, `search_path=public`, and `auth_is_ops()` gate.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Production SQL/RPC evidence remains stronger than local source assumptions.
- Policy text is not backend authority.

## 26. Direct Data Access / RLS Reliance Map

| Policy-sensitive domain | Technical reliance | Policy implication |
| --- | --- | --- |
| `profiles` / `user_profiles` | Mixed direct/RPC profile access; production policy coverage incomplete | Public profile/privacy copy requires verified field contract |
| Events / venues | Direct reads and RPCs; RLS/storage reliance | Public/event/host/venue claims require visibility parity |
| Event/venue media/storage | Public URLs, signed URLs, hide/delete/moderation surfaces | Media delete/publicity policy must match DB/storage behavior |
| Messages/conversations | DM RPCs; production evidence incomplete | Private communication policy must not overpromise deletion/report/support behavior |
| Notifications/push tokens | Settings/RPC/table evidence; delivery ambiguity | Notification preference copy must match backend delivery behavior |
| `app_diagnostics` | Direct client insert evidence | Analytics/diagnostics disclosure must match payload/read/retention behavior |
| Commerce/payment/provider logs | RPC/table/provider placeholder evidence | Refund/payment/receipt policy needs provider-backed authority |
| Tickets/reservations/claims | RPC-heavy entitlement/claim flows | Terms must distinguish ticket, reservation, claim, refund, and entitlement |
| Reports/abuse/moderation | Formal report tables not confirmed | Safety/report policy needs implementation/process evidence |
| Social graph/share groups | Mixed direct/RPC evidence | Block/mute/group visibility policy needs documented effects |
| Audit/support/admin logs | Partial transfer audit evidence | Support/admin process promises need traceability |

## 27. Duplicated / Split / Legacy Legal-Policy Surfaces

| Policy/source surface | Observed role | Current / legacy / unknown | Risk if still user-facing | Evidence type | Recommendation |
| --- | --- | --- | --- | --- | --- |
| Mobile privacy page | Most complete privacy policy text | Current / needs legal review | Promises may exceed backend deletion/analytics capability | User-facing local source | Reconcile |
| Mobile terms page | Most complete terms text | Current / needs legal review | Refund/account deletion/suspension claims need backing | User-facing local source | Verify policy copy |
| Web privacy page | Public privacy route | Placeholder/generalized | Public site may expose incomplete legal copy | User-facing local source | Needs legal review |
| Web terms page | Public terms route | Placeholder | Public site may expose incomplete legal copy | User-facing local source | Needs legal review |
| Web support FAQ | Support and product explanation | Current / partial | Support process claims may be underspecified | User-facing local source | Document contract |
| Checkout no-refund copy | Purchase-time policy | Current / conflicts possible | Refund expectations can conflict with terms | User-facing local source | Reconcile |
| Notification settings copy | User preference controls | Current / implementation partial | Preference promises may not affect delivery | User-facing local source + TODO | Verify implementation |
| Diagnostics/analytics copy | Privacy disclosure | Current / possible mismatch | Anonymous analytics claim may not match diagnostics behavior | User-facing + source evidence | Reconcile |

## 28. Legal-Trust-Safety-Critical Invariants

- This audit is not legal advice.
- User-facing policy text is not implementation proof.
- Implementation evidence is not legal compliance proof.
- Privacy, deletion, refund, and report promises must match verified capability or be explicitly product/legal accepted.
- Product deletion, audit retention, and legal retention are separate.
- Refund, cancellation, and dispute are separate.
- Report submission, moderation decision, and appeal are separate.
- Public marketing claims must match public/product behavior.
- Support process promises require process evidence and auditability.
- Diagnostics, analytics, and push collection need clear policy disclosure.
- Payment/provider claims require deployed provider evidence.
- Local Edge Function source is not deployment evidence.
- Public web legal pages should not remain placeholders for launch unless product/legal explicitly accepts them.
- No legal compliance claim should be made without qualified legal review.

## 29. Unknown / Needs Verification Surfaces

- Final legal review status for mobile privacy and terms.
- Whether web privacy/terms placeholders are production-facing.
- Legal entity, jurisdiction, governing law, imprint/impressum, and formal complaint/escalation process.
- Account deletion implementation and whether it is support-mediated, self-service, or both.
- Data export/portability policy and implementation.
- Analytics/diagnostics payload linkage to user identity and disclosure accuracy.
- Notification preference delivery enforcement.
- Refund/dispute/chargeback support policy and payment provider deployment.
- Host-specific refund policy data model or support process.
- Formal report submission, review, resolution, appeal, takedown, and moderation notice workflow.
- Support/admin visibility, process evidence, and auditability for deletion, refund, report, and private-data requests.
- Public/private visibility parity for public web, share routes, feed, search, profiles, events, and media.

## 30. Legal / Trust & Safety Policy Gaps / Risk Register Seeds

| Gap ID | Domain | Current issue | Expected clean legal/trust/safety policy contract | Risk | Priority candidate | Blocked by | Recommended next action |
| --- | --- | --- | --- | --- | --- | --- | --- |
| LTS-GAP-001 | Account deletion | Mobile terms and privacy copy promise deletion paths not backed by confirmed implementation | Canonical self-service/support deletion policy tied to verified backend/process behavior | Legal/compliance-sensitive, privacy-sensitive | Candidate P1 | Product/legal decision and implementation verification | Reconcile deletion copy with actual capability |
| LTS-GAP-002 | Web legal pages | Web privacy/terms bodies are placeholder/generalized | Final reviewed web privacy and terms copy | Legal/compliance-sensitive | Candidate P1 | Legal review | Replace placeholder status through legal/product process |
| LTS-GAP-003 | Refunds | Host-refund-policy terms conflict with all-sales-final checkout copy | One canonical refund/cancellation/dispute policy across terms, checkout, support, and commerce | Revenue-sensitive | Candidate P1 | Product/legal/payment decision | Decide canonical refund semantics |
| LTS-GAP-004 | Notifications | User-facing notification settings may not be consumed by delivery pipeline | Settings copy backed by backend delivery enforcement or clearly scoped UI behavior | Privacy-sensitive | Candidate P1 | Notification delivery verification | Verify preference enforcement |
| LTS-GAP-005 | Diagnostics/analytics | Privacy copy says anonymous analytics/crash data not linked to identity; diagnostics evidence may be user-scoped | Accurate disclosure for analytics, diagnostics, crash reports, payloads, retention, and linkage | Privacy-sensitive | Candidate P2 | Diagnostics payload review and legal copy review | Reconcile diagnostics disclosure |
| LTS-GAP-006 | Reporting/moderation | Safety rules exist, but formal report/review/appeal workflow not confirmed | Community/safety policy tied to report, review, takedown, appeal, and audit process | Trust/safety-sensitive | Candidate P2 | Abuse/reporting product decision | Define trust/safety policy and process |
| LTS-GAP-007 | Public media/content | Media delete/publicity/storage semantics are not fully reflected in policy | Public content, media license, storage URL, delete, and moderation policy mapped to implementation | Privacy-sensitive | Candidate P2 | Media/storage verification and legal review | Document media/public content policy |
| LTS-GAP-008 | Support/admin process | Support email exists, but support powers/auditability are unclear | Support process policy separating read, review, deletion, refund, report, and admin mutation authority | Operational/admin-sensitive | Candidate P2 | Support process design and audit evidence | Map support workflows to authority/audit contracts |
| LTS-GAP-009 | Legal identity/contact | Legal entity, jurisdiction, imprint/impressum, escalation channels not confirmed | Reviewed legal contact, entity, jurisdiction, and complaint/escalation copy | Legal/compliance-sensitive | Unknown | Legal owner input | Obtain legal review requirements |
| LTS-GAP-010 | Data export | Data export/portability not confirmed | User data request/export policy and implementation/process boundary | Legal/compliance-sensitive | Candidate P2 | Product/legal decision | Define data export policy before launch commitment |

## 31. Product / Legal Decisions Required

- Which privacy/terms copy is canonical: mobile, web, or a separate reviewed legal source?
- Is account deletion self-service, support-mediated, both, or not launch-ready?
- Is the 30-day removal promise accepted and technically supported?
- What is the canonical refund policy: host-defined, all-sales-final, event-specific, or support-mediated?
- Are disputes/chargebacks handled through a provider process, support process, or not yet supported?
- What formal report, moderation, takedown, appeal, and safety-review processes exist?
- What diagnostics, analytics, crash-reporting, and push-token disclosures are legally required for launch?
- What web legal entity, jurisdiction, imprint/impressum, and complaint/escalation text is required?
- Which support/admin actions require process-level auditability?

## 32. Recommended Next Audits

- Release Readiness / Production Hardening Gap Register.
- Data Export / Account Deletion Implementation Gap Audit.
- Public Launch Policy Copy Review.

## 33. Non-Goals

- No legal advice.
- No legal compliance determination.
- No application, dashboard, mobile, web, or Supabase code changes.
- No SQL, migrations, RLS, RPC, storage, auth, payment-provider, or production changes.
- No production connection, Supabase CLI, builds, tests, installs, staging, or commits.
- No recommendation for immediate patches.
- No claim that policy mismatch is unsafe solely because evidence is incomplete.

## 34. Open Questions

- Has qualified legal counsel reviewed the mobile privacy policy, mobile terms, web privacy page, and web terms page?
- Are the web legal placeholder bodies reachable in production?
- What legal entity and jurisdiction should be shown?
- Is an imprint/impressum required for the intended launch markets?
- What is the canonical account deletion path?
- Does settings contain a working account deletion flow, or is that terms copy stale?
- Is support prepared to process deletion, export, refund, dispute, report, and moderation requests?
- What refund policy applies at checkout, per host, per event, and per venue reservation?
- What report/review/appeal process is intended for abuse, harassment, illegal content, fake events, and misleading listings?
- Are notification preferences enforced by push delivery?
- Are diagnostics and crash reports anonymous, user-linked, or mixed?
- What policy covers public storage URLs, featured media, marketing use of user content, and media deletion?

## 35. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `07_Audits/LegalTrustSafetyPolicyMappingAudit.md` was created/modified.

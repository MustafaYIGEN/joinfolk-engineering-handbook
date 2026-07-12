# Release Readiness / Production Hardening Gap Register

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook audit synthesis only
- canonical: false

## 2. Purpose

This register consolidates JoinFolk handbook audit outputs into one release-readiness and production-hardening gap register. It aggregates gap seeds, risk findings, launch dependencies, product/legal decisions, and production verification needs from existing handbook documents.

This is not an implementation plan, not a patch plan, not a migration plan, and not authorization to change code, dashboard, mobile, web, Supabase, SQL, RLS, RPCs, storage, auth, payment infrastructure, or production systems.

## 3. Source Set and Evidence Boundary

This register uses existing handbook audits and decision records only. No application source, dashboard source, mobile source, web source, Supabase source, production database, Supabase CLI, builds, tests, dependency installs, or external legal review were inspected or performed for this synthesis.

Evidence boundary:

- All referenced audits are Draft / Version 0.2 / non-canonical unless separately verified.
- Production SQL/RPC/RLS evidence remains stronger than local source assumptions.
- Database Functions / RPC evidence is separate from Edge Function deployment evidence.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Local Edge Function source folders exist in some Supabase trees, but deployment status is not confirmed.
- Supabase migration future working target is `C:\dev\hostos\supabase\migrations`.
- That target is not historical sole canonical proof.
- Split-source migration history remains unresolved.
- No backend patch, migration, source cleanup, or production change is authorized by this register.

## 4. Release Readiness Executive Summary

The highest release-readiness themes are cross-cutting rather than isolated to one feature:

- Supabase source/provenance: future migration target is accepted as `C:\dev\hostos\supabase\migrations`, but historical source-of-truth and production deployment path remain unresolved.
- RPC/RLS authority: sensitive functions, grants, `SECURITY DEFINER` settings, search paths, overloads, and RLS policy correctness require production verification before implementation work.
- Direct data access reliance: privacy- and revenue-sensitive tables are used through mixed direct table access, RPCs, storage APIs, and UI filters; RLS enabled is not enough.
- Commerce/payment/refunds: purchase/order/ticket authority, active payment provider/webhook state, refund/dispute policy, receipt semantics, and financial retention are unresolved.
- Privacy/legal policy mismatch: account deletion, 30-day deletion copy, data export, web legal placeholder pages, diagnostics disclosure, and refund copy need product/legal reconciliation.
- Notifications/diagnostics: server notification delivery is now proven through the guarded outbox/scheduler/Edge boundary, while reminder device UAT and legacy RPC rollout remain open; diagnostics payload privacy, retention, and support visibility remain unresolved.
- Abuse/moderation: formal report, review, appeal, takedown, public suppression, and evidence retention workflows are not fully confirmed.
- Ops/support/admin: support/private-data visibility, manual overrides, deletion/refund/report/export handling, and admin action auditability need explicit process contracts.
- Public web/share/feed: event/detail/feed/search/share visibility parity, public claim/verification boundaries, and deleted/hidden/private suppression remain pre-launch verification candidates.
- Media/storage: database hide/delete and storage object deletion/public URL invalidation are separate and not fully verified.
- Messaging: DM delete/archive/report/support visibility and notification-preview privacy remain unresolved.

No launch readiness claim is made by this register.

## 5. Release Decision Framework

Priority labels in this register are planning candidates, not final severity findings:

- Candidate P0: use only where prior evidence indicates likely launch-blocking security, privacy, revenue, legal, or production risk. Incomplete evidence alone is not enough.
- Candidate P1: strong pre-launch or release-hardening candidate; likely must be resolved, verified, or explicitly accepted before broader launch.
- Candidate P2: important beta-readiness, scale-readiness, or pre-scale hardening issue that may follow P1 if launch scope allows.
- Candidate P3: documentation, polish, cleanup, or post-launch backlog.
- Unknown / Needs verification: evidence is incomplete and must not be converted to patch work before verification.

Release decision labels:

- Launch blocker candidate.
- Pre-launch hardening candidate.
- Beta hardening candidate.
- Post-launch backlog.
- Product decision required.
- Legal review required.
- Production verification required.
- Patch plan required.
- Documentation only.
- Unknown / Needs verification.

## 6. Master Release Gap Register

| Register ID | Source gap ID(s) | Source audit(s) | Domain | Consolidated issue | Evidence status | Risk class | Priority candidate | Release decision | Blocked by | Recommended next action | Patch plan group |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| RR-GAP-001 | BE-AUD-001; provenance findings; Supabase target decision | Supabase Backend Gap, Production Parity, Focused Follow-Up, Production Provenance, Source Map, Supabase Target Decision | Supabase source/provenance | Split Supabase source history and production deployment path remain unresolved even though future target is accepted | Mixed production/local/decision evidence | Product correctness, operational-sensitive | Candidate P1 | Pre-launch hardening candidate; Production verification required | Deployment path evidence and source reconciliation | Create production provenance verification report and source/deployment ADR | PP-01 |
| RR-GAP-002 | BE-AUD-005/006/008; ABV-GAP-001; DDA gaps; SHO-GAP-002/007 | Supabase Production Parity, Focused Follow-Up, Authority Boundary, Direct Data Access, Staff/Host | Direct data access / RLS | Sensitive RPC bodies, grants, `SECURITY DEFINER`, search paths, overloads, and RLS policies need production verification | Prior production evidence partial; correctness incomplete | Security-sensitive, privacy-sensitive, revenue-sensitive | Candidate P1 | Pre-launch hardening candidate; Production verification required | Production SQL/RPC/RLS review | Create Production RPC/RLS verification pack | PP-01 |
| RR-GAP-003 | DDA findings; CTC-GAP-004; VBF-GAP-008; SHO-GAP-001/005 | Direct Data Access, Commerce, Venue Buyer, Staff/Host | Direct data access / RLS | Mixed direct table/storage reads and writes rely on RLS for sensitive profile, event, ticket, reservation, media, staff, venue, diagnostics, notification, and social data | Audit synthesis; table-by-table correctness incomplete | Privacy-sensitive, revenue-sensitive | Candidate P1 | Pre-launch hardening candidate; Production verification required | RLS policy correctness and active callsite mapping | Build direct-access/RLS verification matrix | PP-01 |
| RR-GAP-004 | ABV-GAP-001+; ELC-GAP-001/002/003; SHO-GAP-004 | Authority Boundary, Event Lifecycle, Staff/Host | Authority / ViewerRole | ViewerRole, host/staff/participant/ticket-holder/checked-in/ops authority and lifecycle state semantics are spread across UI, RPC, RLS, and product docs | Mixed; needs production body and product decision | Security-sensitive, product correctness | Candidate P1 | Pre-launch hardening candidate; Product decision required | Authority matrix and lifecycle ADR | Create viewer-role/lifecycle decision matrix | PP-05 |
| RR-GAP-005 | CTC-GAP-001/002/003/006/009/011; VBF-GAP-011; PRD-GAP-001/002 | Commerce, Venue Buyer, Payments | Commerce / Ticketing | Purchase/order/ticket/claim/reservation source of truth and entitlement conflict coverage remain unresolved across RPC versions and UI paths | Prior audits; active path incomplete | Revenue-sensitive | Candidate P1 | Pre-launch hardening candidate; Product decision required; Production verification required | Canonical commerce path and production RPC review | Decide canonical commerce/payment/refund contract | PP-04 |
| RR-GAP-006 | LTS-GAP-003; PRD-GAP-002/003; CTC-GAP-011 | Legal Policy, Payments, Commerce | Payments / refunds / disputes | Refund policy text conflicts with checkout copy, while refund/dispute/provider/webhook implementation is not confirmed | Policy/source synthesis; provider deployment unknown | Revenue-sensitive, legal/compliance-sensitive | Candidate P1 | Legal review required; Product decision required | Product/legal/payment decision | Reconcile refund/dispute/payment policy before launch | PP-04 |
| RR-GAP-007 | VBF-GAP-001/002/003/004/005/008/011; CTC-GAP-005 | Venue Buyer, Commerce | Venue buyer flow | Venue layout snapshots, product-section mapping, exact-seat/standing/session capacity, and purchase revalidation need backend-authoritative parity | Audit synthesis; production RLS/RPC incomplete | Revenue-sensitive, product correctness | Candidate P1 | Pre-launch hardening candidate; Production verification required | Venue layout/session/table RLS and purchase RPC body review | Create venue buyer authority verification report | PP-04 |
| RR-GAP-008 | ELC-GAP-001/002/004/005/006/007; SDF gaps | Event Lifecycle, Search/Discovery, Public Web | Event lifecycle | Event state, status aliases, publish/unpublish/cancel/delete/archive, commerce availability, feed/search/share visibility, and notifications need one accepted lifecycle contract | Mixed local/production evidence; product decisions open | Privacy-sensitive, revenue-sensitive, product correctness | Candidate P1 | Product decision required; Production verification required | Lifecycle state machine decision and RPC review | Create lifecycle/public visibility decision pack | PP-05 |
| RR-GAP-009 | PWS gaps; SDF-GAPs; SGV-GAPs; PRV-GAP-010 | Public Web, Search/Discovery, Social Graph, Privacy | Public web / share | Public web/share/feed/search/detail suppression parity for private/group/invite/deleted/hidden/archived/moderated content remains unresolved | Prior audits; policy correctness incomplete | Privacy-sensitive, product correctness | Candidate P1 | Pre-launch hardening candidate; Production verification required | Public route and RLS/storage verification | Create public visibility suppression pack | PP-05 |
| RR-GAP-010 | NPR gaps; LTS-GAP-004; PRV-GAP-005 | Notification, Legal Policy, Privacy | Notifications / push / reminders | Server notification security and delivery are proven through the guarded outbox/scheduler/Edge boundary. Remaining open gates are closed-app reminder device UAT, legacy RPC Phase B rollout, and any not-yet-proven device receipt evidence. | Updated notification production evidence | Privacy-sensitive | Candidate P1 | Conditional pass / Not fully closed; production verification required for device-visible reminder receipt and legacy RPC Phase B | Device UAT and legacy RPC rollout verification | Use the notification delivery closure audit and status gate | PP-06 |
| RR-GAP-011 | DOA-GAP-001/002/006/007; LTS-GAP-005; PRV-GAP-006/007 | Diagnostics, Legal Policy, Privacy | Diagnostics / observability / audit logs | Diagnostics payload minimization, user linkage, support read access, retention/redaction, and audit-log immutability are unresolved | Prior audits; production policy incomplete | Privacy-sensitive, compliance/audit-sensitive | Candidate P2 | Beta hardening candidate; Legal review required | Diagnostics payload review and audit policy | Create diagnostics/privacy/auditability pack | PP-06 |
| RR-GAP-012 | MGM gaps; PRV-GAP-003; LTS-GAP-007; ARM media gaps | Media, Privacy, Legal Policy, Abuse | Media / gallery / memory wall | Media hide/delete/moderate, public highlights, storage object deletion, public URL invalidation, and content-license policy need verification | Prior audits; storage status partial | Privacy-sensitive | Candidate P2 | Beta hardening candidate; Production verification required | Storage bucket/object lifecycle review | Create media storage lifecycle pack | PP-09 |
| RR-GAP-013 | PPI gaps; PRV-GAP-002; LTS public identity findings | Profile/Persona, Privacy, Legal Policy | Profile / persona / public identity | Personal profile, host persona, public identity, avatar exposure, redaction, and host identity transfer retention need canonical field contracts | Prior audits; profile RLS incomplete | Privacy-sensitive, operational-sensitive | Candidate P2 | Product decision required; Production verification required | Profile/persona field and transfer policy decision | Include in account deletion/data request pack | PP-03 |
| RR-GAP-014 | SGV gaps; ARM block/mute gaps; PPI social findings | Social Graph, Abuse, Profile | Social graph / groups / visibility | Friend/follow/group/block/mute/share-group effects on visibility, messaging, notifications, discovery, and deletion are not fully documented or verified | Prior audits; production evidence incomplete | Privacy-sensitive, trust/safety-sensitive | Candidate P2 | Beta hardening candidate | Product decisions and RLS verification | Create social visibility side-effect decision matrix | PP-07 |
| RR-GAP-015 | SDF gaps; ELC-GAP-006; SGV findings | Search/Discovery, Event Lifecycle, Social Graph | Search / discovery / feed | Home/Discover/Search/Rising/feed listing must match detail/share visibility and lifecycle/social/private rules | Prior audits; parity incomplete | Privacy-sensitive, product correctness | Candidate P1 | Pre-launch hardening candidate | Feed/detail/public parity verification | Include in public visibility suppression pack | PP-05 |
| RR-GAP-016 | SHO-GAP-001/002/003/005/007/008/009; proof plan | Staff/Host, Commerce, Public Web, Patch Plan | Staff / host operations | Staff assignment, scanner/manager roles, check-in proof helpers, public verification, ticket/reservation mutations, and UI guard boundaries need authority proof | Positive controls plus unresolved proof/helper risk | Security-sensitive, revenue-sensitive | Candidate P1 | Pre-launch hardening candidate; Patch plan required | Production function reachability/body review | Production verification first; proof plan remains blocked | PP-01 |
| RR-GAP-017 | MDC gaps; PRV-GAP-004; ARM messaging gaps | Messaging, Privacy, Abuse | Messaging / direct conversation | DM membership, delete/archive semantics, notification previews, report/moderation, deep links, and support visibility remain unresolved | Prior audits; production evidence incomplete | Privacy-sensitive | Candidate P2 | Beta hardening candidate | DM schema/RPC/policy verification | Create messaging privacy lifecycle pack | PP-10 |
| RR-GAP-018 | OAS gaps; SHO-GAP-007; DOA auditability gaps | Ops/Admin, Staff/Host, Diagnostics | Ops / admin / support tools | Support/admin private-data visibility, transfer/admin actions, manual overrides, and mutation auditability need explicit process and backend gates | Partial admin RPC evidence; process incomplete | Operational/admin-sensitive, privacy-sensitive | Candidate P1 | Pre-launch hardening candidate; Product decision required | Support/admin process decision and audit verification | Create ops/admin support auditability pack | PP-08 |
| RR-GAP-019 | ARM-GAPs; LTS-GAP-006; DOA moderation gaps | Abuse, Legal Policy, Diagnostics | Abuse / reporting / moderation | Formal report submission/review/resolution, appeal/takedown, evidence privacy, moderation logs, and public suppression are not confirmed | Prior audits; formal system not confirmed | Trust/safety-sensitive, privacy-sensitive | Candidate P2 | Beta hardening candidate; Legal review required | Trust/safety process and policy decision | Create abuse/moderation workflow pack | PP-07 |
| RR-GAP-020 | PRV-GAP-001/002/005/006/007/008/009/010; LTS-GAP-001/010 | Privacy, Legal Policy, Payments, Diagnostics | Privacy / retention / deletion | Account deletion, data export, retention, redaction, storage deletion, diagnostics, audit logs, commerce retention, and report evidence policies are unresolved | Policy/source/audit synthesis | Privacy-sensitive, legal/compliance-sensitive | Candidate P1 | Legal review required; Product decision required | Privacy/legal retention model and implementation verification | Create account deletion/data request decision pack | PP-03 |
| RR-GAP-021 | LTS-GAP-001/002/003/004/005/009; PRV policy findings | Legal Policy, Privacy, Payments, Notifications | Legal / trust & safety policy | Web legal placeholders, account deletion promises, refund mismatch, notification settings, diagnostics disclosure, legal identity/impressum are unresolved | Handbook/source-policy synthesis | Legal/compliance-sensitive | Candidate P1 | Legal review required | Qualified legal/product copy review | Create legal/public policy copy pack | PP-02 |
| RR-GAP-022 | PRD-GAP-008; PRV-GAP-008; DOA commerce auditability | Payments, Privacy, Diagnostics | Payments / refunds / disputes | Commerce/payment/ticket/order/reservation retention, receipts, provider payloads, and revenue auditability need policy/process contract | Prior audits; provider active state unknown | Revenue-sensitive, compliance/audit-sensitive | Candidate P2 | Beta hardening candidate; Legal review required | Payment/provider/support audit decision | Include in commerce/refund/payment pack | PP-04 |
| RR-GAP-023 | ADRIndex findings; product decision sections across audits | Product contract | Product contract | Many domains require accepted decisions before patches: commerce, deletion, retention, moderation, public content, support powers, lifecycle, roles, legal copy | Handbook synthesis | Product correctness | Candidate P2 | Product decision required | Decision owners and ADR workflow | Create decision records listed in this register | PP-02 |
| RR-GAP-024 | AUTH-EMAIL-01 | Auth email mobile-first contract audit | Auth email / universal links / fallback routing | Password-reset and email-confirmation flows do not have one frozen public canonical host, public AASA is missing, public reset fallback renders the wrong surface, `app.join-folk.com` is rejected as the current auth email host, native confirmation route is missing, and Supabase redirect/template evidence is not yet captured. | Mixed live/source/handbook evidence; some failures are now proven, while Dashboard evidence remains incomplete | Security-sensitive, product correctness | Candidate P1 | Pre-launch hardening candidate; Product decision required; Production verification required | Canonical public-host decision and Dashboard evidence | Freeze canonical public auth-link host before implementation | PP-11 |

## 7. Candidate P0 Launch Blocker Review

No Candidate P0 is assigned by this register based on current evidence.

Items that may become Candidate P0 only after stronger production/legal verification:

- Proof/check-in helper functions if production reachability proves unauthorized mutation of check-in proof state.
- Push dispatch or Edge Function surfaces if later verified as deployed and externally callable without accepted caller controls.
- Public/private visibility exposure if production RLS/public route verification proves private/group/invite/deleted content is publicly readable.
- Payment/order/refund/dispute paths if production verification proves unauthorized revenue mutation or duplicated paid entitlement.
- Legal/policy claims if qualified legal review classifies them as launch-blocking for the target market.

## 8. Candidate P1 Pre-Launch Hardening

Candidate P1 items likely requiring resolution, verification, or explicit owner acceptance before broader launch:

- RR-GAP-001: Supabase source/provenance and deployment path.
- RR-GAP-002: production RPC/RLS/grant/body verification for sensitive functions.
- RR-GAP-003: direct data/RLS reliance for sensitive tables.
- RR-GAP-005: commerce purchase/order/ticket source of truth.
- RR-GAP-006: refund/payment policy mismatch.
- RR-GAP-007: venue buyer authority for seat/standing/session/capacity.
- RR-GAP-008: lifecycle state and public/commerce side effects.
- RR-GAP-009 and RR-GAP-015: public web/share/feed/search suppression parity.
- RR-GAP-010: notification server delivery proven; reminder device UAT and legacy RPC rollout remain open.
- RR-GAP-016: staff/scanner/check-in proof authority.
- RR-GAP-018: ops/admin/support auditability and private-data process.
- RR-GAP-020 and RR-GAP-021: account deletion, retention, policy copy, legal placeholders, and legal review dependencies.

Notification-specific closure note:

- Server-side notification delivery security is closed enough for a conditional pass.
- Reminder local scheduling is implemented but not yet device-accepted.
- Legacy authenticated notification RPC access remains rollout-dependent.
- The open P0-style blocker list no longer includes server-delivery failure as an unresolved item; the remaining gates are device UAT and Phase B rollout.

## 9. Candidate P2 Beta / Pre-Scale Hardening

Candidate P2 items:

- RR-GAP-011: diagnostics payload minimization, retention, support visibility, and audit-log semantics.
- RR-GAP-012: media storage lifecycle, public URL invalidation, and public content policy.
- RR-GAP-013: profile/persona redaction and host identity transfer retention.
- RR-GAP-014: social graph/block/mute/group visibility side effects.
- RR-GAP-017: messaging delete/archive/report/support visibility semantics.
- RR-GAP-019: formal abuse/report/moderation workflow.
- RR-GAP-022: commerce/payment/ticket retention, provider payloads, and revenue auditability.
- RR-GAP-023: decision workflow and ADR backlog.

## 10. Candidate P3 Post-Launch Backlog

Candidate P3 should remain limited to documentation, polish, or low-risk follow-up after P1/P2 gates are resolved or accepted. Current examples:

- Cross-linking duplicate audit findings once canonical decisions exist.
- Improving handbook ADR workflow metadata once release gates are defined.
- UI copy polish where policy/implementation behavior is already verified.
- Documentation-only parity notes for tolerated dashboard/mobile/web visual differences.

No security/privacy/revenue/legal uncertainty is intentionally downgraded to P3.

## 11. Product Decision Dependencies

Product decisions required before implementation work:

- Canonical purchase/order/ticket/claim/reservation path.
- Paid vs free ticketing scope and payment-provider launch scope.
- Canonical refund, cancellation, dispute, and chargeback policy.
- Account deletion model: self-service, support-mediated, both, or deferred.
- Data retention/redaction/export model.
- Formal report/moderation/takedown/appeal workflow.
- Support/admin powers for deletion, refund, dispute, report, export, transfer, and private-data review.
- Public content/media policy, including user content license and storage visibility.
- Notification preference semantics and private-preview behavior.
- Host/venue/business obligations and support responsibility boundaries.
- Lifecycle state machine, including cancel/archive/delete/unpublish side effects.
- ViewerRole/authority vocabulary and role-to-action matrix.
- Launch market/legal entity assumptions.

## 12. Legal Review Dependencies

Legal review topics, without legal advice or compliance claims:

- Privacy policy and mobile/web consistency.
- Terms of service and user agreement.
- Refund, cancellation, dispute, chargeback, ticket, and reservation policy.
- Account deletion, data request, data export, retention, and redaction policy.
- Data retention exceptions for audit, legal, commerce, safety, and support records.
- Report, moderation, takedown, appeal, and trust/safety policy.
- Imprint/impressum, legal entity, jurisdiction, complaint/escalation, and support contact.
- Payment provider, receipt, invoice, and provider payload terms.
- User content/media license and marketing-use language.
- Public marketing claims and public/private visibility language.

## 13. Production Verification Dependencies

Verification needed before patches:

- RPC bodies, overloads, grants, `SECURITY DEFINER`, search paths, and internal guards.
- RLS policies for sensitive tables.
- Deployed Edge Function state and deployment source.
- Storage bucket policies and public/signed URL behavior.
- Actual public web routes, legal pages, support pages, and deployed policy copy.
- Active payment provider, webhook, receipt, refund, and dispute behavior.
- Notification/push delivery, preference consumption, private previews, and payloads.
- Current deployed mobile/web/dashboard behavior where UI parity matters.
- Support/admin production routes and private-data visibility.
- Account deletion/data export implementation, if any.
- Diagnostics and audit-log read/write/retention behavior.

## 14. Backend / RPC / RLS Hardening Dependencies

Backend hardening depends on:

- Production verification first; no local-source-only patch assumptions.
- Canonical RPC names and overload conventions for commerce, lifecycle, notifications, messaging, check-in, transfers, media, and social graph.
- Table-by-table RLS policy correctness review for direct-access surfaces.
- Function-by-function grant and internal gate review.
- `SECURITY DEFINER` search-path and proconfig review.
- Clear separation of RPC authority from Edge Function deployment.
- Confirmed public storage policy semantics.
- Explicit acceptance of any direct table read/write pattern that remains.

## 15. Public Web / Policy Copy Dependencies

Dependencies:

- Web privacy and terms pages need final reviewed copy or explicit launch acceptance.
- Public event/share/feed/search/detail routes need suppression parity for private/group/invite/deleted/hidden/archived/moderated content.
- Public claim/share/check-in verification routes need public-safe field contracts.
- Public profile/avatar/venue/media/highlight/relic fields need accepted public exposure contracts.
- Legal copy must not exceed product/backend capability unless explicitly accepted by product/legal.
- Marketing/support claims must match deployed behavior.

## 16. Mobile / Dashboard / Web Parity Dependencies

Parity areas:

- Mobile/dashboard lifecycle state, readiness, publish/unpublish/cancel/archive/delete behavior.
- Dashboard preview vs mobile buyer map and published venue/layout snapshots.
- Mobile checkout/refund copy vs terms/support copy.
- Mobile notification settings vs push delivery behavior.
- Mobile/web public share and deep-link behavior.
- Dashboard route guards vs backend authority.
- Mobile staff/scanner tools vs backend proof/check-in authority.
- Mobile profile/privacy settings vs public/profile/feed/media visibility.

## 17. Privacy / Retention / Deletion Dependencies

Dependencies:

- Account deletion flow and support/self-service model.
- Data export/portability policy and implementation.
- Profile/persona redaction, avatar storage exposure, and host identity transfer retention.
- Media DB delete vs storage object delete and public URL invalidation.
- Message delete/archive retention semantics.
- Notification history vs push token deletion.
- Diagnostics retention, payload minimization, and support read access.
- Audit log retention, redaction, and append-only expectations.
- Commerce/ticket/order/reservation/legal retention.
- Report/moderation evidence retention and appeal/redaction policy.

## 18. Revenue / Payments / Refunds Dependencies

Dependencies:

- Canonical purchase/order/payment authority.
- Active payment provider/webhook verification.
- Refund, cancellation, dispute, and chargeback policy decision.
- Ticket entitlement vs order/payment state boundary.
- Reservation cancellation vs refund boundary.
- Claim/gift/transfer money/entitlement boundary.
- Stale/expired order capacity release behavior.
- Host revenue visibility vs support/admin refund authority.
- Revenue-sensitive auditability and retention.

## 19. Trust & Safety / Moderation Dependencies

Dependencies:

- Formal report target types and submission workflow.
- Support/admin report review and resolution authority.
- Appeal, takedown, restoration, and moderation notice model.
- Block vs mute semantics.
- DM abuse reporting and private-message evidence policy.
- Media/comment/memory-wall moderation and owner/uploader controls.
- Public/feed/search suppression after moderation.
- Moderation evidence retention/redaction and auditability.

## 20. Ops / Admin / Support Process Dependencies

Dependencies:

- Support processes for deletion, export, refund, dispute, report, moderation, and private-data requests.
- Ops/admin transfer authority, broad grants, internal gates, and auditability.
- Manual override process and traceability.
- Support read-only access vs mutation authority.
- Dashboard route guards vs backend authority.
- Admin action audit logs and retention.
- Support/legal escalation paths.

## 21. Diagnostics / Auditability / Observability Dependencies

Dependencies:

- `app_diagnostics` payload minimization.
- Client diagnostics trust boundary.
- Diagnostics read access and support/ops visibility.
- Notification/push/reminder delivery logging.
- Commerce/ticket/reservation action traceability.
- Transfer/admin/moderation audit logs.
- Audit log access controls, retention, redaction, and append-only semantics.
- Avoiding secrets/private provider payloads in logs and diagnostics.

## 22. Supabase Source / Migration Provenance Dependencies

Preserved source/provenance facts:

- Future accepted migration working target is `C:\dev\hostos\supabase\migrations`.
- This is not proof of historical sole canonical source.
- Split-source migration history remains unresolved.
- Production deployment path remains Unknown / Needs verification.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Local Edge Function source is not deployment proof.
- Production SQL/RPC evidence remains stronger than local source assumptions.
- No Supabase tree modification, migration drafting, cleanup, normalization, or reconciliation is authorized by this register.

## 23. Unknown / Needs Verification Consolidation

Do not convert these unknowns into implementation patches yet:

- Historical canonical Supabase source and production deployment path.
- Production migration version parity.
- Deployed Edge Function state.
- Active payment provider/webhook/refund/dispute behavior.
- Production RLS correctness for sensitive direct-access tables.
- Production RPC body/grant/search-path/internal-gate coverage.
- Account deletion/data export implementation.
- Legal status of privacy/terms/refund/report copy.
- Notification preference delivery enforcement.
- Diagnostics payload linkage, retention, and admin read access.
- Formal report/moderation/review/appeal/takedown workflow.
- DM delete/archive/support visibility.
- Storage object deletion and public URL invalidation.
- Public/share/feed/search/detail visibility parity.
- Support/admin production routes and auditability.

## 24. Recommended Patch Plan Groups

Planning buckets only; these are not patch authorization:

- PP-01 Production Verification Pack: production RPC/RLS/grants/search-path/storage/Edge verification.
- PP-02 Legal/Public Policy Copy Pack: web/mobile legal copy, refund copy, notification copy, diagnostics disclosure, support contact, legal entity.
- PP-03 Account Deletion / Data Request Decision Pack: deletion, export, retention, redaction, support process.
- PP-04 Commerce/Refund/Payment Contract Pack: purchase/order/ticket/refund/dispute/provider/claim/reservation authority.
- PP-05 Public Visibility Suppression Pack: public web/share/feed/search/detail lifecycle and social/private visibility parity.
- PP-06 Notification/Diagnostics Privacy Pack: notification preferences, private previews, push tokens, diagnostics payloads and retention.
- PP-07 Abuse/Moderation Workflow Pack: reporting, moderation, appeals, takedown, block/mute, evidence retention.
- PP-08 Ops/Admin Support Auditability Pack: support/admin visibility, manual overrides, transfer tools, audit logs.
- PP-09 Media Storage Lifecycle Pack: media hide/delete/moderate, storage object deletion, public URL behavior.
- PP-10 Messaging Privacy Lifecycle Pack: DM membership, delete/archive, notification previews, report/support visibility.
- PP-11 Auth Email Mobile-First Closure Pack: canonical auth-link host, AASA alignment, associated domains, native reset/confirmation routing, safe web fallback, and template/redirect alignment.

## 25. Recommended Decision Records

Decision records to create before implementation where applicable:

- Canonical Supabase source and deployment path decision, superseding or extending the future target record if stronger evidence appears.
- Canonical commerce/payment/refund/dispute policy.
- Account deletion/data retention/redaction/export model.
- Legal entity/public launch policy source.
- Trust/safety/report/moderation/takedown/appeal workflow.
- Support/admin authority and auditability model.
- Public visibility and public media field contract.
- Event lifecycle state machine and visibility/commerce side effects.
- ViewerRole/authority vocabulary and role-action matrix.

## 26. Recommended Verification Reports

Verification reports to create before patches:

- Production RPC/RLS verification report.
- Storage bucket policy and public/signed URL verification report.
- Public web/legal route verification report.
- Notification delivery/preference/private-preview verification report.
- Payment provider/webhook/refund/dispute verification report.
- Account deletion/data export implementation verification report.
- Support/admin route authority and auditability verification report.
- Media DB/storage lifecycle verification report.
- Messaging lifecycle and support visibility verification report.
- Public feed/search/share/detail suppression parity verification report.

## 27. Launch Readiness Invariants

- Policy text must not exceed accepted product/backend capability unless explicitly accepted.
- UI state is not backend authority.
- RLS enabled is not enough; policy/body/grant behavior matters.
- Local source is not production proof.
- Edge Function source is not deployment proof.
- Payment/order state and ticket entitlement are separate.
- Refund, cancellation, and dispute are separate.
- Product deletion, audit retention, and legal retention are separate.
- Public visibility suppression must be backend-authoritative.
- Support visibility is not mutation authority.
- Admin mutations must be auditable.
- Logs and diagnostics must not contain secrets or private provider payloads.
- No launch readiness claim should be made without verification and owner decisions.

## 28. Non-Goals

- No code changes.
- No dashboard/mobile/web changes.
- No Supabase tree changes.
- No SQL or migrations.
- No production connection.
- No builds, tests, installs, staging, or commits.
- No legal advice.
- No compliance claim.
- No immediate patch authorization.
- No source-code re-audit.
- No launch readiness claim.

## 29. Open Questions

- What is the actual production deployment path for Supabase migrations and functions?
- Which current production RPCs, grants, and RLS policies are canonical for launch?
- Which public routes and legal pages are deployed and reachable?
- What is the canonical refund/dispute/payment policy?
- Is account deletion support-mediated, self-service, both, or deferred?
- What is the data retention/redaction/export model?
- Which notification preferences are actually enforced by delivery?
- What diagnostics payloads are retained, and who can read them?
- What report/moderation/appeal workflow exists for launch?
- Which support/admin actions are allowed and audited?
- Which payment provider/webhook surfaces are active?
- Which gaps are explicitly accepted for private beta versus broader launch?

## 30. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `00_Status/ReleaseReadinessProductionHardeningGapRegister.md` was created/modified.

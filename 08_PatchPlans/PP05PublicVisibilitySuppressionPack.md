# PP-05 Public Visibility Suppression Pack

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

This is a decision-pack for defining JoinFolk public visibility, suppression, public-safe field, feed/search/share/detail/profile/media/claim/check-in exposure, and backend-authoritative visibility semantics before implementation work begins.

It is not route implementation, not RLS/RPC/storage implementation, not production verification, not legal advice, and not patch authorization.

## 3. Evidence Boundary

This document is based only on handbook audits, the Release Readiness / Production Hardening Gap Register, PP-01, PP-02, PP-03, and PP-04.

No source-code inspection, production connection, Supabase CLI, SQL, builds, tests, public route verification, storage verification, legal review, final copy drafting, or policy copy modification was performed.

## 4. PP-05 Scope Summary

PP-05 covers:

- Public visibility vocabulary.
- Public surface inventory.
- Public-safe field classification.
- Lifecycle, event, profile, persona, media, feed, search, discovery, share, deep link, claim, QR, and check-in suppression models.
- Social, group, invite, block, mute, deletion, redaction, moderation, abuse, commerce, refund, cancellation, host, venue, support, and ops visibility constraints.
- Backend/RPC/RLS/storage/public-route verification dependencies.
- PP-02 public copy constraints.
- PP-03 deletion/data-request constraints.
- PP-04 commerce/refund/payment constraints.
- Dependency mapping to PP-06 through PP-10.

PP-05 does not execute PP-01 and does not authorize route, RLS, RPC, storage, source, copy, database, or production changes.

## 5. Source Register Coverage

| Release gap | Why PP-05 covers it | PP-05 limitation |
|---|---|---|
| RR-GAP-003 | Public visibility depends on direct table/storage access and RLS correctness. | PP-05 does not verify policies. |
| RR-GAP-004 | ViewerRole and authority semantics affect public, authenticated, participant, host, staff, and admin visibility. | PP-05 defines visibility decisions, not full authority implementation. |
| RR-GAP-008 | Event lifecycle state affects public detail, share, feed, search, commerce, and notifications. | PP-05 does not decide the complete lifecycle state machine. |
| RR-GAP-009 | Public web/share/feed/search suppression parity is the central PP-05 scope. | PP-05 does not verify deployed routes. |
| RR-GAP-012 | Media and storage public URL behavior affects public exposure and deletion. | PP-05 depends on PP-09 for storage lifecycle detail. |
| RR-GAP-013 | Profile/persona/public identity fields need a public-safe contract. | PP-05 depends on PP-03 for deletion/redaction decisions. |
| RR-GAP-014 | Social graph, groups, block, mute, and share-group side effects affect public visibility. | PP-07 owns abuse/block/mute workflow detail. |
| RR-GAP-015 | Feed/search eligibility must match detail/share visibility. | PP-05 defines parity requirements; later verification proves behavior. |
| RR-GAP-016 | Staff/check-in/public verification surfaces require public-safe proof contracts. | PP-01 verifies function authority first. |
| RR-GAP-017 | Messaging/deep links and private notification previews intersect public routes. | PP-10 owns messaging lifecycle detail. |
| RR-GAP-018 | Support/admin suppression and restore actions need process and auditability. | PP-08 owns support/admin process. |
| RR-GAP-019 | Report, moderation, takedown, appeal, and public suppression need trust/safety decisions. | PP-07 owns formal workflow. |
| RR-GAP-020 | Deletion, redaction, storage deletion, and account removal must suppress public exposure. | PP-03 owns deletion model. |
| RR-GAP-021 | Public copy/legal pages and public visibility claims need policy alignment. | PP-02 owns copy review. |
| RR-GAP-023 | Product decisions must be accepted before patches. | PP-05 recommends decision records only. |

## 6. Public Visibility Problem Statement

Public visibility is not one UI flag. It includes:

- Public web route.
- Mobile feed.
- Web feed.
- Search result.
- Discovery, Rising, and Home feed.
- Share link.
- Deep link.
- Public profile.
- Public event detail.
- Public venue/business page.
- Public media and highlights.
- Ticket, claim, QR, and check-in verification.
- Storage public URL.
- Group, private, and invite-only visibility.
- Deleted, hidden, archived, cancelled, moderated, and taken-down state.
- Public-safe fields.
- Anonymous user, authenticated user, participant, ticket holder, checked-in user, host, staff, support, and admin views.

Route existence must not be treated as permission to expose data. Public visibility suppression must be backend-authoritative.

## 7. Public Surface Inventory

| Surface | Example user expectation | Data/function/storage domains affected | Owner | PP-01 evidence dependency | PP-02 copy dependency | PP-03 deletion dependency | PP-04 commerce dependency | Current status | Recommended PP-05 decision need |
|---|---|---|---|---|---|---|---|---|---|
| Public event detail page | Anonymous viewer can see public event data. | Events, lifecycle, venue, host, media. | Product + public web | Public route/RLS/RPC evidence | Event visibility copy | Deleted/archived handling | Cancellation/ticket state | Unknown / Needs verification | Define public event field/state contract. |
| Public share event link | Shared link opens safe event view. | Events, share groups, lifecycle. | Product | Share route evidence | Share copy | Deletion suppression | Ticket/refund state | Unknown / Needs verification | Define private/group/invite fallback. |
| Public profile page | Public identity is viewable. | Profiles, personas, avatar storage. | Product + privacy | Profile RLS/route evidence | Profile privacy copy | Redaction fallback | Host attribution | Unknown / Needs verification | Define public profile fields. |
| Public host persona page | Organizer appears public. | Host persona, events, transfers. | Product | Persona/profile evidence | Host public copy | Host deletion/redaction | Host commerce visibility | Unknown / Needs verification | Separate host persona from personal profile. |
| Public venue/business page | Venue/business info is public. | Venues, venue media, reservations. | Venue/product | Venue route/storage evidence | Venue copy | Venue owner deletion | Reservation visibility | Unknown / Needs verification | Define business public fields. |
| Home feed | Eligible public content appears. | Events, lifecycle, social, feed. | Product | Feed backend evidence | Marketing/feed copy | Suppression/deletion | Commerce availability | Unknown / Needs verification | Match feed to detail/share visibility. |
| Discover feed | Public discoverable events appear. | Events, groups, privacy, search. | Product | Feed/search evidence | Discover copy | Suppression | Commerce state | Unknown / Needs verification | Define eligibility. |
| Rising/search feed | Search results match visibility. | Search, lifecycle, public fields. | Product | Search RPC/table evidence | Search copy | Redaction/deletion | Ticket availability | Unknown / Needs verification | Define search parity. |
| Search results | Private/suppressed items excluded. | Profiles, events, venues. | Product + privacy | Search evidence | Public search copy | Redaction | Commerce state | Unknown / Needs verification | Define result field contract. |
| Public media/highlights/memory wall | Public media is safe to show. | Event_media, venue_media, storage. | Media/product | Bucket/object evidence | Media policy copy | Media deletion | Event/ticket context | Unknown / Needs verification | Define public media and uploader fields. |
| Storage public URL | URL may remain directly reachable. | Buckets, objects, caching. | Media + security | Bucket policy evidence | Media/storage copy | Object deletion model | Receipt/proof media if any | Unknown / Needs verification | Decide intentional public URL semantics. |
| Signed media URL | Access may be temporary. | Storage, signed URL generation. | Media + backend | Signed URL evidence | Media copy | Object deletion | None unless proof media | Unknown / Needs verification | Define generator authority. |
| Ticket claim/share page | Token opens safe claim view. | Claims, tickets, share route. | Commerce + public web | Claim route/RPC evidence | Claim copy | Claim retention | Claim/transfer state | Unknown / Needs verification | Define token authority and public fields. |
| Public QR/ticket verification page | QR proves valid/invalid status safely. | Tickets, proof, check-in. | Commerce + staff | Verification route evidence | QR/wallet copy | Proof retention | Refunded/revoked state | Unknown / Needs verification | Define public-safe verification fields. |
| Check-in proof page | Proof can be verified without leaking private data. | Check-in proof, staff roles. | Staff/host + commerce | Proof RPC evidence | Check-in copy | Proof retention | Check-in/refund | Unknown / Needs verification | Define proof display and suppression. |
| Wallet/deep link preview | Deep link opens authorized context. | Tickets, messages, events, notifications. | Product | Deep link/backend evidence | Wallet/notification copy | Deletion suppression | Entitlement state | Unknown / Needs verification | Define preview privacy. |
| Social/group/private event visibility | Group/private events are restricted. | Share groups, memberships, social graph. | Product + privacy | Group/social evidence | Privacy copy | Group removal | Ticket eligibility | Unknown / Needs verification | Define backend-authoritative visibility. |
| Deleted/hidden/archived event route | Suppressed event is not exposed. | Lifecycle, events, feed/search. | Product | Lifecycle route evidence | Deletion copy | Deletion/redaction | Ticket/order retention | Unknown / Needs verification | Define fallback page. |
| Cancelled event route | Cancellation state is clear and safe. | Events, commerce, notifications. | Product + commerce | Lifecycle evidence | Cancellation copy | Event retention | Refund/ticket state | Unknown / Needs verification | Decide public cancelled view. |
| Moderated/taken-down media route | Removed content is suppressed. | Media, moderation, storage. | Trust/safety + media | Moderation/storage evidence | Safety/media copy | Evidence retention | None | Unknown / Needs verification | Decide takedown visibility. |
| Support/legal public pages | Public legal/support info is available. | Web routes, copy. | Legal + support | Public route evidence | Policy copy | None | None | Unknown / Needs verification | Confirm non-placeholder public content. |

## 8. Visibility State Taxonomy

- Public: accepted for broad public display.
- Anonymous-readable: readable without login.
- Authenticated-readable: readable by logged-in users only.
- Owner-only: visible only to record owner.
- Participant-only: visible only to accepted participants.
- Ticket-holder-only: visible only to ticket holders.
- Checked-in-only: visible only after check-in where product requires.
- Host-only: visible only to host/organizer authority.
- Staff-only: visible only to event staff/scanner/manager where accepted.
- Support/admin-only: visible only through operational/admin process.
- Group-only: visible only to accepted group members.
- Invite-only: visible only to invited/accepted users.
- Hidden: retained but not visible in normal public surfaces.
- Archived: retained outside active public flow.
- Deleted: removed or treated as removed from product visibility.
- Redacted: identifying fields removed/replaced.
- Suppressed: prevented from public/feed/search/share exposure.
- Moderated/taken down: suppressed by moderation process.
- Cancelled: lifecycle state with distinct public and commerce behavior.
- Refunded/revoked: commerce state affecting verification.
- Expired: no longer active or claimable.
- Transferred/claimed: entitlement ownership changed.
- Public-safe field: field accepted for public exposure.
- Public route exists but data suppressed: route shell may exist while data is not exposed.

Do not collapse route existence with data visibility.

## 9. Public-Safe Field Classification Model

| Category | Meaning | Example fields |
|---|---|---|
| Always public candidate | May be public after product/legal acceptance. | Public event title, event date/time, city-level location, public venue name. |
| Public only for published/live public events | Public only when lifecycle and privacy state allow. | Event description, public poster, organizer display name, public venue summary. |
| Public only with explicit host/business acceptance | Public where host/venue opted into exposure. | Venue/business page fields, official business badge, venue media, organizer bio. |
| Authenticated-only | Not anonymous; visible after login. | Some event interaction state, RSVP affordances, authenticated deep-link context. |
| Participant/ticket-holder-only | Visible to accepted participants or holders. | Participant-specific status, ticket holder-specific event access, private event details. |
| Owner/host/staff-only | Operational event management data. | Participant list, ticket status lists, scanner/check-in operation data. |
| Support/admin-only | Operational/private support data. | Support notes, admin actions, private diagnostics, moderation evidence. |
| Never public | Should not be publicly exposed under normal product contract. | Message content, diagnostics payloads, payment/order/provider data, private report evidence, claim tokens, QR secrets, support/admin notes. |

Field examples requiring explicit classification: precise address, personal profile name, avatar URL, organizer bio, ticket status, QR/check-in proof, claim token, participant list, media uploader identity, message content, diagnostics data, payment/order/provider data, support/admin notes, and report/moderation evidence.

## 10. Event / Lifecycle Visibility Decision Model

States requiring public behavior decisions:

- Draft.
- Published.
- Live.
- Ended.
- Archived.
- Cancelled.
- Deleted.
- Hidden/unpublished.
- Invite-only.
- Group-only.
- Private.
- Sold-out.
- Ticket sales disabled.
- Event commerce disabled.

Decision needed: which states appear in feeds, search, public detail, and share pages; which states show a public fallback; which states suppress data; and which states remain visible only to host/support/admin.

## 11. Profile / Persona / Public Identity Visibility Decision Model

Decision areas:

- Personal profile.
- Host persona.
- Public avatar.
- Organizer display name.
- Organizer bio.
- Host identity transfer.
- Deleted/redacted user.
- Blocked user.
- Private user profile.

The model must separate personal profile authority from host persona display and define event attribution after deletion/redaction.

## 12. Media / Storage / Public URL Visibility Decision Model

Decision areas:

- Event media.
- Venue media.
- Avatars.
- Posters.
- Public highlights.
- Memory wall.
- Uploaded videos.
- Storage objects.
- Public buckets.
- Signed URLs.
- DB row hidden/deleted.
- Object deleted.
- Moderation/takedown.

Decision needed: when database visibility suppresses object visibility, whether public URLs remain reachable, what cache/public URL caveats exist, and what media metadata is public-safe.

## 13. Feed / Search / Discovery Visibility Decision Model

Feed/search surfaces:

- Home.
- Discover.
- Rising.
- Search.
- Public web listing.
- City/system events if any.
- Promoted/featured events.

Feed eligibility must match detail/share visibility. Hidden, private, deleted, archived, cancelled, moderated, or otherwise suppressed content should not appear where the detail/share route suppresses data unless the product explicitly accepts a different contract.

## 14. Share Link / Deep Link / Public Web Route Visibility Decision Model

Decision areas:

- Event share links.
- Claim links.
- Check-in verification links.
- Profile links.
- Venue links.
- App deep links.
- Browser public routes.
- Expired/revoked/claimed link behavior.

Route existence is not data authorization. Token possession is not full authorization unless explicitly accepted by product/security.

## 15. Claim / Ticket / QR / Check-In Verification Visibility Decision Model

Decision areas:

- Claim token.
- QR code.
- Public ticket verification.
- Check-in proof.
- Checked-in state.
- Refunded, revoked, or cancelled ticket.
- Transferred or claimed ticket.

Decision needed: public-safe fields, token/QR possession authority, invalid-state display, and whether public verification exposes identity.

## 16. Social Graph / Group / Invite Visibility Decision Model

Decision areas:

- Share groups.
- Group membership.
- Invite-only event.
- Followers/friends.
- Block/mute.
- Host followers.
- Private event.

Social/group rules must affect feed, search, share, detail, profile, and media through backend-authoritative checks. A block may suppress interaction, visibility, notifications, or a subset; product decision is required.

## 17. Deletion / Redaction / Account Removal Visibility Decision Model

Decision areas:

- Account deleted.
- Profile redacted.
- Host persona redacted.
- Public media deleted.
- Storage object retained.
- Event owned by deleted user.
- Tickets/orders retained.

Decision needed: public identity fallback, event attribution fallback, public route suppression after deletion, public-safe redacted display text, and how retained records avoid public exposure.

## 18. Moderation / Abuse / Takedown Visibility Decision Model

Decision areas:

- Reported content.
- Hidden media.
- Host moderation.
- Support/admin moderation.
- Takedown.
- Appeal/restoration.
- Blocked user.

Decision needed: immediate suppression vs pending review, public placeholder vs not found, restoration behavior, and audit/evidence retention separate from public visibility.

## 19. Commerce / Refund / Cancellation Visibility Decision Model

Decision areas:

- Refunded ticket.
- Cancelled ticket.
- Revoked ticket.
- Disputed or chargeback ticket.
- Cancelled event.
- Sold-out event.
- Expired order.
- Claimed/transferred ticket.
- Cancelled reservation.

Decision needed: public wallet, QR, check-in, claim/share, feed/search, and event detail behavior after commerce state changes.

## 20. Venue / Host / Business Public Visibility Decision Model

Decision areas:

- Venue public listing.
- Official business badge.
- Venue media.
- Host/venue ownership.
- Deleted host.
- Venue reservation availability.
- Event venue address precision.

Decision needed: public field contract, address precision, ownership attribution, deleted-owner fallback, and suppression rules.

## 21. Support / Ops / Admin Public-Suppression Process Decision Model

Decision areas:

- Manual hide/suppress.
- Restore.
- Takedown.
- Legal request.
- Privacy request.
- Support/admin private data view.
- Audit trail.

Decision needed: who can suppress public content, who can restore it, what evidence is retained, what is visible publicly, and what auditability is required.

## 22. Backend / RPC / RLS / Storage Verification Dependencies

PP-05 requires verification of:

- RLS policies for public event, profile, media, venue, ticket, claim, share, social/group, and support/legal tables where applicable.
- RPCs powering public detail, feed, search, share, claim, and check-in verification.
- Storage bucket policies.
- Public and signed URL behavior.
- Lifecycle status fields.
- Visibility/privacy fields.
- Group/social membership fields.
- Moderation/deletion flags.
- Commerce state fields.
- Grants, `search_path`, and `SECURITY DEFINER` posture where public RPCs exist.

No SQL is included in this pack.

## 23. Public Route / Web Deployment Verification Dependencies

Facts needed:

- Which public web routes are deployed.
- Public legal/support route status.
- App deep links vs web routes.
- Static route fallback behavior.
- Not-found and suppression behavior.
- Search engine indexability if known.
- Public metadata/OpenGraph behavior if known.
- Caching/CDN behavior if known.
- Public route data source.

PP-05 does not execute route verification.

## 24. Public Visibility Data Domain Inventory Matrix

| Domain | Example data | Public expectation | Visibility decision needed | Legal/product review need | PP-01 evidence need | PP-03/PP-04 dependency | Later pack dependency |
|---|---|---|---|---|---|---|---|
| events | Event title, time, city, detail | Public events visible | Lifecycle/privacy state contract | High | Event RLS/RPC/route evidence | Deletion + cancellation | PP-06/PP-07 |
| event lifecycle/status | Draft/published/cancelled/deleted | State-appropriate display | Suppress/fallback rules | High | Status field evidence | Event cancellation | PP-05 |
| event_modules | Event sections/modules | Public modules safe | Public module fields | Medium | Module evidence | Deletion/cancel | PP-05 |
| share_groups/share_group_members | Group visibility | Private/group-only data protected | Group-only contract | High | Group RLS/RPC evidence | Deletion/group removal | PP-07 |
| profiles | Personal public fields | Public profile if accepted | Field classification | High | Profile evidence | Redaction | PP-03 |
| user_profiles | Mirror profile data | Same as profile | Sync/suppression | High | Table evidence | Redaction | PP-03 |
| host persona fields | Organizer name/bio/avatar | Host public identity | Persona vs personal split | High | Persona evidence | Host deletion/transfer | PP-03 |
| venues | Business profile/location | Venue public if accepted | Address/business field contract | High | Venue evidence | Venue owner deletion | PP-05 |
| venue media | Venue photos | Public if accepted | Media/license/storage | High | Bucket/media evidence | Media deletion | PP-09 |
| event media | Event gallery/highlights | Public only where allowed | Uploader/moderation fields | High | Media/storage evidence | Media deletion | PP-09 |
| storage buckets/public URLs | Object URLs | URL behavior understood | Public/signed semantics | High | Bucket policy evidence | Object deletion | PP-09 |
| tickets | Entitlement status | Not broadly public | Public-safe verification only | High | Ticket/RPC evidence | Commerce state | PP-04 |
| ticket claims | Claim status/token | Token opens safe claim view | Token and fields contract | High | Claim evidence | Claim retention/commerce | PP-04 |
| check-in proof | Proof state | Safe verification | Proof field contract | High | Proof RPC evidence | Proof retention/commerce | PP-08 |
| reservations | Reservation state | Usually not public unless accepted | Paid/request/public boundary | High | Reservation evidence | Commerce retention | PP-04 |
| commerce orders/payment state | Amount/provider/status | Never public by default | Suppression/public summary only | Very high | Commerce evidence | Retention/payment | PP-04 |
| social graph/blocks/mutes/follows | Relationships | Context-dependent | Visibility side effects | High | Social evidence | Account deletion | PP-07 |
| messages/deep links if any | Private messages | Not public | Deep-link reauth | High | DM evidence | DM deletion | PP-10 |
| reports/moderation evidence | Report details | Never public by default | Evidence retention/suppression | High | Moderation evidence | Safety retention | PP-07 |
| notifications/deep links | Previews/links | No private leakage | Preview/deep-link rules | High | Notification evidence | Deletion/commerce | PP-06 |
| support/legal public pages | Legal/support copy | Public info only | Placeholder/canonical copy | High | Route evidence | Policy copy | PP-02 |

## 25. Policy-to-Visibility Mismatch Register

| Copy/policy signal | Missing visibility contract decision | Risk | Owner | Recommended next action |
|---|---|---|---|---|
| Public profile/media visibility vs deletion/redaction unknown | Public field and redacted fallback model. | Privacy-sensitive | Product + legal | Decide public-safe profile/media contract. |
| Web legal placeholders/public route uncertainty | Legal/support route canonical status. | Legal/compliance-sensitive | Legal + public web | Verify deployed routes and copy source. |
| Public event/share visibility vs private/group/invite rules | Suppression model for restricted events. | Privacy-sensitive | Product + security | Define backend-authoritative visibility. |
| Public media deletion vs storage URL behavior | DB deletion vs object/public URL behavior. | Privacy-sensitive | Media + security | Verify bucket/object behavior. |
| Notification/deep link previews vs private visibility | Preview and deep-link re-check semantics. | Privacy-sensitive | Notifications + product | Link to PP-06 decisions. |
| Claim/check-in verification public-safe fields unknown | Public proof field contract. | Revenue-sensitive, privacy-sensitive | Commerce + staff/host | Define token/QR public field rules. |
| Cancelled/refunded/revoked ticket visibility unknown | Invalid-state public behavior. | Revenue-sensitive | Commerce + product | Link to PP-04 ticket state contract. |
| Moderated/taken-down content public behavior unknown | Takedown/suppression/fallback model. | Trust/safety-sensitive | Trust/safety + legal | Link to PP-07 workflow. |
| Support/admin takedown process unknown | Manual suppression/restore authority. | Operational/admin-sensitive | Support + ops | Link to PP-08 auditability. |

## 26. Implementation-without-Visibility-Contract Register

| Existing technical/product surface | Missing visibility/suppression contract | Risk | Owner | Recommended next action |
|---|---|---|---|---|
| Public event route | Lifecycle/private/group/deleted/cancelled suppression. | Privacy-sensitive | Product + public web | Define event visibility decision. |
| Share links | Token and restricted event behavior. | Privacy-sensitive | Product | Define share fallback. |
| Feed/search queries | Detail/share parity. | Privacy-sensitive | Product + search | Create feed/search matrix. |
| Public profile/persona | Public-safe identity fields. | Privacy-sensitive | Profile/product | Define field classification. |
| Venue public pages | Business/address/media visibility. | Privacy-sensitive | Venue/product | Define venue public fields. |
| Public storage buckets | Object visibility and URL lifetime. | Privacy-sensitive | Media + security | Verify and decide public URL model. |
| Media gallery/memory wall | Uploader identity, moderation, deletion. | Privacy-sensitive | Media | Define media visibility. |
| Claim links | Claim token and status exposure. | Revenue-sensitive | Commerce | Define claim public-safe fields. |
| Check-in proof/public verification | Proof display and invalid states. | Revenue-sensitive | Commerce + staff | Define verification contract. |
| Group visibility | Private/group-only public suppression. | Privacy-sensitive | Social/product | Define group checks. |
| Block/mute visibility effects | Whether visibility or interaction changes. | Trust/safety-sensitive | Trust/safety + social | Decide block/mute effects. |
| Dashboard hide/moderate controls | Public suppression authority. | Operational-sensitive | Host/support | Define authority/audit. |
| Support/admin suppression | Manual takedown/restore process. | Operational/admin-sensitive | Ops + support | Define process and auditability. |

## 27. PP-01 Evidence Dependencies

PP-05 needs PP-01 evidence for:

- Public route backend dependencies.
- Production RLS for public/direct-access tables.
- Public RPC bodies, grants, and `search_path`.
- Storage public/private status.
- Public and signed URL behavior.
- Edge/public web deployment state.
- Lifecycle/status fields.
- Deletion/moderation flags.
- Claim/check-in public verification behavior.
- Group/social policy behavior.

## 28. PP-02 Policy Copy Dependencies

PP-05 must respect PP-02 constraints:

- Public privacy/profile/media/event claims must match behavior.
- Legal/support public pages must not remain placeholder unless accepted.
- Deletion, media, and support copy must not overpromise public suppression.
- Notification and deep-link copy must match private preview behavior.
- No final public/legal copy should be treated as approved until owner approval.

## 29. PP-03 Deletion / Data Request Dependencies

PP-05 must respect PP-03 constraints:

- Account deletion must define public profile, persona, event, and media fallback.
- Redaction and suppression are distinct.
- Public media URL behavior must be defined.
- Data export and deletion requests may affect visible data without deleting retained records.
- Support privacy request process may require public suppression authority.

## 30. PP-04 Commerce / Refund / Payment Dependencies

PP-05 must respect PP-04 constraints:

- Public claim, ticket, and check-in verification must not expose private payment/order data.
- Refunded, cancelled, revoked, expired, transferred, and claimed tickets need public behavior.
- Event cancellation/refund state affects public event visibility.
- Wallet, QR, claim, and verification copy must match entitlement state.
- Host sales visibility is not public visibility.

## 31. Product Decision Dependency Checklist

- Public field contract.
- Public event status visibility.
- Share route behavior.
- Feed/search eligibility.
- Public profile/persona model.
- Media/public URL model.
- Claim/check-in public-safe model.
- Group/private/invite visibility model.
- Deletion/redaction fallback model.
- Moderation/takedown behavior.
- Commerce-state public behavior.
- Support/admin suppression powers.
- Beta vs public launch scope.

## 32. Legal Review Dependency Checklist

- Public profile, media, and event visibility claims.
- User content/media license and public display.
- Deletion, redaction, and suppression promises.
- Public support/legal page copy.
- Takedown, moderation, and appeal public behavior.
- Ticket, claim, and check-in public verification language.
- Privacy implications of public URLs.
- Search/indexing/public metadata if applicable.

## 33. Risk Priority Matrix

| Priority candidate | Items | Rationale |
|---|---|---|
| Candidate P0 | None assigned by this pack. | Current handbook evidence does not support P0 without production verification. |
| Candidate P1 | Public/private event suppression; public profile/media deletion mismatch; public storage URL behavior; public claim/check-in verification fields; feed/search/detail parity; RLS/RPC verification for public routes. | These can affect privacy-sensitive public exposure and launch trust. |
| Candidate P2 | Social/block/group visibility side effects; moderation/takedown public behavior; OpenGraph/preview/caching/indexing; venue/business public field contract. | Important beta/pre-scale decisions with incomplete evidence. |
| Candidate P3 | Copy polish and documentation after decisions. | Lower-risk after visibility model is accepted. |
| Unknown / Needs verification | Deployed public routes, public buckets, RLS/RPC bodies, signed URL behavior, feed/search parity, group/private behavior. | Do not convert to patch work before PP-01 evidence and owner decisions. |

## 34. Recommended Decision Records

- Public Visibility Contract Decision.
- Public-Safe Field Classification Decision.
- Event Lifecycle Public Visibility Decision.
- Profile/Persona Public Visibility Decision.
- Media/Public URL Visibility Decision.
- Feed/Search Suppression Decision.
- Share/Deep Link Visibility Decision.
- Claim/Check-In Public Verification Decision.
- Social/Group/Invite Visibility Decision.
- Moderation/Takedown Public Suppression Decision.

## 35. Dependency Map to Later Patch Plan Groups

PP-05 depends on PP-01, PP-02, PP-03, and PP-04.

| Later pack | PP-05 dependency |
|---|---|
| PP-06 Notification/Diagnostics Privacy Pack | Notification preview/deep-link behavior and public/private route checks. |
| PP-07 Abuse/Moderation Workflow Pack | Moderation, takedown, appeal, block/mute, and report visibility effects. |
| PP-08 Ops/Admin Support Auditability Pack | Manual suppression, restore, legal/privacy request, and audit authority. |
| PP-09 Media Storage Lifecycle Pack | Public bucket, object deletion, signed URL, cache, and media suppression behavior. |
| PP-10 Messaging Privacy Lifecycle Pack | Private-message deep links, support/report visibility, and public route re-checks. |

## 36. PP-05 Output Artifacts

Recommended artifacts after execution, not created now:

- `PublicVisibilityContractDecision.md`
- `PublicSafeFieldClassificationMatrix.md`
- `PublicRouteVerificationReport.md`
- `FeedSearchSuppressionMatrix.md`
- `ShareDeepLinkVisibilityDecision.md`
- `ClaimCheckInPublicVerificationDecision.md`
- `MediaPublicUrlVisibilityDecision.md`
- `PublicVisibilityImplementationReadinessChecklist.md`

## 37. Execution Preconditions

Before executing PP-05:

- Product owner assigned.
- Legal/privacy owner assigned.
- Public web/mobile owner assigned.
- Backend/security owner assigned.
- PP-01 production evidence available where needed.
- PP-02 copy constraints available.
- PP-03 deletion/data-request constraints available.
- PP-04 commerce constraints available.
- Launch scope defined.
- No production changes planned as part of decision work.
- No public route changes.
- No SQL/RLS/RPC/storage changes.
- No final legal claims made.
- Sanitized evidence rules accepted.

## 38. Explicitly Blocked Actions

PP-05 blocks:

- Public route changes.
- Feed/search behavior changes.
- RLS/RPC/storage changes.
- Production access.
- SQL or Supabase CLI.
- Migrations.
- Source code changes.
- Storage object deletion.
- Policy publication.
- Legal compliance claims.
- Immediate patch authorization.

## 39. Unknown / Needs Verification Items

- Which public routes are deployed.
- Which web/mobile deep links are publicly reachable.
- Which event states are public.
- Which profile/persona fields are public.
- Which venue/business fields are public.
- Which media objects and storage URLs are public.
- Whether public storage URLs are intentionally persistent.
- What happens to public links after deletion/redaction.
- How private/group/invite events behave through share links.
- Which fields appear on claim, ticket, QR, and check-in verification pages.
- What happens after refund, cancel, revoke, expire, transfer, or claim.
- Whether feed/search results match detail/share visibility.
- How block/mute/group membership affects public visibility.
- How moderated/taken-down content is suppressed.
- Who can manually suppress or restore public content.

## 40. Acceptance Criteria for PP-05 Completion

PP-05 is complete only when:

- Public surface inventory is confirmed.
- Public-safe field model is accepted or explicitly deferred.
- Lifecycle visibility model is accepted or explicitly deferred.
- Feed/search/share/detail parity model is accepted or explicitly deferred.
- Profile/persona visibility model is accepted or explicitly deferred.
- Media/public URL visibility model is accepted or explicitly deferred.
- Claim/check-in public verification model is accepted or explicitly deferred.
- Social/group/invite visibility model is accepted or explicitly deferred.
- Deletion/redaction public fallback model is accepted or explicitly deferred.
- Moderation/takedown public suppression model is accepted or explicitly deferred.
- Commerce-state public visibility model is accepted or explicitly deferred.
- PP-01 evidence dependencies are linked.
- PP-02 copy constraints are linked.
- PP-03 deletion constraints are linked.
- PP-04 commerce constraints are linked.
- Product owner decisions are assigned.
- Legal/privacy review dependencies are assigned.
- Public web/mobile owner is assigned.
- Backend/security owner is assigned.
- Follow-up PP-06 through PP-10 groups are updated or explicitly marked unchanged based on public visibility decisions.
- No final public/legal/privacy text is treated as approved unless the responsible owner confirms it.

## 41. Recommended Follow-Up Reports

Recommended follow-up reports after execution:

- Public Visibility Contract Decision.
- Public-Safe Field Classification Matrix.
- Public Route Verification Report.
- Feed/Search Suppression Matrix.
- Share/Deep Link Visibility Decision.
- Claim/Check-In Public Verification Decision.
- Media Public URL Visibility Decision.
- Social/Group/Invite Visibility Decision.
- Public Visibility Implementation Readiness Checklist.

## 42. Non-Goals

- No code changes.
- No SQL or migrations.
- No production execution.
- No public route verification execution.
- No public route changes.
- No feed/search changes.
- No RLS/RPC/storage changes.
- No storage object deletion.
- No legal advice.
- No compliance claim.
- No launch readiness claim.
- No final public/legal/privacy copy.
- No immediate patch authorization.
- No source-code re-audit.

## 43. Open Questions

- Which routes are publicly reachable?
- Which event states are public?
- Which profile/persona fields are public?
- Which media objects are public?
- Are public storage URLs intentionally permanent?
- What happens to public links after deletion?
- What happens to share links for private/group/invite events?
- What fields are shown on ticket/claim/check-in verification pages?
- What happens after refund, cancel, revoke, transfer, or claim?
- Do feed/search results match detail/share visibility?
- How do blocks, mutes, and groups affect public visibility?
- How is moderated or taken-down content suppressed?
- Who can manually suppress or restore public content?

## 44. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No public route/feed/search/RLS/RPC/storage action was executed.
- No files were staged or committed.
- Only `08_PatchPlans/PP05PublicVisibilitySuppressionPack.md` was created/modified.

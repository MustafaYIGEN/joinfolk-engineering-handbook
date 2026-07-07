# PP-09 Media / Storage Lifecycle Pack

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook audit synthesis only
- canonical: false
- Execution status: Not executed
- Legal status: Engineering planning only; not legal advice
- Media/storage status: Not executed / Storage behavior not verified
- Deletion status: Not executed / Object deletion not verified

## 2. Purpose

This is a decision-pack for defining JoinFolk media and storage lifecycle semantics before implementation work begins.

It is not storage implementation, not bucket policy implementation, not media deletion execution, not production verification, not legal advice, and not patch authorization.

## 3. Evidence Boundary

This document is based only on handbook audits, the Release Readiness / Production Hardening Gap Register, PP-01, PP-02, PP-03, PP-04, PP-05, PP-06, PP-07, and PP-08.

No source-code inspection, production connection, Supabase CLI, SQL, builds, tests, storage verification, bucket verification, object deletion, media upload, media inspection, evidence export, legal review, policy-copy modification, or final copy drafting was performed.

## 4. PP-09 Scope Summary

PP-09 covers:

- Media surface inventory.
- Storage bucket and object inventory.
- Media lifecycle taxonomy.
- Upload and ownership model.
- Public/private storage model.
- Public URL, signed URL, cache, CDN, OpenGraph, and indexing model.
- Database row versus storage object model.
- Event, gallery, memory wall, venue, avatar, poster, profile, and video media models.
- Host hide, user delete, support takedown, moderation, appeal, and restoration model.
- Account deletion and data request media model.
- Commerce, ticket, claim, refund/dispute, and check-in proof media dependencies where applicable.
- Notification, deep-link, and preview media dependencies.
- Support/admin media access model.
- Retention, redaction, deletion, and export constraints.
- Backend/RPC/RLS/storage verification dependencies.
- PP-02 policy, PP-03 deletion, PP-04 commerce, PP-05 visibility, PP-06 notification, PP-07 moderation, and PP-08 ops/admin dependencies.
- Dependency mapping to PP-10.

PP-09 does not execute PP-01 and does not authorize media upload changes, storage bucket changes, object deletion changes, RLS changes, RPC changes, media moderation changes, copy changes, database changes, CDN/cache changes, or production changes.

## 5. Source Register Coverage

| Release gap | Why PP-09 covers it | PP-09 limitation |
|---|---|---|
| RR-GAP-003 | Media tables and storage-related direct access may rely on RLS and storage policies. | PP-09 does not verify production policies. |
| RR-GAP-009 | Public media, share images, public URLs, and media route suppression must match public visibility rules. | PP-05 owns the broader public visibility contract. |
| RR-GAP-012 | Media hide/delete/moderate, public highlights, storage object deletion, URL invalidation, and content-license policy are core PP-09 scope. | PP-09 defines decisions, not implementation. |
| RR-GAP-013 | Avatars, profile media, host persona media, redaction, and ownership transfer affect storage lifecycle. | PP-03 owns account deletion decisions. |
| RR-GAP-018 | Support/admin media access, takedown, restore, and break-glass operations require authority and auditability. | PP-08 owns support/admin role detail. |
| RR-GAP-019 | Media reports, takedowns, appeals, evidence retention, and restoration depend on the abuse workflow. | PP-07 owns trust/safety workflow detail. |
| RR-GAP-020 | Account deletion, data export, storage deletion, media retention, and redaction are unresolved. | PP-09 does not provide legal advice. |
| RR-GAP-021 | Public media, privacy, deletion, and user-content copy must not overpromise unverified behavior. | PP-02 owns legal/public copy reconciliation. |
| RR-GAP-022 | Commerce proof, ticket proof, refund/dispute evidence, or check-in proof media may need retention constraints. | PP-04 owns commerce contract detail. |
| RR-GAP-023 | Media/storage changes require accepted product decisions before patch work. | PP-09 recommends decision records only. |

## 6. Media / Storage Lifecycle Problem Statement

Media lifecycle is not one delete button. It crosses:

- Database row.
- Storage object.
- Bucket.
- Public bucket.
- Private bucket.
- Signed URL.
- Public URL.
- Cache/CDN.
- OpenGraph preview.
- Uploader.
- Host.
- Participant.
- Checked-in user.
- Venue/business owner.
- Support/admin.
- Moderation.
- Takedown.
- Appeal.
- Restoration.
- Account deletion.
- Data export.
- Redaction.
- Public visibility.
- Object retention.
- Audit evidence.
- Legal/privacy copy.

Database row deletion, row hiding, product suppression, object deletion, public URL invalidation, cache expiry, and legal retention are separate decisions.

## 7. Media Surface Inventory

| Surface | Example user expectation | Data/storage domains affected | Owner | PP-01 evidence dependency | PP-02 copy dependency | PP-03 deletion dependency | PP-05 visibility dependency | PP-07 moderation dependency | PP-08 support/admin dependency | Current status | Recommended PP-09 decision need |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Event media/gallery | Event photos appear only where allowed. | `event_media`, storage objects, event status | Product / Media | Bucket, table, RLS, and URL behavior | Public content and privacy copy | Uploader deletion and event retention | Feed/detail/share suppression | Report/takedown workflow | Support takedown audit | Unknown / Needs verification | Define lifecycle per event state. |
| Memory wall/highlights | Highlighted media follows attendee/public rules. | Event media, highlight flags, storage URLs | Product / Media | Highlight data source and policies | Public display copy | De-attribution on deletion | Public-safe media rules | Host/support moderation | Override authority | Unknown / Needs verification | Decide participant versus public model. |
| Venue media | Venue gallery is controlled by accepted venue authority. | `venue_media`, venue objects | Venue / Media | Table, bucket, and owner policy evidence | Business/media copy | Owner deletion handling | Venue page visibility | Venue media reports | Support correction/takedown | Unknown / Needs verification | Define owner/support authority and retention. |
| Business/venue gallery | Business images remain accurate and safe. | Venue media, public page assets | Venue / Product | Bucket visibility and row state | Business listing copy | Business owner removal | Public listing suppression | Takedown/appeal workflow | Support audit trail | Unknown / Needs verification | Decide public gallery contract. |
| Event poster | Poster matches event lifecycle and deletion state. | Event poster field/object | Event owner / Media | Storage path and public URL evidence | Marketing/public copy | Host deletion behavior | Event detail/share fallback | Takedown handling | Support replacement audit | Unknown / Needs verification | Define replacement and suppression behavior. |
| Venue poster/cover | Cover image follows venue authority. | Venue poster/cover object | Venue owner | Bucket and row evidence | Business profile copy | Owner deletion behavior | Venue page fallback | Moderation behavior | Support override | Unknown / Needs verification | Define cover lifecycle. |
| Profile avatar | Avatar can be hidden/redacted when profile changes. | Profile avatar fields/objects | Identity / Privacy | Profile table and storage evidence | Privacy/profile copy | Account deletion/redaction | Public profile rules | Avatar reports | Support correction | Unknown / Needs verification | Decide avatar deletion and fallback. |
| Host persona avatar | Host identity remains separate from personal profile. | Host persona media | Host / Privacy | Persona table and storage evidence | Host identity copy | Persona deletion/transfer | Public host profile rules | Persona abuse workflow | Host transfer support | Unknown / Needs verification | Define persona media retention. |
| Organizer/media attribution | Uploader attribution may be public or redacted. | Media metadata, profile/persona refs | Product / Privacy | Metadata and RLS evidence | Public attribution copy | De-attribution model | Public-safe field matrix | Report evidence handling | Admin correction | Unknown / Needs verification | Decide attribution fields. |
| Uploaded video | Large media may need special storage/retention rules. | Video objects, thumbnails, previews | Media / Storage | Bucket and processing evidence | Media copy | Deletion/export scope | Public preview rules | Moderation workflow | Support access audit | Unknown / Needs verification | Decide video lifecycle and cost controls. |
| Check-in/proof media if any | Proof media is not public unless accepted. | Check-in proof, ticket evidence media | Commerce / Staff | Table/storage/function evidence | Ticket/check-in copy | Retention exception | Verification page rules | Fraud/safety review | Admin access audit | Unknown / Needs verification | Decide proof media privacy and retention. |
| Report/moderation evidence media if any | Evidence is protected and retained only as accepted. | Report attachments, moderation evidence | Trust/Safety | Report/evidence storage evidence | Trust/safety copy | Safety retention exception | Public suppression | Appeal/takedown workflow | Evidence access audit | Unknown / Needs verification | Define evidence access and retention. |
| Support/admin media evidence if any | Support media is private and auditable. | Support attachments, audit refs | Support / Ops | Support storage and RLS evidence | Support copy | Support record retention | Not public | Escalation workflow | Least privilege | Unknown / Needs verification | Define support media handling. |
| Public share/OpenGraph media | Share previews do not expose suppressed media. | OG images, posters, thumbnails | Web / Product | Public route and cache evidence | Public route copy | Deletion/suppression effects | Share visibility | Takedown behavior | Manual purge authority | Unknown / Needs verification | Decide preview and stale-cache stance. |
| Notification preview media if any | Push/in-app previews do not leak private media. | Notification payloads, image URLs | Notification / Privacy | Delivery payload evidence | Notification privacy copy | Account deletion effects | Deep-link reauthorization | Moderation notification rules | Support visibility | Unknown / Needs verification | Define preview media boundaries. |

## 8. Storage Bucket / Object Inventory

| Bucket/object class | Example objects | Expected visibility | Owner | Current evidence status | Verification needed | Decision needed |
|---|---|---|---|---|---|---|
| Posters if present | Event poster objects | Public only if event visibility permits | Media / Backend | Unknown / Needs verification | Bucket existence, public status, object paths, policies | Poster lifecycle and suppression. |
| Venue-posters if present | Venue cover/poster objects | Public only if venue page is public | Venue / Backend | Unknown / Needs verification | Bucket existence, public status, owner writes | Venue poster ownership and cleanup. |
| Venue-media if present | Venue gallery objects | Public, authenticated, or host-only as accepted | Venue / Media | Unknown / Needs verification | Bucket policies, row linkage, public URL behavior | Venue media public/private contract. |
| Event media bucket if present | Gallery, memory wall, highlights | Public, participant-only, or host-only as accepted | Event / Media | Unknown / Needs verification | Bucket policies, event state linkage, object deletion | Event media lifecycle. |
| Avatars/profile media if present | Profile and host persona avatars | Public only per profile/persona contract | Identity / Privacy | Unknown / Needs verification | Bucket public status, profile links, old-object cleanup | Avatar fallback and deletion. |
| Uploaded videos if present | Videos, thumbnails, previews | Unknown / Needs verification | Media / Storage | Unknown / Needs verification | Storage location, processor state, access policy | Large-media retention and moderation. |
| Moderation/report evidence objects if present | Evidence attachments, snapshots | Support/admin-only unless accepted otherwise | Trust/Safety | Unknown / Needs verification | Private storage, access logs, retention | Evidence retention and access controls. |
| Support/evidence attachments if present | Support screenshots or submitted files | Support/admin-only | Support / Ops | Unknown / Needs verification | Storage location, access policy, auditability | Support evidence lifecycle. |
| Generated thumbnails/previews if present | Resized images, OG images, video thumbnails | Mirrors source media visibility or explicitly differs | Media / Web | Unknown / Needs verification | Generation source, cache, cleanup behavior | Thumbnail and preview deletion behavior. |

## 9. Media State / Lifecycle Taxonomy

- Uploaded: media has been accepted into a storage/object path or product record.
- Active: media is eligible for product use.
- Public: media can appear to anonymous users through accepted routes or URLs.
- Authenticated-only: media requires signed-in access.
- Participant-only: media requires participant, ticket-holder, checked-in, or similar authority.
- Host-only: media is visible to event/venue/host operators.
- Support/admin-only: media is limited to privileged review with auditability.
- Hidden: media is not shown in normal product surfaces, but the row/object may remain.
- Suppressed: media is intentionally excluded from public surfaces.
- Deleted DB row: product row is removed or logically deleted.
- Deleted storage object: physical object is removed from storage.
- Orphaned object: object exists without a valid product row.
- Retained object: object remains for accepted audit, safety, support, payment, or legal reasons.
- Moderated/taken down: media is removed from product visibility because of a moderation action.
- Pending appeal: restoration or final takedown decision is unresolved.
- Restored: media is returned to an accepted visible state.
- Redacted metadata: identity, captions, paths, or references are removed or masked.
- Public URL reachable: object is reachable by URL regardless of product row visibility.
- Signed URL expired: a previously issued URL is no longer valid by lifetime.
- Cached externally: copies or previews may persist outside product storage.

Do not collapse database visibility with object reachability.

## 10. Upload Intake / Ownership Decision Model

PP-09 must define:

- Who can upload media: owner, host, participant, checked-in user, venue/business owner, staff, support, or admin.
- Which uploaded media requires event, venue, profile, persona, ticket, claim, or check-in context.
- Whether checked-in status is required for memory wall or proof media.
- Which metadata is captured: uploader, owner, event, venue, profile/persona, timestamp, object class, mime type, size, moderation state, and visibility state.
- Whether file type, file size, malware scanning, content safety review, rate limits, or quota rules are required as product/security decisions.
- Whether consent/license signals exist and where they are recorded.

Decision required: accepted upload ownership and authority model by media class.

## 11. Public vs Private Storage Decision Model

PP-09 must distinguish:

- Public bucket status.
- Private bucket status.
- Authenticated storage reads.
- Signed URL reads.
- Owner, participant, host, staff, support, and admin reads.
- Product-visible public media.
- Object-level URL reachability.

Decision required: which media classes are acceptable in public buckets, which require private buckets, and which require signed URL access with accepted lifetimes.

## 12. Public URL / Signed URL / Cache Decision Model

PP-09 must define behavior for:

- Permanent or long-lived public URLs.
- Signed URLs and expiration.
- Signed URL generation authority.
- Browser cache.
- CDN cache.
- Social/OpenGraph preview cache.
- Search indexing where applicable.
- Stale previews after media suppression, deletion, takedown, or event visibility changes.

Decision required: what public URL exposure can be guaranteed, what is best-effort, and what must not be promised in public or support copy until verified.

## 13. Database Row vs Storage Object Decision Model

PP-09 must define how each action affects:

- Media row active state.
- Media row hidden state.
- Media row deleted state.
- Storage object retained state.
- Storage object deleted state.
- Orphaned object state.
- Row restoration.
- Object restoration, if possible.

Decision required: product-visible deletion versus physical deletion contract for each media class.

## 14. Event Media / Gallery / Memory Wall Decision Model

PP-09 must map:

- Event media.
- Live or checked-in upload.
- Participant/gallery visibility.
- Public highlights.
- Memory wall.
- Host hide/unhide.
- User delete.
- Ended event behavior.
- Archived event behavior.
- Cancelled or deleted event behavior.

Decision required: event media lifecycle and visibility per event lifecycle state.

## 15. Venue Media / Business Media Decision Model

PP-09 must map:

- Venue gallery.
- Venue cover/poster.
- Business owner upload.
- Venue public page.
- Venue reservation context where applicable.
- Venue deletion or owner deletion.
- Support correction.

Decision required: venue/business media ownership, public display, replacement, deletion, and support authority model.

## 16. Poster / Avatar / Profile Media Decision Model

PP-09 must map:

- Event poster.
- Venue poster.
- Profile avatar.
- Host persona avatar.
- Organizer avatar.
- Old/replaced objects.
- Public fallback images.
- Deleted or redacted user state.
- Host identity transfer.

Decision required: replacement cleanup, public fallback, account deletion behavior, and persona/profile separation.

## 17. Video / Large Media Decision Model

PP-09 must map:

- Uploaded video.
- Generated thumbnails or previews if any.
- Transcoding or processing if any.
- Storage size and cost.
- Public/private preview behavior.
- Moderation and takedown behavior.
- Deletion and retention.

Decision required: large media lifecycle, retention, cleanup, and moderation scope.

## 18. Media Metadata / Public-Safe Field Decision Model

PP-09 must classify:

- Uploader id.
- Uploader display name/avatar.
- Host/persona attribution.
- Upload timestamp.
- Event and venue references.
- Storage path or URL.
- Mime type and size.
- Captions/comments if any.
- Moderation state.
- Report/evidence state.
- Support/admin notes.

Decision required: which metadata is public-safe, authenticated-only, participant-only, host-only, support/admin-only, or never public.

## 19. Host Hide / User Delete / Support Takedown Decision Model

PP-09 must distinguish:

- Uploader delete.
- Host hide/unhide.
- Support/admin takedown.
- Public suppression.
- Storage object deletion.
- Audit log side effect.
- Restoration or rollback.

Decision required: authority matrix and whether each action affects the database row, storage object, public URL, cache, support evidence, or all of them.

## 20. Moderation / Appeal / Restoration Decision Model

PP-09 must map:

- Report pending.
- Immediate takedown.
- Hidden media.
- Appeal pending.
- Appeal accepted.
- Appeal denied.
- Restoration.
- Evidence retention.
- Object deletion after final decision.

Decision required: media moderation workflow and object retention during and after review.

## 21. Account Deletion / Data Request Media Decision Model

PP-09 must map:

- Deleted uploader.
- Deleted host.
- Deleted venue owner.
- Deleted event owner.
- Profile/avatar deletion.
- Event media uploaded by deleted user.
- Host/persona media for deleted or transferred accounts.
- Media export request.
- Metadata redaction.
- Storage object deletion or retention.

Decision required: which media are deleted, retained, redacted, de-attributed, suppressed, exported, or excluded from export.

## 22. Commerce / Ticket / Check-In Proof Media Decision Model

PP-09 must map commerce-related media only if present:

- Check-in proof media.
- Ticket proof attachments.
- Refund/dispute evidence media.
- Claim/transfer proof media.
- Receipt or confirmation media artifacts.
- Support/admin commerce evidence.

Decision required: revenue/support evidence retention, public visibility prohibition unless explicitly accepted, entitlement-state reauthorization, and privacy boundaries.

## 23. Notification / Deep Link / Preview Media Decision Model

PP-09 must map:

- Notification image preview.
- Deep-linked media.
- Private preview setting.
- Public share preview.
- Removed, hidden, moderated, private, or deleted media link.
- Diagnostics or delivery logs containing media references.

Decision required: preview suppression, payload minimization, and reauthorization rules for media links.

## 24. Support / Ops / Admin Media Access Decision Model

PP-09 must map:

- Support media read access.
- Media moderation override.
- Storage object metadata access.
- Report evidence media.
- Support evidence media.
- Audit logs.
- Break-glass media operations.

Decision required: least privilege, reason codes, evidence references, approval requirements, auditability, and retention for support/admin media access.

## 25. CDN / Cache / OpenGraph / Search Indexing Decision Model

PP-09 must map:

- CDN caches.
- Social crawlers.
- OpenGraph image previews.
- Browser cache.
- Search indexing.
- Stale deleted media previews.
- Stale moderated media previews.
- Purge or invalidation capability if any.

Decision required: what can be guaranteed, what is best-effort, and what copy must not overpromise.

## 26. Retention / Redaction / Deletion / Export Decision Model

PP-09 must define treatment for:

- Database media rows.
- Storage objects.
- Thumbnails and previews.
- Public URLs.
- Signed URLs.
- Moderation evidence.
- Audit logs.
- Support records.
- Uploader attribution.
- Object paths and metadata.

Decision required: what survives account deletion, what is exportable, what is redacted, what is physically deleted, and what is retained for safety, audit, payment, support, or legal reasons after owner review.

## 27. Backend / RPC / RLS / Storage Verification Dependencies

PP-01 must provide production evidence for:

- Storage buckets.
- Bucket public/private status.
- Storage policies.
- Media tables.
- Venue media tables.
- Profile/avatar fields.
- Event poster/media fields.
- Signed URL generators.
- Public URL generation.
- Upload, delete, hide, unhide, and moderate RPCs/functions if present.
- Host moderation RPCs/functions if present.
- Support/admin media functions if present.
- Grants, search path, and `SECURITY DEFINER` posture where relevant.
- RLS policies for media tables.
- Storage object deletion behavior.
- Orphan object cleanup if any.
- Edge Functions or media processors if any.

No executable SQL, storage commands, or implementation steps are authorized by this pack.

## 28. Media / Storage Data Domain Inventory Matrix

| Domain | Example data | User expectation | Lifecycle decision needed | Legal/product/security review need | PP-01 evidence need | PP-03/PP-05/PP-07/PP-08 dependency | Later pack dependency |
|---|---|---|---|---|---|---|---|
| `event_media` | Event gallery rows | Media follows event visibility and deletion rules. | Hide/delete/retain/export behavior | Product/privacy/security | Table, RLS, storage linkage | Deletion, visibility, moderation, support | PP-10 only if DM media references exist. |
| `venue_media` | Venue gallery rows | Business media is controlled by accepted venue authority. | Owner/support deletion and public status | Product/legal/security | Table, policies, object linkage | Visibility and support authority | None known. |
| Posters/event poster objects | Event poster files | Posters disappear or fallback when event is suppressed. | Replacement and suppression behavior | Product/privacy | Bucket/path/public URL evidence | Visibility and support override | None known. |
| Venue poster/cover objects | Venue cover images | Venue cover follows venue visibility. | Owner deletion and fallback | Product/legal | Bucket/path evidence | Visibility and support correction | None known. |
| Avatars/profile media | User avatars | Avatar redacts on accepted account deletion model. | Delete, retain, or fallback | Privacy/legal/security | Profile fields and storage evidence | Deletion and public identity | None known. |
| Host persona media | Host avatars/bios media | Host identity remains separate from personal profile. | Transfer/deletion/de-attribution | Product/privacy | Persona media evidence | Deletion, visibility, support transfer | None known. |
| Uploaded videos | Video files | Videos follow stricter lifecycle due to size and sensitivity. | Retention, moderation, cleanup | Product/security/privacy | Bucket/processor evidence | Moderation and support access | None known. |
| Thumbnails/previews | Generated media | Previews disappear with source media or have accepted caveats. | Cleanup and cache behavior | Product/security | Generation and storage evidence | Visibility and takedown | None known. |
| Public bucket objects | Publicly reachable files | Public reachability is intentional and disclosed. | Public/private bucket acceptance | Legal/privacy/security | Bucket public status and policies | Visibility and deletion | None known. |
| Signed URLs | Temporary media links | Access expires and reauthorization is clear. | Lifetime and generation authority | Security/privacy | Generator and policy evidence | Deep-link visibility | PP-10 if DM attachments use signed URLs. |
| Storage paths/object keys | Object identifiers | Paths do not leak private identifiers. | Public-safe path model | Security/privacy | Object path conventions | Support access | None known. |
| Media metadata/uploader identity | Uploader id/name/time | Attribution is public only if accepted. | Public-safe metadata model | Legal/privacy | Metadata schema and RLS | Deletion/redaction, visibility | PP-10 if DM media metadata exists. |
| Moderation/report evidence media | Evidence attachments | Evidence is protected and retained only as accepted. | Retention, redaction, export exclusions | Legal/trust-safety/security | Storage and access evidence | Deletion, moderation, support audit | PP-10 if message evidence media exists. |
| Support/admin media action logs | Takedown/restore logs | Privileged actions are auditable. | Required log fields and retention | Legal/privacy/security | Audit table/function evidence | Support/admin auditability | None known. |
| Cache/CDN/OpenGraph previews | Social preview images | Stale media handling is not overpromised. | Purge/best-effort model | Legal/product/security | Public route/cache evidence | Public visibility | None known. |

## 29. Policy-to-Storage Mismatch Register

| Copy/policy signal | Missing storage lifecycle decision | Risk | Owner | Recommended next action |
|---|---|---|---|---|
| Delete media/account deletion | Whether DB deletion also deletes storage objects is unknown. | Candidate P1 | Product / Privacy / Storage | Decide deletion versus retention by media class after PP-01. |
| Public media visibility | Public bucket reachability may differ from product visibility. | Candidate P1 | Product / Security | Define public bucket contract and route suppression model. |
| Private media copy | Signed URL behavior and lifetime are unverified. | Candidate P2 | Security / Privacy | Verify signed URL generation and decide copy limits. |
| Moderation/takedown | Takedown may hide rows without deleting objects. | Candidate P1 | Trust/Safety / Media | Define takedown object handling and appeal retention. |
| Support/admin media access | Privileged media inspection/auditability is unknown. | Candidate P1 | Ops / Security | Define least-privilege access and audit requirements. |
| Venue/business media public copy | Owner deletion and public business media retention are unresolved. | Candidate P2 | Venue / Legal | Decide venue ownership and deletion behavior. |
| Avatar/persona redaction | Object cleanup after profile/persona redaction is unknown. | Candidate P2 | Privacy / Identity | Define avatar/persona fallback and storage cleanup. |
| OpenGraph/cache/indexing deletion promise | Stale external previews may remain reachable. | Candidate P2 | Web / Legal / Security | Decide best-effort language and verification needs. |

## 30. Implementation-without-Storage-Contract Register

| Existing technical/product surface | Missing media/storage lifecycle contract | Risk | Owner | Recommended next action |
|---|---|---|---|---|
| Public buckets | Accepted semantics for public object reachability. | Candidate P1 | Security / Storage | Verify and decide public/private bucket model. |
| Signed URL use | Lifetime, issuer authority, and revocation expectations. | Candidate P2 | Security / Backend | Verify generation paths and decide access model. |
| Event media/gallery | Hide/delete/retain/export behavior. | Candidate P1 | Media / Product | Create event media lifecycle decision. |
| Venue media gallery | Venue owner/support authority and public field contract. | Candidate P2 | Venue / Media | Create venue media lifecycle decision. |
| Avatar/poster replacement | Old object cleanup and fallback behavior. | Candidate P2 | Identity / Media | Decide replacement lifecycle. |
| Host hide/unhide | Difference from support/admin takedown. | Candidate P1 | Host / Trust/Safety | Define authority matrix. |
| Support/admin takedown | Audit, reason code, restoration, and object retention. | Candidate P1 | Ops / Trust/Safety | Link PP-08 authority and PP-07 workflow. |
| Account deletion media handling | Delete, retain, redact, or de-attribute by media class. | Candidate P1 | Privacy / Product | Link PP-03 deletion decision. |
| Orphan storage objects | Cleanup ownership, timing, and evidence retention. | Candidate P2 | Backend / Storage | Verify orphan cleanup existence. |
| OpenGraph/public preview media | Stale preview and cache behavior. | Candidate P2 | Web / Security | Decide cache and preview stance. |

## 31. PP-01 Evidence Dependencies

PP-09 needs PP-01 to provide:

- Production bucket names and public/private state.
- Storage policies.
- Media table schemas and RLS.
- Upload/delete/moderate functions and grants if present.
- Signed URL generation paths.
- Public URL generation paths.
- Object deletion behavior.
- Orphan cleanup behavior if any.
- Support/admin media access paths.
- Deployed public route and OpenGraph behavior.
- Edge/media processor deployment state.

Until PP-01 evidence exists, production media/storage behavior remains Unknown / Needs verification.

## 32. PP-02 Policy Copy Dependencies

PP-09 is constrained by PP-02 because:

- Media/public-content copy must match storage behavior.
- Deletion/privacy copy must not overpromise physical deletion unless verified and accepted.
- User-content/license copy must match public/private media usage.
- Moderation/takedown copy must match the accepted workflow.
- Public URL/cache/crawler limitations need owner-approved wording if mentioned.
- No final media/privacy/legal copy is approved by PP-09.

## 33. PP-03 Deletion / Data Request Dependencies

PP-09 is constrained by PP-03 because:

- Account deletion must define media deletion, retention, de-attribution, and redaction.
- Data export must define media objects versus media metadata.
- Support/privacy requests may require object deletion or public suppression.
- Audit/moderation evidence may require retention exceptions.
- Third-party, uploader, host, event, venue, and support relationships must be handled separately.

## 34. PP-04 Commerce / Refund / Payment Dependencies

PP-09 depends on PP-04 only where commerce media exists or is later confirmed:

- Check-in, ticket, claim, refund, dispute, or provider proof media may require retention.
- Commerce evidence media must not be public by default.
- Support/admin commerce media access requires auditability.
- Ticket/claim media links must re-check entitlement and state.
- Refund/dispute evidence retention must match the accepted commerce contract.

## 35. PP-05 Public Visibility Dependencies

PP-09 is constrained by PP-05 because:

- Public media visibility must match public route, feed, search, share, and detail behavior.
- Public URL reachability is separate from product visibility.
- Deleted, private, group-only, invite-only, hidden, archived, cancelled, moderated, or taken-down media must be suppressed from accepted product surfaces.
- OpenGraph/cache/indexing behavior must not contradict accepted visibility promises.

## 36. PP-06 Notification / Diagnostics Dependencies

PP-09 is constrained by PP-06 because:

- Notification previews must not leak removed, private, group-only, or moderated media.
- Deep links to media must reauthorize at open time.
- Diagnostics containing media references, paths, URLs, or object keys need payload rules.
- Support and delivery logs containing media URLs are privacy-sensitive.

## 37. PP-07 Abuse / Moderation Dependencies

PP-09 is constrained by PP-07 because:

- Media reports, takedown, appeal, and restoration require workflow decisions.
- Report evidence media access must be least-privilege.
- Moderation action does not automatically imply storage object deletion.
- Restoration requires row, object, public URL, and cache state decisions.

## 38. PP-08 Ops / Admin Support Dependencies

PP-09 is constrained by PP-08 because:

- Support/admin media access must be auditable.
- Media override, takedown, restore, and object deletion authority require reason codes and ownership.
- Break-glass media operations require strict process.
- Private storage object metadata access is private-data access.

## 39. Product Decision Dependency Checklist

- Media surface model.
- Bucket public/private model.
- Upload ownership model.
- Public URL/signed URL model.
- Database row versus object deletion model.
- Event media lifecycle.
- Venue media lifecycle.
- Avatar/poster replacement lifecycle.
- Large media/video lifecycle.
- Public-safe metadata model.
- Host hide/user delete/support takedown model.
- Moderation/appeal/restoration model.
- Account deletion/media redaction model.
- Support/admin media access model.
- Cache/OpenGraph/indexing stance.
- Beta versus public launch scope.

## 40. Legal / Privacy / Security Review Dependency Checklist

- Public media visibility.
- User content/license and media reuse.
- Account deletion/media deletion promises.
- Public URL/cache limitations.
- Storage object retention.
- Moderation evidence retention.
- Support/admin media access.
- Profile/avatar/persona redaction.
- Venue/business media ownership.
- Media export/data request behavior.

This checklist is for review routing only and is not legal advice.

## 41. Risk Priority Matrix

| Risk class | Candidate items | Notes |
|---|---|---|
| Candidate P0 | None unless prior evidence supports it. | PP-09 does not assert a P0 production vulnerability. |
| Candidate P1 | Public bucket exposure without accepted contract; media deletion promise versus object retention mismatch; public URL/cache reachability after suppression; support/admin media access without audit; moderation/takedown not deleting or suppressing as promised; account deletion media retention unknown. | Requires PP-01 evidence and product/legal/security decisions. |
| Candidate P2 | Avatar/poster orphan cleanup; venue media ownership deletion; signed URL lifetime uncertainty; notification preview media leakage; OpenGraph stale previews; large media retention/cost. | Likely beta hardening unless launch scope makes it earlier. |
| Candidate P3 | Copy polish and documentation after decisions. | Not a substitute for verification. |
| Unknown / Needs verification | Bucket existence, policy bodies, object deletion behavior, signed URL behavior, cache behavior, media processors, support/admin access paths. | Use this when evidence is incomplete. |

## 42. Recommended Decision Records

- MediaStorageLifecycleDecision
- PublicPrivateBucketDecision
- PublicUrlSignedUrlCacheDecision
- DatabaseRowVsStorageObjectDecision
- EventMediaLifecycleDecision
- VenueMediaLifecycleDecision
- AvatarPosterReplacementDecision
- MediaModerationTakedownDecision
- AccountDeletionMediaRetentionDecision
- SupportAdminMediaAccessDecision

## 43. Dependency Map to Later Patch Plan Groups

PP-09 outputs feed PP-10 Messaging Privacy Lifecycle Pack if message attachments, message media previews, DM evidence media, signed media links, or report evidence tied to DMs exists.

PP-09 depends on PP-01, PP-02, PP-03, PP-04 where commerce media exists, PP-05, PP-06, PP-07, and PP-08.

## 44. PP-09 Output Artifacts

Recommended documents after PP-09 execution, not created by this pack:

- `MediaStorageLifecycleDecision.md`
- `PublicPrivateBucketDecision.md`
- `PublicUrlSignedUrlCacheDecision.md`
- `DatabaseRowVsStorageObjectDecision.md`
- `EventMediaLifecycleDecision.md`
- `VenueMediaLifecycleDecision.md`
- `AvatarPosterReplacementDecision.md`
- `MediaModerationTakedownDecision.md`
- `AccountDeletionMediaRetentionDecision.md`
- `SupportAdminMediaAccessReview.md`
- `MediaStorageImplementationReadinessChecklist.md`

## 45. Execution Preconditions

Before executing PP-09:

- Product owner assigned.
- Legal/privacy owner assigned.
- Security owner assigned.
- Media owner assigned.
- Backend/storage owner assigned.
- Support/admin owner assigned where support access is included.
- Trust/safety owner assigned where moderation is included.
- PP-01 production evidence available where needed.
- PP-02 copy constraints available.
- PP-03 deletion/data-request constraints available.
- PP-04 commerce constraints available where proof/commerce media exists.
- PP-05 public visibility constraints available.
- PP-06 notification/diagnostics constraints available.
- PP-07 abuse/moderation constraints available.
- PP-08 support/admin constraints available.
- Launch scope defined.
- No production changes.
- No storage object operations.
- No media inspection.
- No SQL/RLS/RPC/storage policy changes.
- No final legal claims.
- Sanitized evidence rules accepted.

## 46. Explicitly Blocked Actions

- No media upload.
- No storage object deletion.
- No storage object inspection.
- No bucket policy changes.
- No public/private bucket changes.
- No signed URL generation execution.
- No CDN/cache purge.
- No media moderation execution.
- No support/admin media action.
- No user data/media export.
- No production access.
- No SQL/Supabase CLI.
- No migrations.
- No source code changes.
- No RLS/RPC/storage changes.
- No policy publication.
- No legal compliance claim.
- No immediate patch authorization.

## 47. Unknown / Needs Verification Items

- Which storage buckets exist in production.
- Which buckets are public or private.
- Which media tables exist in production and how they map to objects.
- Whether signed URLs are used and who can generate them.
- Whether public URLs are used for avatars, posters, event media, venue media, videos, thumbnails, or previews.
- Whether database row deletion deletes storage objects.
- Whether hidden or suppressed media remains object-reachable.
- Whether object cleanup exists for replaced avatars, posters, covers, or thumbnails.
- Whether orphan objects are detected or cleaned up.
- Whether host hide/unhide affects only product rows or also storage objects.
- Whether support/admin takedown affects rows, objects, cache, or all three.
- Whether report/moderation evidence media exists and how it is retained.
- Whether check-in, ticket, claim, refund, or dispute proof media exists.
- Whether media references are present in notifications, deep links, diagnostics, or support logs.
- Whether OpenGraph, CDN, browser, or search caches can retain stale media.
- Whether support/admin media access is least-privilege and audited.

## 48. Acceptance Criteria for PP-09 Completion

PP-09 is complete only when:

- Media surface inventory is confirmed.
- Storage bucket/object inventory is confirmed.
- Media lifecycle taxonomy is accepted or explicitly deferred.
- Upload/ownership model is accepted or explicitly deferred.
- Public/private storage model is accepted or explicitly deferred.
- Public URL/signed URL/cache model is accepted or explicitly deferred.
- Database row versus storage object model is accepted or explicitly deferred.
- Event media/gallery/memory wall model is accepted or explicitly deferred.
- Venue/business media model is accepted or explicitly deferred.
- Poster/avatar/profile media model is accepted or explicitly deferred.
- Video/large media model is accepted or explicitly deferred.
- Media metadata/public-safe field model is accepted or explicitly deferred.
- Host hide/user delete/support takedown model is accepted or explicitly deferred.
- Moderation/appeal/restoration model is accepted or explicitly deferred.
- Account deletion/data request media model is accepted or explicitly deferred.
- Commerce/ticket/check-in proof media model is accepted or explicitly deferred.
- Notification/deep-link/preview media model is accepted or explicitly deferred.
- Support/ops/admin media access model is accepted or explicitly deferred.
- CDN/cache/OpenGraph/search-indexing model is accepted or explicitly deferred.
- Retention/redaction/deletion/export model is accepted or explicitly deferred.
- PP-01 evidence dependencies are linked.
- PP-02 copy constraints are linked.
- PP-03 deletion constraints are linked.
- PP-04 commerce constraints are linked or explicitly out of scope.
- PP-05 visibility constraints are linked.
- PP-06 notification/diagnostics constraints are linked.
- PP-07 abuse/moderation constraints are linked.
- PP-08 ops/admin/support constraints are linked.
- Product owner decisions are assigned.
- Legal/privacy/security review dependencies are assigned.
- Media owner is assigned.
- Backend/storage owner is assigned.
- Support/admin owner is assigned where support access is included.
- Trust/safety owner is assigned where moderation is included.
- Follow-up PP-10 group is updated or explicitly marked unchanged based on media/storage lifecycle decisions.
- No final legal/media/privacy/storage/deletion text is treated as approved unless the responsible owner confirms it.

## 49. Recommended Follow-Up Reports

Recommended reports after PP-09 execution, not created now:

- `MediaStorageLifecycleDecision.md`
- `StorageBucketPolicyVerificationReport.md`
- `PublicPrivateBucketDecision.md`
- `PublicUrlSignedUrlCacheDecision.md`
- `DatabaseRowVsStorageObjectDecision.md`
- `MediaDeletionRetentionMatrix.md`
- `EventMediaLifecycleDecision.md`
- `VenueMediaLifecycleDecision.md`
- `AvatarPosterReplacementDecision.md`
- `MediaModerationTakedownDecision.md`
- `AccountDeletionMediaRetentionDecision.md`
- `SupportAdminMediaAccessReview.md`
- `MediaStorageImplementationReadinessChecklist.md`

## 50. Non-Goals

- No code changes.
- No SQL/migrations.
- No production execution.
- No media upload.
- No storage object deletion.
- No storage object inspection.
- No bucket policy changes.
- No public/private bucket changes.
- No signed URL generation execution.
- No CDN/cache purge.
- No media moderation execution.
- No support/admin media action.
- No user data/media export.
- No RLS/RPC/storage changes.
- No legal advice.
- No compliance claim.
- No launch readiness claim.
- No final media/storage/privacy/deletion copy.
- No immediate patch authorization.
- No source-code re-audit.

## 51. Open Questions

- Which storage buckets exist?
- Which buckets are public?
- Which media tables exist?
- Which storage objects are public URL reachable?
- Are signed URLs used?
- Who can generate signed URLs?
- Does database row deletion delete storage objects?
- Does hidden/suppressed media remain object-reachable?
- How are avatars/posters cleaned up after replacement?
- What happens to media after account deletion?
- What happens to media after moderation/takedown/appeal/restoration?
- Are public URL/cache/CDN/OpenGraph previews purgeable?
- Who can inspect storage object metadata?
- Who can delete storage objects?
- Are orphan objects cleaned up?
- What is beta launch scope for media/storage lifecycle?

## 52. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No media/storage/bucket/public-url/signed-url/CDN/cache/deletion/moderation/support-admin/RLS/RPC/storage-policy action was executed.
- No files were staged or committed.
- Only `08_PatchPlans/PP09MediaStorageLifecyclePack.md` was created/modified.

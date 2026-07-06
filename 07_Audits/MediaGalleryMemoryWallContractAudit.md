# Media / Gallery / Memory Wall Contract Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false

## 2. Purpose

This audit maps JoinFolk media, gallery, and memory wall authority across event media upload, event gallery read, signed URL generation, public highlights, relics, venue media, posters, videos, likes, comments, moderation, hide/delete behavior, and lifecycle-specific visibility.

This is not a patch plan, cleanup plan, migration plan, or implementation authorization. It does not authorize backend/RPC/RLS/storage/auth changes. Frontend gallery rendering, UI gates, and local helper comments are treated as evidence only; media record authority and storage object authority must be backend/RPC/RLS/storage-authoritative.

## 3. Audit Scope

Read-only inspection covered handbook audit/architecture/decision documents and targeted source searches under:

- `C:\dev\joinfolk-engineering-handbook`
- `C:\dev\hostos`
- `C:\dev\joinfolk-web`
- `C:\dev\hostos\apps\mobile`

Current system context preserved from prior handbook evidence:

- Future accepted Supabase migration working target: `C:\dev\hostos\supabase\migrations`.
- This is not proof of historical sole canonical source.
- Split-source migration history remains unresolved.
- Production SQL/RPC evidence remains stronger than local source assumptions.
- Database Functions / RPC evidence is separate from Edge Function deployment evidence.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Local Edge Function source folders exist in some Supabase trees, but deployment status is not confirmed.
- No backend patch or migration is authorized by this audit.

## 4. Media / Gallery / Memory Wall Contract Summary

Observed media architecture has several distinct contracts:

- Event media records live in backend tables such as `event_media`; media bytes live in storage, primarily the local-source `event-media` bucket.
- Event media upload in mobile uploads the object first, then creates the media record through `create_event_media_v2`.
- Event gallery reads are RPC-mediated and resolve storage objects through signed URLs.
- Dashboard event gallery moderation calls `host_moderate_media_v1` for feature, unfeature, hide, unhide, and delete actions.
- Owned media management uses RPCs for hide, unhide, delete, and hidden-media list, while storage object removal is attempted after backend row deletion.
- Public highlights are intentionally separate from private attendee gallery and are described as poster, frozen winning media, and host promo assets with no uploader identity.
- Venue media uses separate `venue-media` and `venue-posters` public bucket contracts.
- Relics are server-authoritative; public relic reads intentionally exclude internal grant metadata.
- Event memory comments and photo comments are separate systems with different read/write expectations.

Clean contract expectation:

- Media record visibility and storage object visibility are separate but consistent.
- Storage upload path alone does not make media visible.
- Public highlights are derived only from media approved for public exposure.
- Venue public media and event participant media remain separate contracts.
- Notification payloads never leak private media URLs or metadata to unauthorized recipients.

## 5. Media Surface Inventory Matrix

| Surface / domain | Media exposed or action initiated | Access path observed | Expected authority owner | Visibility scope | Production evidence status | Risk class | Recommendation |
|---|---|---|---|---|---|---|---|
| Event media upload | Upload photo and register media record | Storage upload + `create_event_media_v2` | Backend/RPC/storage/auth | Checked-in/eligible user or system-event authenticated user, per source comments | Event table RLS evidence exists; `event-media` bucket production evidence missing | Privacy-sensitive | Verify storage policy and RPC guard parity |
| Event gallery read | Gallery/media list and signed image URLs | `get_event_gallery_v2` wrapper + signed URL resolver | Backend/RPC/storage/RLS | Participant/check-in/host/staff depending contract | `event_media` RLS confirmed; policy correctness incomplete | Privacy-sensitive | Document read contract |
| Memory wall | System/city memory grid, camera, likes/comments | Event media + memory comments RPCs | Backend/RPC/storage/auth | Public read for comments per local helper; write gated | Production parity incomplete | Privacy-sensitive / Product correctness | Reconcile public memory vs private gallery |
| Public event highlights | Poster and winning media | `get_event_public_highlights_v1`; signed storage URLs | Backend/RPC/storage | Public-safe | Local source/provenance; production policy incomplete | Privacy-sensitive | Verify public highlight field/storage contract |
| Host highlights | Winner-only host profile highlights | `get_host_public_highlights_v1`; signed URLs with public fallback | Backend/RPC/storage | Public-safe, no uploader identity per source | Local source/provenance; production bucket unknown | Privacy-sensitive | Verify no private media fallback exposure |
| Public relics | Public earned relic display | `get_user_relics_public_v1` | Backend/RPC/auth/RLS | Public profile surface | Local source; production policy incomplete | Privacy-sensitive | Link to profile/persona audit |
| Venue media | Venue poster/gallery context | Public `venue-posters` and `venue-media` URLs | Storage policy + venue RPCs | Public if ADR accepts | Production public bucket evidence confirmed | Privacy-sensitive | ADR/security decision |
| Event posters/videos | Poster snapshots and event videos | `posters` public URLs, `event-videos` signed helpers | Storage policy/RPC | Unknown | Production bucket evidence not supplied | Privacy-sensitive | Verify bucket policy |
| Media likes/comments | Likes, photo comments, memory comments | RPC-mediated social actions | Backend/RPC/RLS/auth | Friend/checked-in/event-context dependent | Production parity incomplete | Privacy-sensitive / Product correctness | Document social media contract |
| Dashboard moderation | Feature, hide, unhide, delete | `host_moderate_media_v1` | Backend/RPC/auth | Host/staff/ops expected | Local migration/source evidence; production body not reverified here | Operational-sensitive | Verify host authority |

## 6. Event Media Upload Contract Assessment

Observed upload path:

- Mobile `event-media.v1.ts` checks auth before upload.
- It writes the object to `event-media` under an event/user path.
- It verifies the uploaded object before media record registration when possible.
- It then calls `create_event_media_v2` with event id, storage path, overlay type, and optional geo coordinates.
- Expected domain errors include auth, participant/authorization, geo, cooldown, and one-photo limit outcomes.

Observed gating evidence:

- `eventMediaAccess.ts` states standard event gallery upload should require live state plus check-in proof, with max upload enforced by backend.
- System/city memory event upload is separately described as live plus authenticated, with geo and cap enforced by backend.
- Local `check_one_photo_rule_v1` wrapper is fail-open on client check, while comments state server-side media creation enforces the rule.

Interpretation:

- Positive pattern: storage upload and media record creation are separated, and media record creation is RPC-mediated.
- Remaining risk: storage object write may occur before RPC denial, so object cleanup/orphan policy and storage upload authorization matter.
- No production vulnerability is claimed; production bucket policy and live function body verification remain needed.

## 7. Event Media Read / Gallery Contract Assessment

Observed read path:

- `getEventGalleryV1` wrapper calls `get_event_gallery_v2`.
- It normalizes media rows and resolves `event-media` objects through signed URLs.
- Expected access denials such as not participant, not authorized, or auth required are handled as valid empty/denied outcomes.
- Dashboard event detail gallery panel calls `get_event_gallery_v1`.

Expected clean contract:

- Private attendee gallery is not public.
- Viewer authority is backend-enforced before signed URLs are generated.
- Hidden-by-host and hidden-by-user media do not appear to normal viewers.
- Host/staff visibility exceptions are explicit.

Production evidence:

- `event_media` RLS was confirmed enabled and policies exist, but policy correctness still needs deeper review.
- `event-media` storage bucket production policy state was not supplied in prior production evidence.

## 8. Memory Wall / Post-Event Media Contract Assessment

Observed memory wall behavior:

- Mobile event detail includes memory-wall UI and uses the camera/media flow for system memory events.
- The memory wall has sorting/filtering UI and stream mode.
- Source comments separate normal event gallery from system/city memory behavior.
- Event memory comments are a separate system from photo-scoped comments.

Lifecycle interpretation:

- Standard event memory wall likely depends on ended/post-event semantics, but source evidence also shows live upload gating.
- System/city memory events are special and not commerce-gated.
- Ended, archived, cancelled, and deleted behavior remains Unknown / Needs verification for media exposure.

Contract need:

- Memory wall must explicitly define which media becomes public, participant-only, checked-in-only, host-visible, or archived.
- Winning/highlight selection must not silently turn private gallery media into public media without accepted rules.

## 9. Public Highlights / Relics Contract Assessment

Observed public highlights:

- `get_event_public_highlights_v1` is described as public-safe and separate from private attendee gallery.
- It returns poster/winning media fields and intentionally no uploader identity.
- `get_host_public_highlights_v1` returns curated winner-only host highlights and resolves signed URLs for winner media.
- Source includes a fallback public URL for host highlights if signing fails.

Observed public relics:

- `get_user_relics_public_v1` returns branded public relic rows.
- Public relic reads intentionally exclude persona scope, source type, and source id.
- Public reads do not fall back to internal self-read.

Interpretation:

- These are strong local-source separation signals.
- Public highlight and public relic production policy/body parity still needs verification.
- The public fallback URL in host highlights should be reviewed against actual `event-media` bucket privacy semantics.

## 10. Venue Media vs Event Media Boundary

Venue media:

- Uses `venue-media` and `venue-posters`.
- Production evidence confirms these buckets are public with public-read policies and constrained write-policy evidence.
- Venue buyer and venue detail UI builds public URLs for posters/gallery.
- Public venue media can be acceptable if an ADR/product-security decision accepts it.

Event media:

- Uses `event-media` for participant/event photos, standalone photos, public highlights, and memory-wall visuals.
- Uses signed URLs in most observed event-media reads.
- Production bucket posture for `event-media` was not supplied.
- Event media is more privacy-sensitive because it may include participants, checked-in users, memory wall content, comments, and social interactions.

Posters/videos:

- `posters` and `event-videos` appear in local source for event poster snapshots and poster/video media.
- Their production bucket status was not covered by supplied evidence.

## 11. Storage Bucket / Signed URL Authority Assessment

| Bucket / surface | Public read vs signed/private | Upload/update/delete authority | Production evidence status | Recommendation |
|---|---|---|---|---|
| `venue-media` | Confirmed public in production evidence | Host/owner constrained write evidence supplied | Confirmed public bucket | ADR/security decision |
| `venue-posters` | Confirmed public in production evidence | Host/owner constrained write evidence supplied | Confirmed public bucket | ADR/security decision |
| `event-media` | Local source uses signed URLs; some public fallback helpers exist | Mobile upload/remove direct storage plus RPC record paths | Not covered by supplied production bucket evidence | Verify bucket policy and signed URL authority |
| `event-videos` | Local source uses signed URL helpers | Local poster-video upload/read/remove helpers | Not covered by supplied production bucket evidence | Verify bucket policy |
| `posters` | Local source uses public URL helpers | Dashboard/mobile poster upload references | Not covered by supplied production bucket evidence | Verify bucket policy and public semantics |
| `avatars` | Confirmed public in production evidence | Owner-path write policy evidence supplied | Confirmed public bucket | Profile/persona public identity decision |

Signed URL creation is not itself proof of authorization. The authorization question is whether the caller can obtain a signed URL only after backend/RLS/storage policy confirms viewer authority.

## 12. Media Moderation / Hide / Delete Contract Assessment

Observed owner controls:

- `hide_owned_media_v1`, `unhide_owned_media_v1`, `delete_owned_media_v1`, and `get_my_hidden_owned_media_v1` are called by mobile media helpers.
- Source comments state server enforces `auth.uid()` equals the media owner.
- Delete attempts storage object removal after the backend record operation returns a storage path.

Observed host controls:

- Dashboard gallery panel calls `host_moderate_media_v1` for feature, unfeature, hide, unhide, and delete.
- Local migration evidence shows `host_moderate_media_v1` and `hidden_by_host` / `hidden_by_user` behavior.

Observed comment controls:

- Photo comments are friend-gated and include author delete / owner hide semantics.
- Event memory comments include author hard-delete and host soft-hide semantics.

Interpretation:

- Positive signal: moderation actions are RPC-mediated, not purely UI flags.
- Remaining gap: production body/grant review for host moderation and owned media management is needed before final authority claims.

## 13. Lifecycle-Specific Media Availability

| Lifecycle state | Upload expectation | Read/gallery expectation | Memory/highlight expectation | Status |
|---|---|---|---|---|
| draft | No attendee upload | Host/editor media only if explicitly allowed | No public memory | Unknown / Needs verification |
| published | No standard attendee upload unless product says otherwise | Public highlights/posters may show | No private gallery public exposure | Needs verification |
| live | Standard upload allowed only for checked-in/proof users per local helper; system events allow authenticated+geo | Gallery read for checked-in/host/staff per local helper | System memory may be active | Mostly deterministic locally; production verification needed |
| ended | Upload likely closed unless product allows memory additions | Memory wall/highlights may appear | Winner/highlight freeze likely | Needs decision |
| archived | Upload closed | Memory visibility should be explicit | Public/private exposure unknown | Unknown / Needs verification |
| cancelled/canceled | Upload closed | Media suppression likely unless accepted exception | Public highlights should likely suppress | Unknown / Needs verification |
| deleted | Upload/read closed | No media exposure unless retained privately for ops | Unknown | Unknown / Needs verification |
| check-in open/closed | Check-in proof affects standard upload/read in local helper | Checked-in state is key for attendee gallery | Proof lifecycle coupling needs review | Needs verification |

## 14. Viewer Role / Persona / Staff Media Access Map

| Viewer role | Expected media access | Observed local evidence | Status |
|---|---|---|---|
| Guest | Public highlights, public venue media, public relics only | Public highlight/relic helpers exist | Needs production verification |
| Authenticated non-participant | System/city memory read/upload may be allowed; standard private gallery denied | Local helper separates system events | Needs verification |
| Participant / ticket holder | May view event detail; private gallery likely requires checked-in/proof | Local helper ranks participant below checked-in for gallery | Mostly deterministic locally |
| Checked-in | Standard gallery read/write candidate | Local helper and media comments use checked-in/proof concepts | Needs production verification |
| Host | Gallery read/moderation; venue media management | Dashboard host moderation RPC | Needs production verification |
| Staff | Gallery/tool visibility expected if product allows | Local helper ranks staff above checked-in | Needs verification |
| Ops/admin | Moderation/admin exception possible | No canonical ops media contract found in focused pass | Unknown / Needs verification |
| Owner/uploader | Hide/delete own media; read hidden own media | Owned media RPC wrappers | Needs production verification |
| Public profile visitor | Public-safe owned media/relics/highlights only | Public owned media/relic/highlight helpers | Needs field contract |

## 15. Public Web / Share Media Exposure Map

Observed public/share media surfaces:

- Public event share audit found no rich event media in the simple `/e/:id` web page.
- Public venue detail and buyer surfaces use `venue-posters` and `venue-media` public URLs.
- Public host profile/highlight surfaces use `get_host_public_highlights_v1` and signed `event-media` URLs.
- Public event highlight helper exposes poster and winning media storage path fields.
- Public relic helper exposes public-safe relic fields.

Interpretation:

- Public venue media is a separate product-public media contract.
- Public event highlights are a curated derivative of event media, not the full private gallery.
- Event media public/share exposure requires stronger storage and RPC verification than venue media because production `event-media` bucket evidence was not supplied.

## 16. Notification Payload Media Exposure Map

Prior notification audit evidence shows media notification types for media likes, comments, replies, memory comments, and photo published events.

Expected contract:

- Notification payloads should carry identifiers and safe preview text only when viewer authority permits.
- Push payloads should not include private event media URLs unless the recipient is authorized and product settings allow previews.
- Private/group/invite-only event media should be masked or omitted in push notifications unless product explicitly accepts preview exposure.
- Notification deep links must re-check media authority on open.

Status: Unknown / Needs verification for production payload field behavior.

## 17. Dashboard Media Surface Map

Observed dashboard surfaces:

- Event dashboard has a gallery tab.
- It loads media through `get_event_gallery_v1`.
- It exposes gallery lock/unlock through event module settings.
- It sends feature, unfeature, hide, unhide, and delete actions through `host_moderate_media_v1`.
- It displays storage path placeholders rather than signing media URLs server-side in the inspected dashboard panel.
- Dashboard venue/media surfaces elsewhere use public `venue-media`, `venue-posters`, and `posters` URL helpers.

Interpretation:

- Dashboard media controls are operationally sensitive and should remain RPC-mediated.
- Gallery lock is module availability, not storage privacy by itself.
- Dashboard public URL generation for venue/poster assets should remain separate from event participant media.

## 18. Mobile Media Surface Map

Observed mobile surfaces:

- Event camera/gallery modules upload event media.
- Photos feed and event gallery resolve signed URLs for `event-media`.
- Memory wall uses camera/media flow and event memory comments.
- Profile media uses owned media/public owned media RPCs.
- Likes and media comment interactions are RPC-mediated.
- Venue buyer/detail media uses public `venue-posters` and `venue-media` URLs.
- Public highlights and relics are RPC-mediated public-safe reads.

Interpretation:

- Mobile is the main consumer and creator of event media.
- It uses a mix of UI gating, RPC authority, storage direct operations, signed URLs, and public URLs.
- The highest-risk boundary is direct storage operation paired with backend media record authority.

## 19. Backend RPC / RLS / Storage Authority Evidence Map

Known production evidence from prior handbook reports:

- `event_media` RLS was confirmed enabled and policy surface exists, but policy correctness still needs deeper review.
- `venue_media` RLS was confirmed enabled and storage public read evidence exists for `venue-media`.
- Storage buckets `avatars`, `venue-media`, and `venue-posters` were confirmed public with public-read and constrained write-policy evidence.
- Production bucket evidence did not cover `event-media`, `posters`, or `event-videos` unless future reports add evidence.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Production SQL/RPC evidence remains stronger than local source assumptions.

Unreviewed buckets/functions must not be treated as safe solely because local source uses signed URLs, public URLs, or RPC wrappers.

## 20. Direct Data Access / RLS Reliance Map

| Surface | Direct access observed | RPC-mediated access observed | Authority reliance | Recommendation |
|---|---:|---:|---|---|
| Event media upload | Direct storage upload to `event-media` | `create_event_media_v2` | Storage policy + RPC guard | Verify storage/RPC consistency |
| Event media read | Signed URL generation from storage | `get_event_gallery_v2`, owned/public media RPCs | RPC read authority + storage signing | Verify signed URL authority |
| Event media delete | Direct storage remove after RPC result | `delete_owned_media_v1` | Backend row authority + storage cleanup | Document orphan handling |
| Dashboard moderation | No storage write in focused panel | `host_moderate_media_v1` | Host authority in RPC | Verify production body/grants |
| Venue media | Public storage URLs | Venue media RPCs for list/add/update/remove in prior audits | Public storage + host write policy | ADR/security decision |
| Posters/videos | Public/signed storage helpers | Poster/video attach helpers in prior audits | Bucket policy unknown | Verify production bucket state |
| Likes/comments | No direct table mutation in focused files | Like/comment RPCs | Backend social/media authority | Verify media social policy |

## 21. Duplicated / Split / Legacy Media Surfaces

| Surface / helper / RPC / table / bucket | Observed role | Current / legacy / unknown | Risk if still active or authoritative | Evidence type | Recommendation |
|---|---|---|---|---|---|
| `create_event_media_v1` vs `create_event_media_v2` | Event media record creation versions | Split/versioned | Older path may bypass newer checks if callable | Local migration/source | Verify active RPC use/grants |
| `get_event_gallery_v1` vs `get_event_gallery_v2` | Gallery read versions | Split/versioned | Field/visibility drift | Local source | Document current gallery contract |
| `event-media` bucket | Private/signed event media storage | Current likely | Production bucket policy unknown | Local source | Verify production storage policy |
| `venue-media` bucket | Public venue media | Current | Public media semantics need ADR | Production + local | Keep separate from event media |
| `posters` bucket | Event poster snapshots | Current likely | Production public/private posture unknown | Local source | Verify bucket policy |
| `event-videos` bucket | Event poster/video media | Unknown/current | Production posture unknown | Local source | Verify bucket policy |
| Event memory comments vs photo comments | Separate comment systems | Current split | Privacy/read/write semantics can diverge | Local source | Document two comment contracts |
| Public highlights vs private gallery | Curated public derivative vs attendee gallery | Current split | Private media could leak if derivation is wrong | Local source | Verify public-safe derivation |
| Public relics vs internal relics | Public profile display vs internal grant ledger | Current split | Internal source metadata exposure | Local source | Preserve separation |

## 22. Media-Critical Invariants

- Only eligible users can upload event media.
- Media upload lifecycle and viewer role rules are backend-authoritative.
- Storage upload path does not alone create visible media.
- Media records and storage objects stay consistent.
- Public highlights are derived only from media approved for public exposure.
- Private/event-only media is not exposed through public URLs.
- Signed URLs are generated only for authorized viewers.
- Host/staff/ops moderation cannot be bypassed by UI-only controls.
- Uploader hide/delete does not grant unrelated users media control.
- Cancelled/deleted events do not expose memory media unless explicitly accepted.
- Notification payloads do not leak private media URLs or metadata.
- Venue public media and event participant media remain separate contracts.

## 23. Unknown / Needs Verification Surfaces

- Production bucket and policy state for `event-media`.
- Production bucket and policy state for `posters`.
- Production bucket and policy state for `event-videos`.
- Active production definitions/grants for `create_event_media_v1` and `create_event_media_v2`.
- Active production definitions/grants for `get_event_gallery_v1` and `get_event_gallery_v2`.
- Host moderation RPC body/grants and host/staff/ops scope.
- Whether `event-media` public fallback helpers can produce usable public URLs in production.
- Exact lifecycle rules for ended, archived, cancelled, and deleted media exposure.
- Whether memory wall public/read/write behavior is canonical for standard events or only system/city memory events.
- Whether media likes/comments notifications mask private media payloads.
- Whether standalone photos and event-linked photos share one public profile contract.

## 24. Media / Gallery / Memory Wall Gaps / Risk Register Seeds

### MGM-GAP-001

- Gap ID: MGM-GAP-001
- Domain: Event media storage authority
- Current issue: Local source uses direct `event-media` storage upload, signed reads, and storage removal, but production bucket policy evidence was not supplied.
- Expected clean media/gallery/memory contract: `event-media` bucket upload/read/delete policy matches RPC media record authority.
- Risk: Privacy/storage consistency risk if storage policy and media record policy diverge.
- Priority candidate: Candidate P1 / Needs verification
- Blocked by: Production storage bucket and object policy verification.
- Recommended next action: Verify `event-media` bucket public/private status and object policies.

### MGM-GAP-002

- Gap ID: MGM-GAP-002
- Domain: Public highlights
- Current issue: Public highlight helpers expose winning media paths and signed/public URL behavior, but production derivation rules are not fully verified.
- Expected clean media/gallery/memory contract: Public highlights include only approved media, exclude uploader identity, and never expose hidden/private media.
- Risk: Privacy-sensitive exposure if private attendee media becomes public incorrectly.
- Priority candidate: Candidate P1 / Needs verification
- Blocked by: Public highlight RPC body and storage policy review.
- Recommended next action: Verify public highlight field and storage contract.

### MGM-GAP-003

- Gap ID: MGM-GAP-003
- Domain: Event media upload gating
- Current issue: Local helper states live plus check-in proof for standard event upload, while storage upload occurs before RPC record creation.
- Expected clean media/gallery/memory contract: Backend/storage authority blocks ineligible upload and record creation consistently.
- Risk: Orphan objects or unauthorized storage writes if storage policy is weaker than RPC policy.
- Priority candidate: Candidate P1
- Blocked by: Storage policy verification and upload cleanup/orphan handling review.
- Recommended next action: Document upload pipeline authority and failure cleanup behavior.

### MGM-GAP-004

- Gap ID: MGM-GAP-004
- Domain: Media lifecycle contract
- Current issue: Live upload and memory wall behavior are visible locally, but ended/archived/cancelled/deleted exposure rules are not canonical.
- Expected clean media/gallery/memory contract: Each lifecycle state defines upload, gallery read, memory wall, highlight, and suppression behavior.
- Risk: Product correctness and privacy drift after event end/cancel/archive.
- Priority candidate: Candidate P2
- Blocked by: Product lifecycle media decision.
- Recommended next action: Extend Event Lifecycle Contract with media-specific state table.

### MGM-GAP-005

- Gap ID: MGM-GAP-005
- Domain: Host moderation
- Current issue: Dashboard moderation is RPC-mediated, but production body/grant parity for `host_moderate_media_v1` is not fully reviewed here.
- Expected clean media/gallery/memory contract: Host/staff/ops moderation is scoped and cannot be bypassed by UI-only controls.
- Risk: Operational/admin-sensitive moderation drift.
- Priority candidate: Candidate P2
- Blocked by: Production RPC definition/grant verification.
- Recommended next action: Verify host moderation RPC authority.

### MGM-GAP-006

- Gap ID: MGM-GAP-006
- Domain: Event vs venue media boundary
- Current issue: Venue media is production-confirmed public through `venue-media` and `venue-posters`, while event media uses `event-media`, signed URLs, and participant/memory content with production bucket posture not supplied.
- Expected clean media/gallery/memory contract: Venue public media and event participant media are documented as separate contracts with separate bucket, visibility, upload, moderation, and public exposure rules.
- Risk: Privacy/product drift if event participant media inherits venue-public assumptions or if venue media is treated like private event media.
- Priority candidate: Candidate P2
- Blocked by: Storage/media ADR and bucket policy verification.
- Recommended next action: Document separate venue and event media contracts.
### MGM-GAP-007

- Gap ID: MGM-GAP-007
- Domain: Media social interactions
- Current issue: Media likes, photo comments, memory comments, and notification payloads are split across several RPC/helper systems.
- Expected clean media/gallery/memory contract: Social interactions inherit the same media visibility and viewer authority rules as the media record.
- Risk: Privacy/product drift through comments, likes, or notification deep links.
- Priority candidate: Candidate P2
- Blocked by: Social graph/groups/visibility audit.
- Recommended next action: Audit media social visibility and notification payload fields.

## 25. Product Decisions Required

- Is standard event gallery visible only to checked-in users, or also to accepted participants/ticket holders?
- Is standard event gallery upload limited to live state and check-in proof?
- What media remains visible after ended, archived, cancelled, or deleted states?
- Are public highlights always allowed for public events, or only after event end?
- Can public highlights include event participant photos, and under what consent/selection rule?
- Should `event-media` ever produce public URLs, or signed URLs only?
- Are standalone photos part of the same privacy model as event-linked photos?
- Which host/staff/ops roles can moderate media?
- What is the accepted ADR for venue media public exposure?

## 26. Recommended Next Audits

1. Profile / Persona / Public Identity Contract Audit: public relics, avatars, host highlights, and owned media depend on persona/profile exposure rules.
2. Social Graph / Groups / Visibility Contract Audit: media comments, likes, friend-gated photo reads, blocks, and group/private event visibility need a unified authority contract.
3. Staff / Host Operations Authority Audit: gallery moderation, host tools, staff roles, and ops/admin media powers need explicit backend authority review.

## 27. Non-Goals

- No application, dashboard, mobile, web, or Supabase code changes.
- No SQL, migration, policy, storage, RPC, or Edge Function implementation.
- No production connection or Supabase CLI usage.
- No claim that media access is unsafe solely because it exists.
- No claim that RLS is correct solely because it is enabled.
- No claim that public storage is unsafe solely because a bucket is public.
- No feature removal or cleanup recommendation.
- No release readiness conclusion.

## 28. Open Questions

- What is the production policy state for `event-media`, `posters`, and `event-videos`?
- Which event media RPC versions are currently active and callable?
- Does host moderation delete storage objects or only media records?
- Are orphaned storage objects expected after RPC-denied uploads?
- Are memory comments public read for all events or only memory/system events?
- How are public winning photos selected, frozen, and removed?
- Does hiding media remove it from public highlights, notifications, feeds, and profile surfaces?
- Does cancelled/deleted event state suppress media and highlights?
- What consent/product rule allows participant media to become public highlight media?

## 29. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `07_Audits/MediaGalleryMemoryWallContractAudit.md` was created/modified.

# Search / Discovery / Feed Visibility Contract Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false

## 2. Purpose

This audit maps JoinFolk search, discovery, and feed visibility contracts across Home feed, Discover feed, Rising/trending rails, search, public event share, venue discovery, profile discovery, media/photo feed, event detail parity, public web parity, ranking/sorting, and direct data/RLS reliance.

This is not a patch plan, cleanup plan, migration plan, or implementation authorization. It does not authorize backend/RPC/RLS/storage/auth changes. Frontend filters, ranking, sorting, category chips, and UI feed guards are product evidence only; listing visibility and detail readability must be backend/RPC/RLS-authoritative.

## 3. Audit Scope

Read-only inspection covered handbook audit/architecture/decision documents and targeted local source searches under:

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

## 4. Search / Discovery / Feed Visibility Contract Summary

Observed discovery architecture is mixed but mostly backend-oriented for core event feeds:

- Home feed calls `get_home_feed_events_v2` through `getHomeFeedV2`.
- Discover feed calls `get_discover_events_v2` or geo-aware discover RPCs through `getDiscoverFeedV2` and `getDiscoverFeedGeoV1`.
- Rising rail calls `get_rising_events_v1`.
- Discover venue feed calls `get_discoverable_venues_v1`, `get_discoverable_venues_geo_v1`, or `get_discoverable_venues_geo_v2`.
- Home mixed feed interleaves RPC event feed results with RPC-mediated participant and standalone photo feeds.
- Event detail direct-reads `events` and relies on RLS to return or hide rows.
- Public web event share directly reads `events` and relies on public RLS/policy behavior.
- Direct city/system event reads exist for controlled Discover injection.
- Profile/user search uses `search_users_v2`, not direct profile table search in the focused helpers.

Clean contract expectation:

- Feed/search listings cannot expose objects that canonical detail routes would deny.
- Ranking/trending/category filters do not authorize access.
- Public web/share visibility is at least as restrictive as the accepted public event contract.
- Social graph visibility remains separate from ticket/participant/check-in entitlement.
- Direct table reads rely on verified RLS or should later be replaced by accepted RPC authority.

## 5. Discovery Surface Inventory Matrix

| Surface / domain | Content exposed or action initiated | Access path observed | Expected authority owner | Visibility scope | Production evidence status | Risk class | Recommendation |
|---|---|---|---|---|---|---|---|
| Home feed | Event cards from own, social, followed/public contexts | `get_home_feed_events_v2` RPC + client eligibility filter | Backend/RPC/RLS/auth | Authenticated | Events RLS confirmed; feed RPC body not production-reviewed here | Privacy-sensitive / Product correctness | Document feed/detail parity |
| Mixed Home feed photos | Event-linked and standalone photos interleaved with events | Photo feed RPCs + signed URL resolver | Backend/RPC/storage/RLS/auth | Participant/self/friends per local comments | Event media RLS confirmed; photo feed RPC not fully verified | Privacy-sensitive | Link to media/social visibility contracts |
| Discover feed | Public host/pro exploration events | `get_discover_events_v2` / geo RPC + client public/persona filter | Backend/RPC/RLS/auth | Authenticated/public-like discovery | Events RLS confirmed; discover RPC not fully verified | Privacy-sensitive / Product correctness | Verify Discover contract |
| Rising rail | Momentum/trending public events | `get_rising_events_v1` + client eligibility filter | Backend/RPC/RLS/auth | Public discovery | Production body not verified here | Product correctness | Ensure ranking follows visibility |
| City/system events | Controlled city memory/system event injection | Direct `events` read filtered by system flag/status/title | Backend/RLS/RPC | Public discovery if intended | Events RLS confirmed; policy correctness incomplete | Privacy-sensitive | Verify city/system read policy |
| Event search | Explicit event search not clearly established in focused local source | Unknown | Backend/RPC/RLS/auth | Unknown | Unknown | Unknown | Needs verification |
| User/profile search | People search for friends, wallet, host member, claims | `search_users_v2` RPC | Backend/RPC/RLS/auth | Authenticated | Profile/social policy evidence incomplete | Privacy-sensitive | Document public profile search fields |
| Venue discovery | Discoverable venue cards | Venue discover RPCs; public venue storage URLs | Backend/RPC/storage/RLS | Public/authenticated discovery | Venue RLS and public venue media evidence partial | Privacy-sensitive / Product correctness | Document public venue field contract |
| Public event share | Compact event landing page | Direct `events` read in web share page | Backend/RLS or public RPC | Anonymous | Events RLS confirmed; public policy correctness incomplete | Privacy-sensitive | Reconcile with public event contract |
| Event detail | Full event read and module access | Direct `events` read, RLS, viewer access helpers | Backend/RLS/RPC/auth | Viewer-role dependent | Events RLS confirmed; policy correctness incomplete | Privacy-sensitive / Revenue-sensitive | Verify listing-detail parity |
| Dashboard event lists | Host/ops event management views | Direct/RPC mixed per prior audits | Backend/RPC/RLS/auth | Host/ops/admin | Partial | Operational/admin-sensitive | Keep separate from public discovery |
| Ranking/sorting | Live first, starts_at, recency, affinity, geo/category filters | Frontend sort/filter plus backend ranking RPCs | Backend for visibility, UI for presentation | Same as source feed | Visibility evidence depends on source feed | UX/Product correctness | Ranking must not override visibility |

## 6. Home Feed Contract Assessment

Observed Home feed behavior:

- `getHomeFeedV2` calls `get_home_feed_events_v2`.
- Client-side defense-in-depth keeps only `published` and `live` statuses and excludes `city_memory` by default.
- Local comments describe Home as own events plus accepted friends plus followed public events.
- Home card identity is hydrated from profile identity helpers.
- Home mixed feed adds event-linked participant photos and standalone self/friend photos via separate RPCs.

Contract interpretation:

- Listing eligibility is mostly RPC-mediated, with frontend filtering as a mirror/defense layer.
- Home feed needs parity with event detail: any event listed should be detail-readable by the same viewer.
- Photo interleaving must not broaden event/media visibility beyond the source RPCs.

Unknowns:

- Production body of `get_home_feed_events_v2` was not reverified in this audit.
- Social graph table policy correctness for friend/follow/group inputs remains Unknown / Needs verification.

## 7. Discover / Rising / Trending Contract Assessment

Observed Discover behavior:

- `getDiscoverFeedV2` calls `get_discover_events_v2`.
- `getDiscoverFeedGeoV1` calls `get_discover_events_geo_v1` and falls back to non-geo Discover on local source error.
- Discover client filters keep feed-eligible rows, `visibility = public`, and exclude `created_under_persona = personal`.
- `getRisingFeedV1` calls `get_rising_events_v1`, then applies feed eligibility and personal-event exclusion.
- Discover screen applies category, live-only, geo, affinity, and venue type filters client-side.

Contract interpretation:

- Discover/Rising should show only public-approved events.
- Ranking, geo, category, and affinity can order or filter results, but cannot authorize private/group/friends-only content.
- Personal events are intentionally excluded in the observed Discover helpers.

Unknowns:

- Whether backend Discover/Rising RPCs always exclude non-public/private rows before client filtering.
- Whether public web, Discover, and event detail agree on published/live/ended visibility.

## 8. Search Contract Assessment

Observed search surfaces:

- User/profile search uses `search_users_v2`.
- Claim/friend/host-member/wallet picker search uses social search wrappers.
- Event search was not clearly established as a dedicated local helper in the focused scan.
- Venue discovery uses RPC-mediated discover lists rather than a direct text search in the inspected helpers.

Expected clean contract:

- Search results do not expose objects unavailable through canonical detail/read routes.
- User/profile search exposes only accepted public/searchable profile fields.
- Search can rank or scope results but cannot bypass profile, block, group, event, media, ticket, or venue visibility.

Status: Needs verification for event search and global public search surfaces.

## 9. Event Detail Parity Assessment

Observed event detail path:

- Mobile event detail direct-reads `events` with `maybeSingle`.
- Missing rows after RLS are treated as unavailable/not found.
- Deep-link shim uses `can_view_event_for_user_v1` for authenticated non-host users.
- Anonymous fallback in the shim locally checks `visibility = public` and active lifecycle states.
- Draft guard blocks non-host users in the detail screen.

Parity risks:

- Feed RPCs, detail direct reads, public web direct reads, and `can_view_event_for_user_v1` may diverge unless they share the same backend visibility contract.
- Client-side status/visibility filters are not sufficient authority.

Expected clean contract: every event card should either open successfully for the same viewer or have a documented reason why listing and detail differ.

## 10. Public Web / Share Parity Assessment

Observed public web event share:

- Web `/e/:id` reads `events` directly and selects compact public fields including title, time, location, visibility, and status.
- Public web event share does not authenticate the viewer.
- Prior public web audit identified this as RLS-dependent.

Expected parity:

- Public web/share should not reveal private, group, friends-only, invite-only, draft, deleted, or otherwise hidden events unless an explicit backend-authorized share contract exists.
- Public event share should match the accepted public event field contract, not the richer mobile event detail contract.
- Public share must remain separate from share-group membership and private social visibility.

## 11. Lifecycle Visibility Matrix

| Lifecycle state | Home | Discover | Search | Public web/share | Event detail | Notes |
|---|---|---|---|---|---|---|
| `draft` | Host-owned visibility possible per docs, general feed Unknown | Not expected | Unknown | Not expected | Host-only expected | Needs backend parity check |
| `published` | Eligible in Home via RPC/client filter | Eligible if public/non-personal | Unknown | Public if RLS allows | Readable if viewer authorized | Core feed state |
| `live` | Eligible in Home via RPC/client filter | Eligible if public/non-personal | Unknown | Public if RLS allows | Readable if viewer authorized | Core feed state |
| `ended` | Local feed helper says ended tail deferred | Not in client feed eligibility | Unknown | Public behavior Unknown | Detail may be visible depending RLS/lifecycle | Needs product decision |
| `archived` | Not in client feed eligibility | Not expected | Unknown | Unknown | Some profile/archive surfaces may show | Needs parity decision |
| `cancelled` / `canceled` | Not in client feed eligibility | Not expected | Unknown | Unknown | Should be hidden or explicit cancellation view | Needs lifecycle decision |
| `deleted` | Not expected | Not expected | Not expected | Not expected | Not expected | Depends on RLS/soft delete policy |

## 12. Event Visibility Mode Matrix

| Visibility mode | Home | Discover | Search | Public web/share | Event detail | Notes |
|---|---|---|---|---|---|---|
| `public` | Eligible depending social/feed RPC | Expected Discover mode | Unknown | Expected if lifecycle allowed | Readable by allowed public/auth viewers | Public field contract needed |
| `private` | UI value normalized to `invite_only` locally | Not expected | Unknown | Not expected | Authorized viewers only | Vocabulary split |
| `friends` | Expected through social/group inputs | Not expected | Unknown | Not expected | Friend/group-authorized viewers only | Needs friend/group contract |
| `followers` | Docs imply followed public host events in Home | Discover public only | Unknown | Public only if public | Follower-specific access unclear | Needs product decision |
| `group` | Not clearly canonical; share groups exist | Not expected | Unknown | Not expected | Group member only if backend allows | Needs verification |
| `invite_only` | Possible through private/group targeting | Not expected | Unknown | Not expected | Invite/authorized only | Needs public route parity |
| `members` | Local UI value observed; status unclear | Unknown | Unknown | Unknown | Unknown | Needs product decision |
| Participant/ticket-holder | Not a feed visibility mode | Not expected | Unknown | Not expected | Detail/module access may use entitlement | Separate from social visibility |
| Host/staff/ops | Management visibility only | Not public discover | Unknown | Not public | Host/staff/ops detail/admin surfaces | Separate from public feeds |

## 13. Social Graph / Group Visibility Interaction

Social graph findings from the prior audit:

- Friend actions are mostly RPC-mediated.
- Follow actions are RPC-mediated.
- Share group management uses direct `share_groups` and `share_group_members` access.
- Profile visibility uses RPCs.
- Event publish links visibility and group targets through `publish_event_with_groups_and_snapshot_v2`.

Feed implications:

- Home feed may include social/friend/group/followed events.
- Discover should not include friends/group/private-only events.
- Social visibility does not confer commerce entitlement.
- Ticket or participant entitlement does not make a private social event broadly discoverable.

## 14. Entitlement / Participation Interaction

Entitlement concepts relevant to discovery:

- Participant
- Ticket holder
- Checked-in
- Host
- Staff
- Ops/admin

Contract expectation:

- Event detail, gallery, check-in, wallet, ticket, and media access may depend on entitlement.
- Discovery/search listing should not use entitlement to make private social data broadly searchable.
- A ticket holder may be able to open a detail view without causing that event to appear in public Discover.
- Checked-in status may unlock gallery/media but should not change public event search visibility.

## 15. Venue Discovery / Public Venue Contract Assessment

Observed venue discovery:

- `getDiscoverableVenuesV1`, `getDiscoverableVenuesGeoV1`, and `getDiscoverableVenuesGeoV2` are RPC-mediated.
- Venue discover payload intentionally excludes `host_id` in local types.
- Venue discover returns active, poster-ready, reservation-enabled venues per local comments.
- Discover screen applies venue type filters client-side.
- Public venue read uses `get_public_venue_v1` and `get_public_venue_detail_v2` in other helpers.

Expected clean contract:

- Venue discovery fields are public-approved.
- Venue media/poster public storage behavior follows the public venue/media ADR.
- Venue discovery does not expose private owner identity unless accepted.

## 16. Profile / Host Discovery Contract Assessment

Observed profile discovery:

- People/profile search uses `search_users_v2`.
- Search rows include personal/organizer identity, friend/request state, follow state, tier, and match context.
- Following/friends/host member/claim recipient flows reuse search.

Expected clean contract:

- Profile discovery exposes only accepted searchable fields.
- Blocks, mutes, profile visibility, and private identity fields are respected by backend search.
- Tier or host persona display does not confer capability.

Production profile/social policy evidence remains incomplete.

## 17. Media / Photo Feed Discovery Contract Assessment

Observed media/photo discovery:

- Home mixed feed uses `get_participant_photos_feed_v1` for event-linked photos.
- Home mixed feed uses `get_standalone_photos_v1` for standalone self/friend photos.
- Signed URLs are resolved after RPC results.
- Owner identity is post-hydrated from profile identity helpers.
- Media social summaries use `get_media_social_summary_v1`.

Expected clean contract:

- Photo feed visibility inherits media/event/social visibility.
- Signed URLs are generated only after an authorized media listing result.
- Standalone photo discovery is limited to self/friends unless product explicitly broadens it.

## 18. Ranking / Sorting / Trending Authority Boundary

Observed ranking/sorting:

- Home sorts live first, then starts_at, then updated_at.
- Discover applies category, live-only, geo, affinity, and venue-type filters.
- Rising uses a backend RPC and client feed eligibility filter.
- Venue discovery can use geo-aware RPCs and client-side type chips.

Authority boundary:

- Ranking can affect order.
- Filtering can improve UX.
- Neither ranking nor filtering grants access.
- Backend/RPC/RLS must ensure hidden/private/group-only rows are not included before ranking can expose them.

## 19. Dashboard / Ops Discovery Surface Map

Observed dashboard relevance:

- Dashboard event lists and operational views use status, visibility, host ownership, and direct/RPC mixed access in prior audits.
- Dashboard discovery-like views are management surfaces, not public discovery.
- Ops/admin listings may legitimately see broader data, but that authority must not leak into public/mobile feeds.

Status: Operational/admin discovery authority remains Unknown / Needs verification.

## 20. Mobile Discovery Surface Map

Observed mobile surfaces:

- Home feed screen.
- Discover screen with event/venue modes.
- Rising rail cards.
- Public/deep-link event shim.
- Event detail direct read.
- Photo feed/mixed feed.
- Profile/user search in friends, following, host members, wallet, and claim recipient flows.
- Venue pages and public venue helpers.

Interpretation: mobile is the primary discovery surface and uses a combination of feed RPCs, public venue RPCs, profile search RPCs, direct event reads, and frontend presentation filters.

## 21. Public Web Discovery Surface Map

Observed public web surface:

- `/e/:id` public event share page directly reads `events`.
- Claim and verification pages are handoff/placeholder-style surfaces in prior audits.
- Public web is not the same as authenticated mobile Discover.

Expected clean contract:

- Public web event fields are minimal and public-approved.
- Public web does not surface private/group/invite-only events.
- Public web share behavior should be verified against production RLS/policies.

## 22. Backend RPC / RLS Authority Evidence Map

Prior production evidence only:

- Events RLS was confirmed enabled and policy surface exists, but policy correctness still needs deeper review.
- Production evidence for `share_groups`, `share_group_members`, friendships, follows, and blocks/mutes was not fully covered.
- `profiles` / `user_profiles` production RLS/policy evidence was not fully covered.
- Event/media/venue/profile/public identity/search behavior remains dependent on their own table/RPC/storage authority.
- Storage buckets `avatars`, `venue-media`, and `venue-posters` had public-read evidence; other event media/poster buckets were not fully covered.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Production SQL/RPC evidence remains stronger than local source assumptions.

Local-source-only authority signals:

- Home, Discover, Rising, venue discovery, photo feed, profile search, and profile visibility are mostly RPC-mediated.
- Event detail and public web share still direct-read `events`.
- Direct city/system event discovery relies on event RLS.

## 23. Direct Data Access / RLS Reliance Map

| Data surface | Direct access observed | RPC-mediated access observed | Production evidence status | Risk class | Recommendation |
|---|---|---|---|---|---|
| `events` | Event detail, public web share, deep-link shim, city/system event fetch | Home/Discover/Rising feeds and publish visibility | RLS confirmed; correctness incomplete | Privacy-sensitive | Verify feed/detail/public parity |
| `user_profiles` | Host identity hydration and Discover follow/profile state | `search_users_v2`, profile visibility RPCs | Not fully covered | Privacy-sensitive | Verify profile search/public field contract |
| `user_follows` | Discover local follow state read | Follow RPCs | Not covered | Privacy-sensitive | Verify follow RLS or use RPC state |
| `share_groups` / `event_share_groups` / `share_group_members` | Publish diagnostics and group management in prior audit | Publish RPC | Not covered | Privacy-sensitive | Verify group visibility policy |
| Venue tables | Focused discover helpers are RPC-mediated | Venue discover/public venue RPCs | Venues RLS confirmed; field contract incomplete | Privacy-sensitive | Preserve RPC pattern |
| `event_media` / photo feeds | Focused feed helpers use RPCs and signed URLs | Photo feed/social summary RPCs | Event media RLS confirmed; policy correctness incomplete | Privacy-sensitive | Verify media feed RPC contract |
| Public storage buckets | Venue poster/media card URLs | Storage public reads and media RPCs | Partial | Privacy-sensitive | ADR/storage verification |

## 24. Duplicated / Split / Legacy Feed Surfaces

| Surface / helper / RPC / table | Observed role | Current / legacy / unknown | Risk if still active or authoritative | Evidence type | Recommendation |
|---|---|---|---|---|---|
| `get_home_feed_events_v2` vs direct event detail read | Listing vs full detail | Current | Listed event may not open, or hidden event may open via a different rule | Local source + prior reports | Reconcile feed/detail parity |
| `get_discover_events_v2` vs Discover client filters | Backend public feed plus UI public/persona filter | Current | Client filter could hide but not safely authorize rows | Local source | Verify backend Discover filter |
| Public web `/e/:id` direct read | Anonymous public share | Current | Public route parity may drift from mobile detail/feed | Local source | Verify RLS/public event contract |
| `private` vs `invite_only` | UI visibility value mapped to DB value | Current mapping | Privacy vocabulary drift | Local source | Document canonical vocabulary |
| `members` visibility | Local UI publish value | Unknown | Undefined listing/detail behavior | Local source | Product decision |
| `friends_of_friends`, `active`, `closed` aliases | Type/docs aliases | Legacy/unknown | Feed/lifecycle drift | Local docs/source | Reconcile vocabulary |
| City/system event direct fetch | Controlled Discover injection | Current / special-case | Direct RLS reliance outside feed RPCs | Local source | Verify system event contract |
| Feed event photos vs event gallery | Feed-safe photo surfaces | Current | Media visibility could diverge from gallery/detail | Local source | Link to media contract |

## 25. Search-Discovery-Critical Invariants

- Feed/search listings cannot expose objects that event detail would deny.
- Public web/share listings cannot expose private/group/invite-only events.
- Ranking/trending does not override visibility.
- Lifecycle filters are consistent across Home, Discover, Search, public web/share, and event detail unless product explicitly documents differences.
- Social visibility does not confer commerce/ticket/check-in entitlement.
- Ticket/participant entitlement does not make private social events broadly discoverable.
- Venue/profile/media search follows accepted public field contracts.
- Frontend filters are not security controls.
- Direct table reads rely on verified RLS or must move behind RPC later.
- Ops/admin discovery surfaces do not become public feed/search surfaces.

## 26. Unknown / Needs Verification Surfaces

- Production bodies and grants for `get_home_feed_events_v2`, `get_discover_events_v2`, `get_discover_events_geo_v1`, and `get_rising_events_v1`.
- Whether `can_view_event_for_user_v1` is canonical for all authenticated event detail reads.
- Whether public web event share has identical public visibility constraints to mobile public detail.
- Whether ended/archived public events should appear in any feed/search surface.
- Whether event search exists as a dedicated product surface.
- Whether `members` visibility is active, future-facing, or legacy.
- Whether social/group/private visibility affects Home, detail, profile, media, and notifications consistently.
- Whether city/system events should bypass default feed type exclusions.

## 27. Search / Discovery / Feed Visibility Gaps / Risk Register Seeds

| Gap ID | Domain | Current issue | Expected clean search/discovery/feed visibility contract | Risk | Priority candidate | Blocked by | Recommended next action |
|---|---|---|---|---|---|---|---|
| SDF-GAP-001 | Feed/detail parity | Feed RPCs and event detail direct reads may rely on different authority paths | Listed events are readable by the same viewer or intentional differences are documented | Privacy-sensitive / Product correctness | Candidate P1 | Production feed RPC and event RLS review | Verify feed/detail parity |
| SDF-GAP-002 | Public web parity | Web `/e/:id` direct read depends on public RLS and may differ from mobile gates | Public share exposes only approved public event fields/states | Privacy-sensitive | Candidate P1 | Public RLS/policy review | Verify public share event visibility |
| SDF-GAP-003 | Discover backend filter | Discover client filters public/persona after RPC result | Backend Discover returns only visibility-approved public discovery rows | Privacy-sensitive | Candidate P1 | Production RPC body review | Verify Discover RPC output contract |
| SDF-GAP-004 | Lifecycle feed rules | Ended/archived/cancelled feed behavior is partly deferred/unclear | Lifecycle visibility matrix accepted across Home/Discover/Search/detail | Product correctness | Candidate P2 | Product decision | Decide ended/archive discovery behavior |
| SDF-GAP-005 | Visibility vocabulary | `private`, `invite_only`, `friends`, `members`, aliases remain split | One canonical visibility vocabulary | Privacy-sensitive / Product correctness | Candidate P1 | Product/security decision | Reconcile visibility names |
| SDF-GAP-006 | Profile/user search | `search_users_v2` exposes profile/social fields; production profile policy incomplete | Searchable profile field contract documented and verified | Privacy-sensitive | Candidate P2 | Profile RLS/RPC review | Define profile search fields |
| SDF-GAP-007 | Media/photo feed | Photo feed visibility depends on RPCs, signed URLs, and social/event rules | Photo feed inherits media/event/social visibility | Privacy-sensitive | Candidate P2 | Media feed RPC review | Verify photo feed authority |
| SDF-GAP-008 | Venue discovery | Venue discover is RPC-mediated but public venue field contract incomplete | Public venue discovery fields are accepted and storage policy is documented | Privacy-sensitive / Product correctness | Candidate P2 | Venue ADR/field contract | Document venue discovery contract |

## 28. Product Decisions Required

- Should ended events appear in Home, Discover, Search, public share, or only profile/archive surfaces?
- Should Home, Discover, Search, event detail, and public web share use one canonical visibility resolver?
- Which visibility modes are valid and active: `public`, `private`, `friends`, `followers`, `group`, `invite_only`, `members`, participant/ticket-holder, host/staff/ops?
- Should public web share use the same backend event visibility function as mobile deep-link auth gates?
- What profile/user fields are searchable globally?
- Are city/system memory events public discovery surfaces or separate system surfaces?
- Can follower relationships affect Home feed visibility, or only host profile/stats/notifications?
- What venue fields and media are public in venue discovery?
- Should listing and detail parity be strict, or can a listed card intentionally fail to open with a documented reason?
## 29. Recommended Next Audits

- Staff / Host Operations Authority Audit.
- Messaging / Direct Conversation Contract Audit.
- Ops / Admin / Support Tools Authority Audit.

## 30. Non-Goals

- No search, feed, discovery, event, venue, profile, or media implementation changes.
- No Supabase migration drafting.
- No SQL creation.
- No production verification query bundle.
- No cleanup, deletion, move, or source-tree normalization.
- No acceptance of vulnerabilities, priorities, or remediation plans.
- No final claim that any feed/search visibility exposure is exploitable.

## 31. Open Questions

- Which backend function is canonical for event visibility across Home, Discover, detail, and public share?
- Should event detail direct reads continue to rely on RLS, or should all detail reads be RPC-mediated?
- Should public web `/e/:id` support only public/published/live events?
- How should feeds handle archived/ended public events?
- Are social/private/group feeds expected to include events that Discover excludes?
- Is event search planned, active, or absent?
- Should venue discovery expose only reservation-ready venues?

## 32. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `07_Audits/SearchDiscoveryFeedVisibilityContractAudit.md` was created/modified.

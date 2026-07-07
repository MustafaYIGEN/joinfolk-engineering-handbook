# Social Graph / Groups / Visibility Contract Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false

## 2. Purpose

This audit maps JoinFolk social graph, groups, and visibility contracts across friendships, follows, host followers, share groups, group membership, private/friends/group visibility, public visibility, profile visibility, event feed/detail access, media/comment visibility, notification eligibility, blocks, and public share boundaries.

This is not a patch plan, cleanup plan, migration plan, or implementation authorization. It does not authorize backend/RPC/RLS/storage/auth changes. Frontend filters, route guards, and UI labels are evidence only; privacy, visibility, membership, and relationship authority must be backend/RPC/RLS-authoritative.

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

## 4. Social Graph / Groups / Visibility Contract Summary

Observed social visibility architecture is mixed:

- Friendship actions are mostly RPC-mediated through wrappers such as `search_users_v2`, `send_friend_request_v1`, `respond_friend_request_v1`, `cancel_friend_request_v1`, `get_my_friend_requests_v1`, `get_my_friends_v1`, and `remove_friend_v1`.
- Follow and host follower actions are RPC-mediated through `follow_user_v1`, `unfollow_user_v1`, `get_host_profile_stats_v1`, `get_my_following_v1`, and host follower/block RPCs.
- Block actions are RPC-mediated through `block_user_v1`, `unblock_user_v1`, `get_my_blocked_users_v1`, and `get_my_host_followers_v1`.
- Share group management in mobile uses direct `share_groups` and `share_group_members` reads/writes/deletes.
- Event publish uses `publish_event_with_groups_and_snapshot_v2`; local source normalizes `private` to `invite_only`.
- Profile visibility uses RPCs `get_profile_visibility_v1` and `set_profile_visibility_v1`.
- Event deep-link access delegates authenticated non-host visibility to `can_view_event_for_user_v1`, while anonymous fallback checks public visibility and active lifecycle locally.
- Event detail still direct-reads `events` and relies on RLS to return or hide rows.

Clean contract expectation:

- Friend/follow/group relationships are distinct product concepts with separate visibility effects.
- Social graph visibility does not confer ticket, commerce, check-in, or participant entitlement.
- Ticket/participant entitlement does not automatically confer friendship, follow, or group visibility.
- Feed visibility and event detail visibility are consistent or explicitly documented where they differ.
- Public share links and group membership are separate authority concepts.

## 5. Social Visibility Surface Inventory Matrix

| Surface / domain | Visibility or relationship action | Access path observed | Expected authority owner | Visibility scope | Production evidence status | Risk class | Recommendation |
|---|---|---|---|---|---|---|---|
| Friend search | Search users with friend/request/follow state | `search_users_v2` RPC | Backend/RPC/RLS/auth | Authenticated | Production social table evidence incomplete | Privacy-sensitive | Preserve RPC and verify field contract |
| Friend request lifecycle | Send, respond, cancel, list requests | Friend RPC wrappers | Backend/RPC/RLS/auth | Requester/addressee | Production social table evidence incomplete | Privacy-sensitive | Document friendship contract |
| Friend list | Read my friends | `get_my_friends_v1` RPC | Backend/RPC/RLS/auth | Owner | Production evidence incomplete | Privacy-sensitive | Preserve RPC authority |
| Follow/host follow | Follow/unfollow, host profile stats, following list | Follow RPC wrappers | Backend/RPC/RLS/auth | Authenticated/follower/host | Production evidence incomplete | Privacy-sensitive / Product correctness | Separate follow from friend access |
| Host followers | Host follower list and block controls | RPC wrappers | Backend/RPC/RLS/auth | Host owner | Production evidence incomplete | Privacy-sensitive | Verify host-follower visibility |
| Blocks | Block/unblock and blocked user list | RPC wrappers | Backend/RPC/RLS/auth | Owner/host owner | Production evidence incomplete | Privacy-sensitive / Security-sensitive | Document block effects |
| Share groups | Owner-managed audience lists | Direct `share_groups` reads/inserts/updates/deletes | Backend/RLS/auth or accepted RPC | Owner and selected members | Production evidence incomplete | Privacy-sensitive | Verify RLS or prefer RPC authority later |
| Share group members | Add/remove group members with accepted/pending status | Direct `share_group_members` reads/inserts/deletes | Backend/RLS/auth or accepted RPC | Owner/member | Production evidence incomplete | Privacy-sensitive | Document membership status semantics |
| Event publish visibility | Public/friends/private/member visibility and group targets | `publish_event_with_groups_and_snapshot_v2` RPC plus direct audit reads | Backend/RPC/RLS/auth | Host and selected audience | Events RLS confirmed; group policies unknown | Privacy-sensitive / Product correctness | Preserve RPC publish authority |
| Event deep-link gate | Public/guest/auth event access | Direct `events` read + `can_view_event_for_user_v1` for auth non-host | Backend/RPC/RLS/auth | Anonymous/authenticated/host | Events RLS confirmed; policy correctness incomplete | Privacy-sensitive | Reconcile local anonymous fallback with backend authority |
| Event detail | Direct event read and access context rendering | Direct `events` read, RLS, viewer access helpers | Backend/RLS/RPC/auth | Viewer role dependent | Events RLS confirmed; policy correctness incomplete | Privacy-sensitive | Document event read contract |
| Profile visibility | Public/friends-only profile setting | `get_profile_visibility_v1`, `set_profile_visibility_v1` | Backend/RPC/RLS/auth | Owner/friend/public | Profile RLS evidence incomplete | Privacy-sensitive | Link to profile identity contract |
| Media/comment visibility | Gallery/comment/like reads and hidden/private media | Mixed RPC and identity/social helpers | Backend/RPC/RLS/storage/auth | Event/social viewer dependent | Event media RLS confirmed; policy correctness incomplete | Privacy-sensitive | Inherit event/media/social visibility |
| Notification eligibility | Social/private context, mutes, visibility | Local Edge Function source references `evaluate_push_eligibility_v1` | Backend/RPC and deployed delivery path | Recipient only | No deployed Edge Functions visible | Privacy-sensitive | Verify DB function and deployment separately |
| Public share boundary | Public event, claim, venue, profile/share surfaces | Mixed direct reads, RPCs, public storage | Backend/RPC/RLS/storage | Anonymous/public/auth handoff | Partial production evidence | Privacy-sensitive / Revenue-sensitive | Keep public contract separate from groups |

## 6. Friendship Contract Assessment

Observed friendship system:

- `friends.v1.ts` uses RPC wrappers for user search, send request, respond, cancel, request list, friend list, and remove friend.
- Visitor profile logic directly reads `friendships` and `friend_requests` to derive local relationship state.
- Group screens use `getMyFriendsV1` to select members for personal audience lists.

Contract interpretation:

- Friendship is distinct from event participation and ticket ownership.
- Friendship may affect profile visibility and personal audience selection, but it should not create commerce entitlement.
- Direct friendship/request reads in visitor profile rely on production RLS that was not covered in supplied production reports.

Recommendation: preserve the RPC-centered friendship mutation path and verify direct relationship-read RLS before treating friend visibility as deterministic.

## 7. Follow / Host Follower Contract Assessment

Observed follow and host follower surfaces:

- `follow_user_v1` and `unfollow_user_v1` handle follow mutations.
- `get_host_profile_stats_v1` returns follower count, rating summary, and whether current user follows.
- `get_my_following_v1` returns organizer-facing following rows.
- Host follower management uses `get_my_host_followers_v1` and block-related RPCs.

Contract interpretation:

- Follows are not the same as friendships.
- Host follower counts may be public product signals, but access to follower lists should be host/owner-scoped unless product says otherwise.
- Follower-based event access, if any, must be backend-authoritative and distinct from public/friends/group visibility.

## 8. Share Groups / Group Membership Contract Assessment

Observed share group behavior:

- Mobile group screen directly reads, creates, renames, deletes, and counts `share_groups`.
- Mobile group screen directly reads, inserts, and deletes `share_group_members`.
- Personal audience list kinds such as close friends, family, work, custom, and coworkers are treated as owner-managed visibility lists.
- Local comments distinguish personal audience lists from membership/invite communities.
- Event publish links selected groups through the publish RPC and later audits group membership state with direct reads.

Contract interpretation:

- Share groups are visibility/audience lists, not public share links.
- Group membership and member status must be backend/RLS-authoritative.
- Direct group membership mutation is privacy-sensitive and needs production RLS verification.

Expected clean contract:

- Group owner controls membership for private audience lists.
- Group membership visibility does not bypass event lifecycle, public/private status, ticket entitlement, or media visibility.
- Pending versus accepted membership semantics are canonical and not inferred only from UI.

## 9. Invite-Only / Private / Friends Visibility Assessment

Observed visibility vocabulary includes:

- `public`
- `friends`
- `invite_only`
- local UI `private`, normalized to `invite_only`
- local UI `members`, status unclear
- profile `friends_only`
- event participant/ticket-holder/host/staff/ops access concepts

Current uncertainty:

- Some local docs mention `friends_of_friends`, `members`, or group/member variants, but production parity for those modes was not established.
- Frontend labels and helper names are not proof of backend enforcement.
- Private/invite-only event behavior in public web/share surfaces remains a high-signal verification target.

Expected clean contract:

- Public events can be seen by anonymous/public viewers only in accepted lifecycle states.
- Friends/group/invite-only events are visible only to backend-authorized users.
- Host/staff/ops visibility remains separate from social visibility.

## 10. Profile Visibility Contract Assessment

Observed profile visibility behavior:

- Settings screen reads `get_profile_visibility_v1` and writes `set_profile_visibility_v1`.
- Visitor profile reads `get_profile_visibility_v1` and uses it to gate personal hosted events and profile content.
- Profile visibility values observed: `public` and `friends_only`.

Contract interpretation:

- Public avatar storage does not imply public profile visibility.
- Profile visibility is separate from event visibility.
- Friends-only profile behavior should be backend/RPC/RLS-authoritative across profile header, tabs, media, relics, events, and notifications.

Production evidence for `profiles` / `user_profiles` policy correctness was not fully supplied.

## 11. Event Visibility / Feed / Discovery Contract Assessment

Observed event visibility behavior:

- Event detail direct-reads `events` and uses `maybeSingle` so RLS-hidden rows appear unavailable.
- Deep-link shim checks direct event status/visibility, host ownership, `can_view_event_for_user_v1`, and anonymous public fallback.
- Publish path uses `publish_event_with_groups_and_snapshot_v2` and sends visibility and group IDs.
- Local source comments and docs refer to home feed RPC behavior for group/friend visibility.
- Event lifecycle states interact with visibility: draft, published, live, ended, archived, cancelled/canceled, and deleted.

Expected clean contract:

- Feed/discovery eligibility and event detail readability should be driven by the same backend visibility rules or documented differences.
- Draft events are host-only.
- Archived/ended visibility rules are explicit.
- Private/group/invite-only events are not publicly readable through direct routes or public web pages.

## 12. Media / Comment Visibility Assessment

Observed media/comment visibility inputs:

- Media audit found event gallery, memory wall, public highlights, likes, and comments use mixed RPC/storage paths.
- Visitor profile uses public relics, public highlights, signed media URLs, friend status, profile visibility, and media owner identity.
- Local comments indicate some personal profile photo/event visibility can be friend-gated or public-profile-gated.
- Event detail module access engine gates social modules for guests.

Expected clean contract:

- Media/comment visibility inherits event visibility, media visibility, lifecycle state, and social/profile visibility.
- Public highlights/relics are separate from private attendee gallery.
- Friend/follow/group relationship does not override checked-in/participant-only gallery rules unless explicitly designed.

## 13. Notification / Push Eligibility Visibility Assessment

Observed notification visibility inputs:

- Notification audit found notification V2 payloads include actor identity, target persona, event, host, conversation, media, and deep-link fields.
- Local Edge Function source for `push-dispatch` references `evaluate_push_eligibility_v1`, settings, mutes, throttle, and visibility.
- Manual dashboard confirmation found no deployed Supabase Edge Functions visible.

Contract interpretation:

- Notification eligibility is separate from notification delivery.
- Notification and push payloads must respect social visibility, blocks/mutes, event visibility, profile visibility, and private context settings.
- Local Edge Function source is not production deployment proof.

## 14. Public Web / Share Boundary Assessment

Prior public web/share audit found public event, claim, verification, venue, media, avatar, relic, and highlight surfaces.

Expected boundary:

- Public share links do not equal group membership.
- Group/private/invite-only events are not public simply because a route exists.
- Claim or invite handoff may preview public-safe fields, but mutation must be auth-scoped and backend-authoritative.
- Anonymous event share behavior should be backed by RLS/RPC visibility, not only frontend status checks.

## 15. Viewer Role / Entitlement Interaction Map

Social visibility concepts:

- Friend
- Follower / host follower
- Group member
- Invite holder
- Blocked/muted relation

Entitlement and authority concepts:

- Participant
- Ticket holder
- Checked-in
- Host
- Staff
- Ops/admin

Required separation:

- Friend/follower/group membership can affect social visibility and feed eligibility.
- Participant/ticket/check-in status can affect event, commerce, gallery, and check-in access.
- Host/staff/ops roles can affect management and moderation.
- These roles may overlap for one person, but backend authority should evaluate the correct role for each action.

## 16. Dashboard / Ops Social Visibility Surface Map

Observed dashboard social/visibility relevance:

- Dashboard events and host tools read event visibility/status fields.
- Dashboard transfer/admin tooling may expose profile identity and social continuity through host identity transfer reports.
- Dashboard staff/event operations are separate from social graph visibility.

Unknowns:

- Whether dashboard contains active share-group management beyond event publish/product flows.
- Whether ops/admin views can inspect private social graph or group data.
- Whether dashboard event visibility changes are always routed through backend transition/publish authority.

Recommendation: treat dashboard social visibility as operational/admin-sensitive until explicit policy evidence is reviewed.

## 17. Mobile Social Visibility Surface Map

Observed mobile surfaces:

- Friends screen and friend helpers for search, request, response, list, and removal.
- Groups screen for owner-managed share groups and members.
- Host followers screen for followers and blocked users.
- Settings screen for profile visibility and private notification preview settings.
- Event detail and deep-link shim for event visibility.
- Visitor profile for profile visibility, friend/follow state, hosted events, public relics, highlights, and media.
- Publish flow for event visibility and group targets.

Interpretation:

- Mobile is the primary observed social graph and visibility UI surface.
- Mutations are a mix of RPC-mediated friend/follow/block actions and direct share-group table writes.
- Direct share-group access is the main RLS reliance hotspot from this audit.

## 18. Public Web Social Visibility Surface Map

Observed public web/share relevance from prior audits:

- Public event route can expose a compact event landing page.
- Public claim and verification routes are handoff/placeholder style surfaces.
- Public venue/profile/media surfaces exist through RPCs, public buckets, signed media, and app deep links.

Visibility expectations:

- Anonymous viewers should only see public-approved event/profile/venue/media fields.
- Group/private/friends-only content should not be exposed by public routes.
- Public storage exposure is separate from event/profile/group visibility.

## 19. Backend RPC / RLS Authority Evidence Map

Prior production evidence only:

- Production evidence for `share_groups`, `share_group_members`, friendships, follows, host followers, blocks, and mutes was not fully covered in supplied reports.
- Events RLS was confirmed enabled and policy surface exists, but policy correctness still needs deeper review.
- `profiles` / `user_profiles` production RLS/policy evidence was not fully covered.
- Notification, media, and profile visibility contracts remain dependent on their own table/RPC/storage authority.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Production SQL/RPC evidence remains stronger than local source assumptions.

Local-source-only authority signals:

- Friend/follow/block mutations are RPC-mediated.
- Profile visibility settings are RPC-mediated.
- Event publish is RPC-mediated.
- Authenticated event deep-link visibility delegates to `can_view_event_for_user_v1`.
- Share-group management uses direct table access and needs RLS verification.

## 20. Direct Data Access / RLS Reliance Map

| Data surface | Direct access observed | RPC-mediated access observed | Production evidence status | Risk class | Recommendation |
|---|---|---|---|---|---|
| `friendships` | Visitor profile direct relationship read | Friend list/search/request RPCs | Not covered | Privacy-sensitive | Verify RLS for direct reads |
| `friend_requests` | Visitor profile direct request read | Send/respond/cancel/list RPCs | Not covered | Privacy-sensitive | Prefer RPC for relationship state or verify RLS |
| `follows` / host follows | Some feed/social docs and RPC wrappers | Follow/unfollow/stats/following RPCs | Not covered | Privacy-sensitive | Preserve RPC authority |
| Block tables | Not directly observed in focused local helpers | Block/unblock/list RPCs | Not covered | Privacy-sensitive / Security-sensitive | Verify block policy and effects |
| `share_groups` | Direct owner list/create/rename/delete; publish audit reads | Publish RPC links event/group targets | Not covered | Privacy-sensitive | Verify RLS or define RPC contract |
| `share_group_members` | Direct read/insert/delete/count; publish audit reads | None clearly central for member mutations | Not covered | Privacy-sensitive | High-priority policy review |
| `event_share_groups` | Publish audit reads | Publish RPC writes/links | Not covered | Privacy-sensitive | Verify publish and read authority |
| `events.visibility` | Direct event reads in detail/deep-link/profile/feed contexts | Publish RPC, visibility RPC for auth event gate | Events RLS confirmed; correctness incomplete | Privacy-sensitive | Document event visibility RLS/RPC contract |
| `user_profiles` profile visibility | Direct identity reads; profile visibility via RPC | `get_profile_visibility_v1`, `set_profile_visibility_v1` | Not fully covered | Privacy-sensitive | Verify profile RLS |
| Media/comment visibility tables | Mixed; see media audit | Media/comment RPCs | Partial for `event_media` only | Privacy-sensitive | Inherit event/social/media contract |

## 21. Duplicated / Split / Legacy Social Visibility Surfaces

| Surface / helper / RPC / table | Observed role | Current / legacy / unknown | Risk if still active or authoritative | Evidence type | Recommendation |
|---|---|---|---|---|---|
| `private` vs `invite_only` | UI value normalized to DB value | Current mapping | Misread privacy semantics across clients | Local source | Document canonical vocabulary |
| `friends` vs share groups | Friends visibility appears implemented through group targets | Split concept | Confusing friend graph with audience lists | Local docs/source | Define friends/group visibility contract |
| Personal audience list vs membership invite | `accepted` versus `pending` status semantics | Current but not fully canonical | Members may not see private events if status semantics drift | Local source | Document membership status rules |
| Direct visitor relationship reads vs friend RPC wrappers | Relationship state fetched both ways | Split | RLS reliance unclear | Local source | Prefer RPC or verify RLS |
| Public share route vs `can_view_event_for_user_v1` | Anonymous local fallback, authenticated RPC gate | Mixed | Public/private boundary drift | Local source | Align public route with backend authority |
| Host follower stats vs follower list | Stats public-ish, list host-scoped | Current / needs decision | Follower identity exposure if scope broadens | Local source | Document follower field visibility |
| Local Edge Function push eligibility | Mentions mutes/visibility/settings | Local source only | Deployment assumptions may overstate active behavior | Prior reports + local source | Keep separate from production RPC evidence |

## 22. Social-Visibility-Critical Invariants

- Private/group/invite-only event data is not publicly readable unless an explicit backend-authorized share mechanism allows it.
- Friend/follow/group relationship records are owner/member-scoped.
- Social graph visibility does not confer commerce entitlement.
- Ticket/participant entitlement does not automatically confer friend/group visibility.
- Profile visibility is enforced by backend/RPC/RLS, not public avatar URL presence.
- Feed/discovery visibility must match event detail visibility or document intentional differences.
- Media/comment visibility inherits event/media/social visibility.
- Notification and push payloads do not leak hidden/private/group-only content.
- Blocks/mutes affect social visibility and notification eligibility according to product rules.
- Public share links and group memberships remain separate authority concepts.
- Ops/admin visibility does not become public/social visibility.

## 23. Unknown / Needs Verification Surfaces

- Production RLS/policy correctness for `share_groups`, `share_group_members`, `event_share_groups`, friendships, friend requests, follows, host followers, blocks, and mutes.
- Whether `can_view_event_for_user_v1` is the canonical event visibility authority for every authenticated non-host surface.
- Whether anonymous public event reads are always backend/RLS-authoritative.
- Whether friends visibility should mean actual friendship graph, owner-managed audience groups, or both.
- Whether local `members` visibility is active, deprecated, or future-facing.
- Whether follower relationships affect event visibility or only profile/host stats.
- Whether mutes/blocks affect feeds, public profiles, comments, notifications, and push consistently.
- Whether public share surfaces handle private/group/invite-only events through the same backend authority as mobile.

## 24. Social Graph / Groups / Visibility Gaps / Risk Register Seeds

| Gap ID | Domain | Current issue | Expected clean social graph/groups/visibility contract | Risk | Priority candidate | Blocked by | Recommended next action |
|---|---|---|---|---|---|---|---|
| SGV-GAP-001 | Share groups | Direct group/member mutations rely on unverified production RLS | Owner/member-scoped group authority is documented and verified | Privacy-sensitive | Candidate P1 | Production policy evidence | Verify share group policies |
| SGV-GAP-002 | Visibility vocabulary | `private`, `invite_only`, `friends`, `members`, and group concepts are split across UI/docs/source | One canonical visibility vocabulary and mapping | Product correctness / Privacy-sensitive | Candidate P1 | Product decision | Document visibility vocabulary |
| SGV-GAP-003 | Event visibility authority | Event detail, deep-link, feed, and public share use mixed direct/RPC/local checks | One backend-authoritative event visibility contract | Privacy-sensitive | Candidate P1 | RPC/RLS review | Audit feed/detail/public route parity |
| SGV-GAP-004 | Friendship state | Visitor profile direct reads relationship state while helpers use RPCs | Relationship state is RPC-mediated or RLS contract is verified | Privacy-sensitive | Candidate P2 | Social table RLS evidence | Verify relationship read policy |
| SGV-GAP-005 | Profile visibility | `friends_only` behavior is local/RPC-observed but production policy evidence incomplete | Profile visibility enforced across all profile/media/event tabs | Privacy-sensitive | Candidate P1 | Profile RLS/RPC review | Verify profile visibility RPC and table policies |
| SGV-GAP-006 | Blocks/mutes | Block RPC exists; full downstream effects on feeds/media/notifications unknown | Blocks/mutes consistently affect visibility and notification eligibility | Privacy-sensitive / Security-sensitive | Candidate P1 / Unknown | Product decision and policy review | Map block/mute effects |
| SGV-GAP-007 | Notification eligibility | Local push source references visibility/mutes but Edge Function not deployed/visible | Notification eligibility is DB/RPC-authoritative and deployment-aware | Privacy-sensitive | Candidate P2 / Unknown | Edge deployment and DB function evidence | Keep separate notification follow-up |
| SGV-GAP-008 | Public share boundary | Anonymous fallback and public routes need parity with private/group visibility | Public share exposes only public-approved fields | Privacy-sensitive | Candidate P1 | Public route/RLS parity review | Audit public share visibility results |

## 25. Product Decisions Required

- Does `friends` visibility mean accepted friendships, owner-managed share groups, or both?
- Are personal audience lists distinct from membership/invite groups in the accepted product model?
- Should group members always be `accepted` for personal audience lists?
- Is `members` visibility active, future-facing, or legacy?
- Do host followers affect event visibility or only host profile stats/notifications?
- How should blocks and mutes affect profile visibility, event feeds, comments, media, notifications, and push?
- Can participant/ticket-holder access coexist with private social visibility, and which wins for each surface?
- What fields may public share routes reveal for non-public events, if any?

## 26. Recommended Next Audits

- Search / Discovery / Feed Visibility Contract Audit.
- Staff / Host Operations Authority Audit.
- Messaging / Direct Conversation Contract Audit.

## 27. Non-Goals

- No social graph, group, feed, visibility, notification, or profile implementation changes.
- No Supabase migration drafting.
- No SQL creation.
- No production verification query bundle.
- No cleanup, deletion, move, or source-tree normalization.
- No acceptance of vulnerabilities, priorities, or remediation plans.
- No final claim that any social visibility exposure is exploitable.

## 28. Open Questions

- Which backend object is the canonical authority for authenticated event visibility?
- Are share groups private audience lists, membership groups, or both depending on `kind`?
- Which group/member statuses are valid for each group kind?
- Should feed visibility and direct event detail visibility always match?
- Should public share pages call a backend public-read RPC rather than relying on direct table visibility?
- Where are block/mute tables and production policies verified?
- Are follower counts public product signals, authenticated-only signals, or host-only signals?

## 29. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `07_Audits/SocialGraphGroupsVisibilityContractAudit.md` was created/modified.

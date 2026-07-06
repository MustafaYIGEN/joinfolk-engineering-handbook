# Public Web / Share Surface Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false

## 2. Purpose

This audit maps JoinFolk public web and share surfaces: anonymous event share pages, claim handoff, public verification, public venue/profile/media reads, deep links, and storage exposure.

This is not a patch plan, cleanup plan, migration plan, or implementation authorization. It does not authorize backend/RPC/RLS/storage/auth changes. Public web UI behavior is treated as product evidence only; backend/RPC/RLS/storage/auth remains the authority for privacy, revenue, and security-sensitive decisions.

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
- No backend patch or migration is authorized by this audit.

## 4. Public Web / Share Contract Summary

Observed public/share surfaces are split across a small web SPA, mobile/app deep-link handlers, dashboard-generated share/media assets, and backend RPC/storage authority:

- Public web routes under `C:\dev\joinfolk-web\web\src\App.tsx` include `/e/:id`, `/claim/:token`, and `/v/:token`.
- `/e/:id` reads `events` directly for a compact public event landing page.
- `/claim/:token` is a web handoff page and does not validate or mutate claims itself.
- `/v/:token` is a public verification placeholder and does not perform backend verification in the observed local source.
- App/mobile claim routes call claim RPCs such as `get_claim_preview_v1` and `claim_ticket_v1`.
- Public venue/profile/media surfaces use a mix of public RPCs, direct public storage URLs, and signed media URLs.
- Production evidence confirms public storage buckets for `avatars`, `venue-media`, and `venue-posters`; it does not confirm all locally referenced buckets such as `posters`, `event-media`, or `event-videos`.

Clean public contract expectation:

- Public previews expose only approved fields for approved lifecycle and visibility states.
- Public preview pages do not mutate tickets, claims, reservations, check-in proof, media, or notifications.
- Claim, transfer, purchase, proof creation, and ownership mutation are backend-authoritative and auth-scoped.
- Public storage exposure is governed by an accepted product/security decision, not by UI assumptions.

## 5. Public Surface Inventory Matrix

| Surface / route / domain | Public data exposed or action initiated | Auth requirement | Access path observed | Expected authority owner | Production evidence status | Risk class | Recommendation |
|---|---|---:|---|---|---|---|---|
| Web `/e/:id` | Event title, date/time, location label, status/visibility-derived display | Anonymous allowed | Direct `events` read in `EventSharePage.tsx` | RLS/RPC/public contract | Events RLS enabled; policy correctness needs deeper review | Privacy-sensitive / Product correctness | Verify RLS/policies and document public event fields |
| Web `/claim/:token` | Claim token handoff and deep link | Anonymous allowed | UI-only handoff in `ClaimPage.tsx` | Backend claim RPC after handoff | Claim RPC production behavior needs focused verification | Revenue-sensitive | Preserve handoff; document preview/mutation boundary |
| App/mobile `/claim/[token]` | Claim preview and claim action | Preview before claim; mutation should require auth | `get_claim_preview_v1`, `claim_ticket_v1` | Backend/RPC/auth | Tickets and claims RLS enabled with zero direct policies; RPC guards critical | Revenue-sensitive / Privacy-sensitive | Verify claim preview and auth boundary |
| Web `/v/:token` | Verification placeholder and token context | Anonymous allowed | UI-only placeholder in `Verify.tsx` | Backend read-only verification RPC if implemented | `public_verify_checkin` production/local provenance incomplete | Security-sensitive / Product correctness | Reconcile placeholder with backend verification contract |
| Gift claim generation/share | Gift claim token and share URL | Authenticated buyer/owner expected | `create_ticket_claim_v1`, claim link helpers | Backend/RPC/auth | RPC evidence exists; full guard review incomplete | Revenue-sensitive | Document claim token lifecycle |
| Gift transfer handoff | Transfer recipient and claim/token state | Auth required for mutation | `transfer_gift_ticket_v1` | Backend/RPC/auth | Production grant/body detail incomplete in supplied reports | Revenue-sensitive | Verify transfer authority contract |
| Public venue detail | Venue profile, poster, offerings/media | Public or non-owner read | `get_public_venue_v1`, `get_public_venue_detail_v2`, public storage URLs | Backend/RPC/storage | Venue tables RLS enabled; venue media bucket public confirmed | Privacy-sensitive / Product correctness | Document public venue field contract |
| Public venue media | Venue posters/gallery/media | Anonymous public read | `venue-posters`, `venue-media` public URLs | Storage policies + product ADR | Public buckets and public read policies confirmed | Privacy-sensitive | ADR/security decision for public media semantics |
| Public avatars/profile identity | Avatar and public profile identity cues | Public/anonymous likely | `avatars` public URL; public profile/relic RPCs | Storage/RPC/RLS | Avatars public confirmed; profile policy evidence incomplete | Privacy-sensitive | Profile/persona public identity audit |
| Public relic/highlight surfaces | Relic IDs/highlights/winning media | Public or profile viewer | `get_user_relics_public_v1`, `get_host_public_highlights_v1`, `get_event_public_highlights_v1` | Backend/RPC/storage | Local-source evidence; production policy coverage incomplete | Privacy-sensitive / Product correctness | Verify public highlight field contract |
| Feed/discover guest visibility | Public events/venues/cards | Guest/anonymous possible | Feed RPCs and local filters; venue poster public URLs | Backend/RPC/RLS/storage | Events RLS confirmed; policy correctness incomplete | Privacy-sensitive / Product correctness | Verify lifecycle/visibility coupling |
| Public/signed event media | Event media, highlights, poster/video assets | Mixed | `event-media` signed URLs, `posters` public URLs, `event-videos` signed URLs | Storage/RPC/RLS | Not fully covered by supplied production bucket evidence | Privacy-sensitive | Storage policy verification and media ADR |

## 6. Anonymous / Guest / Auth Boundary

Anonymous browser access means a user can load a web URL without a session. Observed examples include `/e/:id`, `/claim/:token`, and `/v/:token`.

Guest or unauthenticated mobile access means a deep link can open app UI before the user proves identity. That UI may display a landing state, but revenue or ownership mutation must wait for backend-authenticated authority.

Authenticated non-participants may be allowed to preview a token-bound claim or public event, but should not receive private/group/invite-only data unless backend rules grant that access.

Participants, ticket holders, hosts, staff, and ops/admin users are separate authority classes. Public share pages may mirror these roles in UI, but role enforcement must remain backend-authoritative.

Allowed-before-auth candidates:

- Public event landing preview for approved public events.
- Claim landing page that explains the handoff.
- Public verification landing page if it remains read-only.

Mutation-after-auth candidates:

- Claim acceptance.
- Gift transfer.
- Ticket purchase or order creation.
- Reservation creation.
- Check-in proof issuance/removal.
- Media upload/update/delete.

## 7. Event Visibility and Lifecycle Contract

Expected public/share behavior by lifecycle:

| Lifecycle state | Expected public/share behavior | Current evidence | Status |
|---|---|---|---|
| draft | Not publicly readable | Public web does not filter locally; relies on backend/RLS | Needs verification |
| published | Publicly readable only when visibility permits | Prior production policy examples allow anon/public published public events | Mostly deterministic, policy review needed |
| live | Publicly readable only when visibility permits | Prior production policy examples include live public events | Mostly deterministic, policy review needed |
| ended | Public memory/share may be allowed by product decision | Prior production evidence mentions ended public visibility in event policies | Needs product contract |
| archived | Public access should be explicit, usually restricted or memory-only | Authenticated entitlement policy examples exist; public behavior unclear | Unknown / Needs verification |
| cancelled/canceled | Public access should be explicit and minimal | Prior policy examples mention authenticated entitlement paths; public behavior unclear | Unknown / Needs verification |
| deleted | Should not be public | No public contract evidence | Unknown / Needs verification |

Expected public/share behavior by visibility:

- `public`: approved fields may be public for approved lifecycle states.
- `private`: not public unless a backend-authorized share mechanism exists.
- `group` / invite-only: not public unless token or membership authority grants access.
- friends/followers: should be backend-authorized, not UI-filtered only.
- participant/ticket-holder: should be backend-authorized through entitlement.
- host/staff/ops/admin: not public.

## 8. Public Event Share Assessment

Observed route: `C:\dev\joinfolk-web\web\src\pages\EventSharePage.tsx`, mounted at `/e/:id`.

Observed behavior:

- Reads the `events` table directly by id.
- Selects a compact field set: id, title, start/end time, location name, visibility, and status.
- Displays title, date, location, status, and a public/friends visibility label.
- Offers a deep link into `joinfolk://e/{id}`.
- Does not locally enforce lifecycle, private/group visibility, ticket ownership, or participant entitlement.

Interpretation:

- This is a public web convenience read that depends on `events` RLS/policy correctness.
- Direct public reads are acceptable only if the public field contract and RLS policy contract are documented and verified.
- The observed page does not expose ticket products, host staff controls, participant lists, private group membership, or media in the inspected local file.
- No production vulnerability is claimed from this evidence; policy correctness remains the authority question.

## 9. Public Venue / Venue Media Assessment

Observed public venue surfaces include:

- Mobile venue route calls to `get_public_venue_v1`.
- Venue detail helper calls to `get_public_venue_detail_v2`.
- Venue media/offering list helpers such as `list_venue_media_v1` and `list_venue_offerings_v1`.
- Public venue poster URLs built from `venue-posters`.
- Public venue gallery URLs built from `venue-media`.

Production evidence:

- `venue-media` and `venue-posters` buckets are public.
- Public read policies exist for those buckets.
- Supplied write policy examples appear host/owner constrained.

Interpretation:

- Public venue context appears product-intended, but the exact approved public venue field set is not canonical in current handbook evidence.
- Public media exposure is not automatically a bug; it requires an accepted ADR/security decision.
- Venue public read RPCs should be the canonical way to expose venue detail fields beyond static public storage URLs.

## 10. Public Profile / Host Identity Assessment

Observed public identity surfaces include:

- Public avatar URLs from the `avatars` bucket.
- Public relic reads through `get_user_relics_public_v1`.
- Host/event highlight reads through `get_host_public_highlights_v1` and `get_event_public_highlights_v1`.
- Event share pages that may imply host/event identity through title and location, though the inspected web event share page did not select host profile fields.

Production evidence:

- `avatars` bucket is public with public read policy.
- Production policy evidence for profile/persona tables was not included in the supplied reports.

Interpretation:

- Public host/persona identity needs a documented contract: which profile fields, avatars, relics, host status, tiers, and highlights are public.
- Persona exposure must not be inferred from UI alone.
- This should feed a dedicated Profile / Persona / Public Identity Contract Audit.

## 11. Claim / Gift / Ticket Handoff Assessment

Observed web handoff:

- `C:\dev\joinfolk-web\web\src\pages\ClaimPage.tsx` renders `/claim/:token`.
- It displays a gift claim token and app/deep-link handoff.
- It does not call Supabase or validate the claim itself.

Observed app/mobile behavior:

- App claim routes use `get_claim_preview_v1` for preview.
- Claim mutation uses `claim_ticket_v1`.
- Gift screens create, list, revoke, and share claims through RPC wrappers such as `create_ticket_claim_v1`, `get_my_ticket_claims_v1`, and `revoke_ticket_claim_v1`.
- Transfer flows use `transfer_gift_ticket_v1`.

Production evidence:

- `tickets` and `event_ticket_claims_v1` have RLS enabled with zero direct policies in focused evidence.
- That likely makes direct table access default-deny and makes RPC internal guards critical.

Interpretation:

- Public claim preview is token-bound and may intentionally reveal limited event/product/expiry status to the holder of the link.
- Claim acceptance, transfer, revocation, and ticket issuance are revenue-sensitive and must be backend-authoritative.
- The handoff boundary is product-critical: preview before auth may be acceptable, mutation before auth is not.

## 12. Public Check-in Proof / Verification Assessment

Observed public web route:

- `C:\dev\joinfolk-web\web\src\pages\Verify.tsx` renders `/v/:token`.
- The page is a placeholder and states that full verification logic is deferred.
- The inspected local web route does not call a backend verification RPC and does not mutate proof state.

Prior backend/provenance evidence:

- `public_verify_checkin` appeared in prior broad execute/function evidence.
- Production/local provenance for `public_verify_checkin` remains incomplete; the provenance report noted it was unmatched in local migration trees.
- Proof-related mutation functions are tracked separately in the proof check-in hardening draft.

Interpretation:

- Public verification should be a read-only verification surface if product-approved.
- Public verification must not issue, create, remove, or mutate proof.
- Proof mutation remains backend-authoritative and should not be inferred from this public placeholder.

## 13. Public Storage Bucket Exposure Assessment

| Bucket / media surface | Public read status | Expected product meaning | Privacy exposure risk | Recommendation |
|---|---|---|---|---|
| `avatars` | Confirmed public in production evidence | Public identity/avatar display | Profile/persona exposure if fields are not scoped | Document public identity contract |
| `venue-posters` | Confirmed public in production evidence | Public venue/event poster context | Venue/media exposure if non-public assets are uploaded | ADR/storage policy verification |
| `venue-media` | Confirmed public in production evidence | Public venue gallery/media | Venue privacy depends on accepted-public semantics | ADR/storage policy verification |
| `posters` | Local source uses public URLs | Event/poster snapshot | Production bucket state not supplied | Needs production storage verification |
| `event-media` | Local source uses signed URLs and some fallback public URL helpers | Event gallery/highlights/memory wall | Could expose participant/media content if policy is wrong | Needs production storage verification |
| `event-videos` | Local source uses signed URLs | Poster/video media | Production bucket state not supplied | Needs production storage verification |

## 14. Direct Table Read / RLS Reliance Map

| Surface | Direct table/storage access observed | RLS/storage reliance | Risk class | Recommendation |
|---|---|---|---|---|
| Web `/e/:id` | Direct `events` read | Events RLS and select policies | Privacy-sensitive | Verify event public/share policy contract |
| App gift transfer screen | Direct `events` read for host ownership context observed in local source | Events RLS and RPC mutation guard | Revenue-sensitive / Product correctness | Keep mutation RPC-authoritative |
| Public venue cards | Direct public storage URL construction for `venue-posters` | Storage public policy | Privacy-sensitive | ADR for public venue media |
| Venue buyer/gallery surfaces | Public `venue-media` URLs | Storage public policy | Privacy-sensitive / Product correctness | Document media field/bucket contract |
| Event media/highlights | Signed URLs and local public fallback helpers | Storage policy and signing authority | Privacy-sensitive | Verify event media bucket policy |
| Claim handoff web | No direct Supabase data read observed | Backend preview occurs in app/RPC | Revenue-sensitive | Preserve preview/mutation split |

Direct access is not automatically unsafe. It is acceptable for public UI display only where RLS/storage policies are documented and verified for the exact public contract.

## 15. RPC-Mediated Public Read / Handoff Map

| RPC / authority path | Public/share role | Mutation or read | Evidence source | Status |
|---|---|---:|---|---|
| `get_claim_preview_v1` | Claim preview for token holder | Read/preview | App/mobile and shared wrappers | Needs auth/anon behavior verification |
| `claim_ticket_v1` | Claim acceptance | Mutation | App/mobile and shared wrappers | Backend-authoritative required |
| `create_ticket_claim_v1` | Gift claim creation | Mutation | Gift flows and wrappers | Backend-authoritative required |
| `revoke_ticket_claim_v1` | Claim revocation | Mutation | Gift management wrappers | Backend-authoritative required |
| `transfer_gift_ticket_v1` | Gift transfer | Mutation | Transfer/share wrappers | Backend-authoritative required |
| `get_public_venue_v1` | Public venue read | Read | Mobile venue route/helper | Public field contract needed |
| `get_public_venue_detail_v2` | Venue detail/media/offering read | Read | Venue detail helper | Public field contract needed |
| `list_venue_media_v1` | Venue media list | Read | Venue detail helper | Storage/RPC public contract needed |
| `get_user_relics_public_v1` | Public relic read | Read | Social relic helper | Persona/profile contract needed |
| `get_host_public_highlights_v1` | Host highlight read | Read | Host highlight helper | Media privacy verification needed |
| `get_event_public_highlights_v1` | Event highlight read | Read | Event highlight helper | Lifecycle/media contract needed |
| `public_verify_checkin` | Public proof verification candidate | Expected read-only | Prior function/provenance evidence | Unknown / Needs verification |

## 16. Public Web Route Map

Observed `C:\dev\joinfolk-web\web\src\App.tsx` route map:

- `/`: Home.
- `/e/:id`: public event share landing page.
- `/claim/:token`: gift claim handoff page.
- `/v/:token`: public verification placeholder page.
- `*`: Home fallback.

Observed behavior:

- The event share route performs a direct public event read.
- The claim route is handoff-only and does not validate or mutate claim state.
- The verify route is placeholder-only in local source.

## 17. Mobile Deep-Link / Share Handoff Map

Observed link helpers and routes:

- `eventWebUrl(eventId)` builds `https://app.join-folk.com/e/{eventId}`.
- `claimWebUrl(token)` builds `https://app.join-folk.com/claim/{token}`.
- `claimDeepUrl(token)` builds `joinfolk://claim/{token}`.
- Public event deep links use `joinfolk://e/{id}`.
- Mobile/app shims route `/e/:id` into event detail.
- Mobile/app claim routes handle preview and acceptance through claim RPCs.
- Gift claim management and share screens generate public claim URLs.

Interpretation:

- Web is primarily a public/share landing layer.
- App/mobile is the authority-aware handoff layer for claim preview/mutation.
- Deep links must not bypass backend lifecycle, visibility, entitlement, or ownership checks.

## 18. Dashboard / Ops Share-Surface Interactions

Observed dashboard/public-share adjacent behavior:

- Dashboard API helpers generate public URLs for `venue-posters`, `venue-media`, and `posters`.
- Dashboard helpers create signed URLs for event media.
- Dashboard host flows can create public-facing event/venue/media state even if the dashboard itself is not a public route.
- A dashboard functions route under `dashboard/functions/e/[id].ts` appears intended to intercept public event share requests for visitors, but this audit did not verify deployment behavior.

Interpretation:

- Dashboard is not the public consumer surface, but it can publish assets and metadata consumed by public surfaces.
- Ops/admin or dashboard-only tools must not be treated as public authority.
- Any server-side public share function needs deployment and authority verification before it can be considered production-active.

## 19. Backend RPC / RLS / Storage Authority Evidence Map

Known production evidence from prior handbook reports:

- `events` table RLS was confirmed enabled and policies exist, but full policy correctness still needs deeper review.
- Storage buckets `avatars`, `venue-media`, and `venue-posters` were confirmed public with public-read and constrained write-policy evidence.
- `tickets` and `event_ticket_claims_v1` had RLS enabled with zero direct policies, making claim/transfer RPC guards critical.
- `commerce_orders` had deny-all style policy evidence.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Database Functions / RPC evidence is separate from Edge Function deployment evidence.
- Production SQL/RPC evidence remains stronger than local source assumptions.

Unreviewed public routes, tables, functions, and buckets must not be treated as safe solely because a UI path exists or because RLS is enabled.

## 20. Duplicated / Split / Legacy Public Surfaces

| Surface / route / helper / RPC | Observed role | Current / legacy / unknown | Risk if still active or authoritative | Evidence type | Recommendation |
|---|---|---|---|---|---|
| Web `/claim/:token` vs app `/claim/[token]` | Handoff-only web page vs RPC-backed app claim flow | Current split | User may confuse preview/handoff with authority | Local source | Document handoff contract |
| Web `/v/:token` vs `public_verify_checkin` | Placeholder UI vs backend verification candidate | Unknown | Verification contract may be incomplete or split | Local source + prior reports | Verify read-only proof contract |
| Public event direct read vs feed/discover RPCs | Direct public event share vs RPC/list feeds | Current split | Visibility/lifecycle logic can drift | Local source | Document canonical public event read contract |
| `get_public_venue_v1` vs `get_public_venue_detail_v2` | Public venue read versions | Current / versioned | Field-set drift | Local source | Define public venue field contract |
| `venue-media` public bucket vs `event-media` signed/local fallback | Venue public media vs event participant media | Mixed | Public/private media semantics may blur | Production + local source | ADR/storage policy review |
| Claim link helpers in web/mobile code | Public URL/deep-link construction | Current duplicated | Link behavior drift | Local source | Keep canonical link helper contract |
| Dashboard share intercept function | Public event share helper candidate | Unknown deployment status | Could create alternate public share behavior | Local source only | Verify deployment before relying on it |

## 21. Public-Surface-Critical Invariants

- Draft/private/group/invite-only event data is not publicly readable unless explicit share authority allows it.
- Public event share only exposes approved fields for approved visibility and lifecycle states.
- Public previews cannot mutate tickets, claims, reservations, check-in, proof, media, or notifications.
- Claim/transfer mutation is auth-scoped and backend-authoritative.
- Public proof verification cannot issue or mutate proof.
- Public storage buckets expose only media accepted as public by product/security decision.
- Signed URLs are generated only for authorized viewers where needed.
- Host identity/public profile exposure follows accepted persona contract.
- Public share/deep-link handoff does not bypass lifecycle, visibility, entitlement, or ownership checks.

## 22. Unknown / Needs Verification Surfaces

- Whether `/e/:id` can ever return private/group/invite/friend-only event data in production; this depends on RLS policy correctness.
- Exact public event field contract for title, host identity, venue/location, description, ticket/reservation CTA, media, social counts, and lifecycle labels.
- Whether claim preview allows anonymous preview or requires auth in production.
- Exact fields exposed by `get_claim_preview_v1` and whether token possession is the intended preview authority.
- Whether `public_verify_checkin` is live, read-only, and scoped to approved verification fields.
- Production storage status for `posters`, `event-media`, and `event-videos`.
- Public host/profile/persona policy for avatars, relics, tiers, highlights, and host identity.
- Whether dashboard functions for public event share are deployed.
- Whether public venue offerings/media contain fields that should be non-public.

## 23. Public Web / Share Gaps / Risk Register Seeds

### PWS-GAP-001

- Domain: Public event share
- Current issue: `/e/:id` directly reads `events`; frontend does not locally enforce lifecycle or visibility.
- Expected clean public/share contract: RLS/RPC exposes only approved fields for approved public lifecycle and visibility states.
- Risk: Private/group/invite-only event details could be exposed if backend policy is wrong.
- Priority candidate: Candidate P1
- Blocked by: Production policy correctness review and public field contract decision.
- Recommended next action: Verify event public/share RLS policy and document approved public event fields.

### PWS-GAP-002

- Domain: Claim / gift handoff
- Current issue: Web claim page is handoff-only, while app claim route performs preview and mutation through RPCs.
- Expected clean public/share contract: Token preview and authenticated mutation boundaries are explicit and backend-authoritative.
- Risk: Revenue and ownership confusion if preview, claim, and transfer boundaries drift.
- Priority candidate: Candidate P1
- Blocked by: Claim RPC body/grant review and product decision on anonymous preview.
- Recommended next action: Audit claim preview and claim mutation contract.

### PWS-GAP-003

- Domain: Public check-in verification
- Current issue: Web `/v/:token` is a placeholder while `public_verify_checkin` appears in prior backend evidence with incomplete provenance.
- Expected clean public/share contract: Public verification is read-only and cannot issue or mutate proof.
- Risk: Security/product correctness risk if verification and proof mutation are conflated.
- Priority candidate: Candidate P1 / Unknown
- Blocked by: Production function existence/body/grant verification.
- Recommended next action: Verify public verification RPC status and separate it from proof mutation hardening.

### PWS-GAP-004

- Domain: Public storage exposure
- Current issue: `avatars`, `venue-media`, and `venue-posters` are public in production; local source also references other media buckets whose production posture is not supplied.
- Expected clean public/share contract: Each public bucket has accepted product/security semantics and documented write authority.
- Risk: Privacy exposure if private media lands in public buckets or if unreviewed buckets are public.
- Priority candidate: Candidate P2
- Blocked by: Storage ADR and production bucket/policy verification for remaining buckets.
- Recommended next action: Run a media/storage contract audit.

### PWS-GAP-005

- Domain: Public venue and venue media
- Current issue: Public venue detail and media are exposed through versioned RPCs and public storage, but public field set is not canonical.
- Expected clean public/share contract: Venue public profile, offerings, poster, gallery, and owner identity fields are explicitly accepted.
- Risk: Privacy/product correctness risk from overexposure or inconsistent venue previews.
- Priority candidate: Candidate P2
- Blocked by: Product/security decision for public venue semantics.
- Recommended next action: Document public venue contract and verify RPC field sets.

### PWS-GAP-006

- Domain: Public profile / host identity
- Current issue: Public avatars, relics, and highlight RPCs exist, but public persona/profile contract is incomplete.
- Expected clean public/share contract: Public identity fields are accepted and consistent across event, venue, profile, and media surfaces.
- Risk: Privacy/persona leakage or inconsistent host identity.
- Priority candidate: Candidate P2
- Blocked by: Profile/persona public identity decision.
- Recommended next action: Perform Profile / Persona / Public Identity Contract Audit.

### PWS-GAP-007

- Domain: Public highlights / memory media
- Current issue: Event and host public highlight RPCs plus signed/public media helpers exist, but lifecycle/media visibility contract is incomplete.
- Expected clean public/share contract: Public highlights are exposed only after accepted lifecycle and viewer rules.
- Risk: Participant media privacy risk if highlight selection or storage access is wrong.
- Priority candidate: Candidate P2
- Blocked by: Media/gallery/memory wall authority review.
- Recommended next action: Perform Media / Gallery / Memory Wall Contract Audit.

### PWS-GAP-008

- Domain: Split public web/app behavior
- Current issue: Web pages, app deep-link handlers, dashboard share helpers, and mobile public surfaces implement adjacent share behavior.
- Expected clean public/share contract: Web and app handoff surfaces share one canonical contract for preview, auth, and mutation.
- Risk: Product correctness drift and inconsistent privacy expectations.
- Priority candidate: Candidate P3
- Blocked by: Route/deep-link contract decision.
- Recommended next action: Document canonical public route and deep-link behavior.

## 24. Product Decisions Required

- Which event fields are approved for anonymous `/e/:id` public share?
- Are ended events public-readable when visibility is public, and what memory-wall fields are included?
- Are archived/cancelled event share pages hidden, limited, or still visible?
- Is claim preview allowed before authentication, and which fields may token holders see?
- What is the accepted public verification product contract for `/v/:token`?
- Which storage buckets are intentionally public, and which require signed/private access?
- Which host/persona/profile fields are public across event, venue, and profile surfaces?
- Are public venue offerings and media intended to be anonymous-public?
- Should public share pages use direct table reads, RPC-mediated reads, or a documented mix?

## 25. Recommended Next Audits

1. Notification / Push / Reminder Contract Audit

   Focus on public/share and claim flows that can trigger notifications, reminders, push eligibility, token ownership, and user-visible lifecycle messages.

2. Media / Gallery / Memory Wall Contract Audit

   Focus on public highlights, event media, memory wall exposure, signed URLs, public buckets, moderation, hide/delete behavior, and lifecycle-specific media visibility.

3. Profile / Persona / Public Identity Contract Audit

   Focus on avatars, host identity, public relics, tiers, organizer persona fields, profile visibility, and canonical public identity field contracts.
## 26. Non-Goals

- No application, dashboard, mobile, web, or Supabase code changes.
- No SQL, migration, policy, storage, or RPC implementation.
- No production connection or Supabase CLI usage.
- No claim that public access is unsafe solely because it exists.
- No claim that public storage is unsafe solely because a bucket is public.
- No claim that RLS is correct solely because it is enabled.
- No feature removal or cleanup recommendation.
- No release readiness conclusion.

## 27. Open Questions

- Is `/e/:id` intended to be the canonical public event share route for all public events?
- Should public event share use a dedicated RPC instead of direct `events` reads?
- Which claim preview fields are allowed before login?
- Is `public_verify_checkin` intended to back `/v/:token`, and is it read-only in production?
- Are `posters`, `event-media`, and `event-videos` production buckets, and what are their public/signed policies?
- Should public venue detail include offerings, media, host identity, and availability cues?
- Should group/invite/friend visibility ever produce a public share preview?
- Is the dashboard public event share function deployed or stale/local-only?

## 28. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `07_Audits/PublicWebShareSurfaceAudit.md` was created/modified.

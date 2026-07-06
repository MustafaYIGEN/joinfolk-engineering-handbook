# Profile / Persona / Public Identity Contract Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false

## 2. Purpose

This audit maps JoinFolk profile, persona, and public identity contracts across personal profiles, host/organizer personas, avatars, public profile views, event host identity, venue/business identity, media uploader identity, notification identity payloads, tier/account capability display, and host identity transfer behavior.

This is not a patch plan, cleanup plan, migration plan, or implementation authorization. It does not authorize backend/RPC/RLS/storage/auth changes. Frontend identity rendering is treated as product evidence only; profile/persona visibility, ownership, public identity field exposure, and capability enforcement must be backend/RPC/RLS/storage-authoritative.

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

## 4. Profile / Persona / Public Identity Contract Summary

Observed identity architecture has a practical center around `user_profiles`, with a legacy or secondary `profiles` projection still used by some screens and update paths.

Observed identity concepts:

- Personal identity: `display_name`, `avatar_url`, `personal_avatar_url`, `bio`, `username`.
- Host/organizer persona identity: `organizer_display_name`, `organizer_avatar_url`, `organizer_bio`, host-mode rendering, and event `created_under_persona`.
- Capability/tier identity: `tier` / account tier values such as `user`, `semi_pro`, and `pro`.
- Public identity projections: public profile views, host highlights, public relics, event host display, venue/business surfaces, media owner labels, and notification actor identity.
- Avatar identity: public `avatars` storage URLs plus profile table fields pointing to those URLs.
- Transfer/copied identity: host identity transfer copies organizer-facing identity while preserving target personal identity, based on prior handbook evidence.

Clean contract expectation:

- Public identity fields are explicitly documented by surface.
- Personal identity and host/organizer persona identity remain separate unless product explicitly merges them.
- `user_profiles` versus `profiles` ownership is reconciled or documented.
- Avatar storage public-read semantics do not imply all profile fields are public.
- Tier/account capability display does not confer capability.
- Persona UI state mirrors backend authority; it does not decide ownership, tier eligibility, staff authority, or ops/admin authority.

## 5. Identity Surface Inventory Matrix

| Surface / domain | Identity exposed or action initiated | Access path observed | Expected authority owner | Visibility scope | Production evidence status | Risk class | Recommendation |
|---|---|---|---|---|---|---|---|
| Current user profile | `display_name`, avatar fields, username, tier, bios | Direct `user_profiles` read in mobile profile context | Backend/RLS/auth | Owner/authenticated | Production profile RLS evidence not fully covered | Privacy-sensitive | Document owner/private field contract |
| Unified identity resolver | Personal and organizer identity fields for arbitrary users | Direct `user_profiles` read with cache | Backend/RLS/RPC | Mixed public/authenticated depending surface | Production profile policy evidence incomplete | Privacy-sensitive | Verify RLS or define public RPC contract |
| Legacy profile projection | `profiles.display_name`, `profiles.avatar_url` | Direct read/update in mobile/dashboard surfaces | Backend/RLS/auth | Unknown | Not fully covered by supplied production evidence | Product correctness / Privacy-sensitive | Reconcile with `user_profiles` |
| Host/organizer persona | Organizer display name, avatar, bio | Direct `user_profiles` read and persona helpers | Backend/RPC/RLS/auth | Public host profile, event host, owner edit | Profile policy evidence incomplete | Product correctness / Privacy-sensitive | Document canonical host persona contract |
| Avatar storage | Avatar public URLs | `avatars` storage upload/read helpers | Storage policy + profile table authority | Public image URL, owner upload | `avatars` bucket public-read confirmed with constrained write-policy evidence | Privacy-sensitive | Keep as ADR/storage contract item |
| Event host identity | Host display/avatar in event feeds/detail/public event surfaces | `events.host_id`, `created_under_persona`, `user_profiles` hydration | Backend/RPC/RLS + UI rendering | Public/auth/event viewer | Events RLS confirmed; profile field policy incomplete | Privacy-sensitive / Product correctness | Document event host identity field contract |
| Venue/business identity | Venue public details, venue owner/business presentation | Public venue RPCs and venue media paths | Backend/RPC/RLS/storage | Public venue viewer, host/owner | Venue RLS and venue media storage evidence exists; field contract incomplete | Privacy-sensitive / Product correctness | Separate venue/business identity from personal identity |
| Media uploader identity | Owner display/avatar, comments, likes, public highlights | Media RPCs plus identity hydration | Backend/RPC/RLS/storage | Participant/public highlight/owner | Event media RLS confirmed; profile identity policy incomplete | Privacy-sensitive | Verify media identity masking contract |
| Public relics/highlights | Public-safe relic IDs and host/event highlights | Public RPC wrappers | Backend/RPC/RLS/storage | Public profile/share surface | Local/provenance evidence; production field contract incomplete | Privacy-sensitive | Document public profile payload |
| Notification actor identity | Actor display/avatar/persona, target persona | Notification V2 RPC contract and mobile rendering | Backend/RPC/RLS/auth | Notification owner only | Notification tables RLS confirmed; payload policy correctness not fully reviewed | Privacy-sensitive | Define notification identity payload contract |
| Tier/account capability | `tier`, `account_tier`, semi-pro/pro host affordances | Direct reads and UI guards | Backend/RPC/RLS/auth | Owner, dashboard, sometimes profile-facing | Production profile RLS evidence incomplete | Security-sensitive / Product correctness | Keep capability enforcement backend-authoritative |
| Host identity transfer | Transfer persona identity from source to target | Dashboard transfer API + admin RPC evidence | Backend/RPC/ops authority | Ops/admin only | Production RPC exists with internal ops gate; profile effects need field contract | Operational/admin-sensitive | Document transfer identity semantics |

## 6. Personal Profile Contract Assessment

Observed personal profile fields include `display_name`, `avatar_url`, `personal_avatar_url`, `bio`, `username`, `tier`, and `updated_at`.

Mobile profile context reads `user_profiles` directly for the current authenticated user and exposes the result as the shared runtime identity model. The identity resolver also reads `user_profiles` for other users to hydrate feeds, event detail, social rows, comments, notifications, and profile screens.

Personal profile editing is owner-scoped by product expectation, but supplied production reports did not fully verify `user_profiles` or `profiles` RLS/policy correctness. Until production policy evidence is reviewed, direct profile writes remain Unknown / Needs verification.

Expected clean contract:

- Owner can edit approved personal fields.
- Public viewers see only approved public profile fields.
- Personal fields do not automatically become event-host or organizer fields.
- `avatar_url` legacy fallback behavior is documented or retired later through a separate accepted plan.

## 7. Host / Organizer Persona Contract Assessment

Observed host/organizer persona fields include `organizer_display_name`, `organizer_avatar_url`, `organizer_bio`, and event `created_under_persona`.

Mobile persona logic treats `user` as personal-only, `semi_pro` as personal plus host with personal default, and `pro` as personal plus host with host default. Active persona is stored in local UI state, while tier/capability must be enforced by backend authority.

Observed host identity rendering chooses organizer fields for host persona and personal fields for personal persona. Event cards and event details resolve host display/avatar from `user_profiles` and `created_under_persona`.

Expected clean contract:

- Host persona is a product-level identity distinct from personal profile identity.
- Persona selection can guide UI rendering but cannot authorize event creation, host ownership, venue mutation, staff/admin behavior, or tier-gated capability.
- Host persona public fields are documented and stable across mobile, dashboard, public web, notifications, and media.

## 8. Public Profile / Public Identity Field Contract

Observed public profile behavior includes:

- Public or visitor profile screen with personal and host modes.
- Public relic reads through `get_user_relics_public_v1`, described locally as returning only presentation-safe relic data.
- Host highlights through `get_host_public_highlights_v1`.
- Event public highlights through `get_event_public_highlights_v1`.
- Host follow stats through `get_host_profile_stats_v1`.
- Public profile visibility helper `get_profile_visibility_v1`.

Field contract remains incomplete. The code strongly suggests a public identity surface, but the canonical list of public fields is not yet documented as an accepted product/security contract.

Expected clean contract:

- Public profile mode exposes only approved identity fields.
- Friends-only or private profile behavior is backend-authoritative.
- Public relic/highlight surfaces omit internal source metadata and private uploader details unless explicitly accepted.
- Public profile visibility is enforced by backend/RPC/RLS, not frontend route conditions alone.

## 9. Avatar Storage and Public URL Assessment

Production evidence previously confirmed the `avatars` bucket is public, has public-read policy evidence, and has constrained write-policy evidence based on authenticated folder ownership.

Observed local source:

- Mobile helpers upload avatar images to `avatars`.
- Public URLs are generated through storage helpers.
- `avatar_url` is a legacy/full-URL field.
- `personal_avatar_url` and `organizer_avatar_url` are persona-specific fields.
- Some helper comments indicate future normalization toward storage-path-only fields, but this audit does not authorize that work.

Public avatar storage is not automatically a bug. It requires an accepted product/security decision that public avatars are intended public media and that profile field exposure remains separately controlled.

## 10. Event Host Identity Exposure Map

Observed event-host identity surfaces:

| Surface | Observed identity behavior | Authority expectation | Status |
|---|---|---|---|
| Mobile home/discover cards | Resolve host identity using `created_under_persona` and `user_profiles` organizer/personal fields | Backend event visibility plus profile field contract | Needs verification |
| Mobile event detail | Reads host profile fields and resolves persona-specific display/avatar | Backend event read authority plus profile field contract | Needs verification |
| Dashboard event creation | Direct event draft insert uses `host_id` and `created_under_persona: "host"` | Backend/RLS/RPC must enforce host authority | Needs verification |
| Public event share | Prior audit found public event share surfaces; host identity field contract incomplete | Backend/RLS/RPC public field contract | Needs verification |
| Ticket/commerce surfaces | Host/event identity appears in purchase and claim context | Backend/RPC commerce authority and profile field contract | Needs verification |

Clean contract expectation: public event host identity should use host persona fields when an event is created under host persona, and should not expose private personal profile fields unless explicitly accepted.

## 11. Venue / Business Identity Exposure Map

Observed venue identity surfaces include public venue RPCs such as `get_public_venue_v1` and `get_public_venue_detail_v2`, venue media, venue posters, dashboard venue surfaces, and venue owner/host context.

Venue/business identity should remain separate from personal profile and host persona identity:

- Venue public detail can be product-public if accepted.
- Venue owner authority must remain backend/RPC/RLS-authoritative.
- Venue public media is governed by the venue-media ADR/storage decision.
- Venue/business display should not accidentally expose private owner profile fields.

Production evidence covers venue/venue_media RLS and public venue media storage at a high level, but not a complete public venue identity field contract.

## 12. Media Uploader / Highlight / Relic Identity Exposure Map

Observed media identity surfaces:

- Event media and home/photo feeds hydrate owner display/avatar from identity resolvers.
- Comments and likes hydrate current profile avatars and names.
- Public highlights are intended to be presentation-safe and avoid uploader identity in local reports.
- Public relic reads are described as exposing relic presentation fields while omitting internal source details.
- Visitor profile photo grids use signed media URLs and owner identity display.

Expected clean contract:

- Private/event-only media may display uploader identity only to authorized viewers.
- Public highlights/relics expose only accepted public identity fields.
- Signed media URLs and public profile identity are separate contracts.
- Notification payloads referencing media must not leak private identity or media context.

## 13. Notification Payload Identity Exposure Map

Notification V2 local types include actor fields: `actor_user_id`, `actor_display_name`, `actor_avatar_url`, `actor_persona`, `target_persona`, plus event, host, conversation, media, and deep-link fields.

Observed mobile notification rendering rehydrates actor identity and chooses organizer or personal display/avatar based on actor persona. This improves UI freshness but also means payload identity and live identity may diverge.

Expected clean contract:

- Notification records are owner-visible only unless a specific ops/admin exception is defined.
- Actor identity fields are safe snapshots or safe live projections for the notification recipient.
- Notification identity does not bypass event, group, claim, media, profile, or conversation visibility.
- Host/persona actor fields are used only where product semantics expect host identity.

## 14. Tier / Account Capability Exposure Map

Observed tier concepts include `user`, `semi_pro`, and `pro`. Mobile persona availability and dashboard route guards use tier values. Dashboard profile fetches read `tier` from `user_profiles`; dashboard guards also check ops/admin flags for restricted views.

Expected clean contract:

- Tier may be displayed as product identity only where accepted.
- Tier display does not confer capability.
- Event creation, host persona use, venue operations, dashboard access, and ops/admin behavior must be backend-authoritative.
- Public tier badges, if product-approved, must not leak private account state beyond the accepted public field contract.

## 15. Host Identity Transfer / Persona Copy Assessment

Prior handbook and production parity evidence confirmed `admin_execute_host_identity_transfer_v1` exists in production, is SECURITY DEFINER, has `search_path=public`, and has an internal `auth_is_ops()` gate. Broad execute privileges were noted, but exploitability was not claimed because the internal ops gate exists.

Known persona copy behavior from prior migration/report evidence:

- Source `organizer_display_name` falls back to source `display_name`.
- Source `organizer_avatar_url` falls back to source `avatar_url`.
- Source `organizer_bio` falls back to source `bio`.
- Target `user_profiles` organizer fields are set.
- Target personal identity is preserved.
- `profiles` mirror `display_name` and `avatar_url` are set to the transferred host display/avatar.
- Source organizer fields are cleared.

This behavior is not re-authorized patch work. It is identity-contract evidence only.

Product/security implications:

- Transfer affects public host identity, event trust, venue/business continuity, followers, notifications, and profile display.
- The `profiles` mirror update may blur personal/public identity unless documented.
- Transfer history and audit views are ops/admin-sensitive and must not become public profile surfaces.

## 16. Dashboard / Ops Identity Surface Map

Observed dashboard/ops identity surfaces:

- Dashboard profile fetch reads `user_profiles` for display/avatar/tier and optionally ops status.
- Dashboard profile update writes `user_profiles.display_name` and `profiles.display_name`.
- Dashboard host event fetch filters by `host_id` and `created_under_persona`.
- Dashboard host identity transfer reads transfer rows and enriches display names from `user_profiles`.
- Dashboard transfer actions call ops/admin RPCs.
- Staff/user search surfaces read profile display names for assignment UX.

Interpretation:

- Dashboard UI gates are useful guidance, but ops/admin and host authority must be backend-authoritative.
- Direct profile updates and legacy mirror writes need an explicit source-of-truth contract.
- Host identity transfer views are operational/admin-sensitive.

## 17. Mobile Identity Surface Map

Observed mobile identity surfaces:

- `profile-context.tsx` loads current user identity from `user_profiles`.
- `identity.ts` resolves arbitrary user identities from `user_profiles`, caches them, and resolves persona-specific avatars.
- `persona.ts` stores active persona locally and derives available personas from tier.
- Event detail/home/discover surfaces render host identity based on event persona and `user_profiles`.
- Visitor profile uses `resolveUserIdentity`, public relics, host highlights, profile visibility, friend/follow state, and public/authorized media.
- Notifications render actor identity with persona-specific display/avatar.
- Avatar upload writes storage and then updates `user_profiles`.

Interpretation:

- Mobile has a coherent identity helper layer, but it still depends on direct profile reads and writes.
- Persona is a UI state and identity rendering tool, not an authority boundary by itself.
- Public profile field exposure needs backend/RLS/RPC verification.

## 18. Public Web / Share Identity Surface Map

Prior public web/share audit found public event, claim, verification, venue, media, avatar, relic, and highlight surfaces.

Identity-relevant expectations:

- Public event share may show host identity only through the accepted public event identity contract.
- Claim handoff may preview event/host context, but claim mutation must be auth-scoped and backend-authoritative.
- Public venue share may expose venue/business identity but should not expose private owner identity.
- Public avatars are acceptable only under an accepted public-avatar product/security decision.
- Public relics/highlights must avoid private profile, media, and source metadata unless accepted.

## 19. Backend RPC / RLS / Storage Authority Evidence Map

Production and prior-report evidence:

- `profiles` / `user_profiles` production RLS and policy correctness were not fully covered by supplied production reports.
- `avatars` storage bucket was confirmed public with public-read and constrained write-policy evidence.
- Event, media, and venue public identity exposure depends on their own RLS/RPC/storage contracts.
- Events RLS was confirmed enabled and policy surface exists, but policy correctness still needs deeper review.
- `event_media` and `venue_media` RLS were confirmed enabled; media policy correctness remains separate.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Production SQL/RPC evidence remains stronger than local source assumptions.

Authority interpretation:

- Public identity claims cannot rely on frontend rendering alone.
- Direct `user_profiles` reads require documented RLS policy behavior or a public/profile RPC contract.
- Avatar storage policy evidence is not enough to prove profile table field visibility is correct.

## 20. Direct Data Access / RLS Reliance Map

| Data surface | Direct access observed | Authority reliance | Production evidence status | Recommendation |
|---|---|---|---|---|
| `user_profiles` | Mobile identity/profile reads, dashboard profile reads, profile enrichment, tier reads | RLS/policy or RPC contract | Not fully covered | Verify and document profile RLS contract |
| `profiles` | Legacy reads for current profile/avatar and dashboard mirror update | RLS/policy and source-of-truth clarity | Not fully covered | Reconcile legacy projection |
| `avatars` bucket | Storage upload and public URL reads | Storage policy and owner path checks | Public-read confirmed; write constrained by supplied evidence | Document public avatar ADR |
| `events.host_id` / `created_under_persona` | Event cards, event detail, dashboard host event fetch | Event RLS plus profile field contract | Events RLS confirmed; policy correctness incomplete | Document host identity projection |
| `host_identity_transfers` / audit log | Dashboard ops transfer views | Ops/admin RLS/RPC authority | Production table policy not fully covered here; admin RPC exists | Verify ops visibility separately |
| Public profile/relic/highlight RPCs | Public reads for relics/highlights/stats | RPC field contract | Production field policy incomplete | Verify public payload contract |
| Notification identity fields | Notification RPC payloads and UI hydration | Notification owner RLS/RPC authority | Notification table RLS confirmed; payload detail incomplete | Document payload identity contract |

## 21. Duplicated / Split / Legacy Identity Surfaces

| Surface / helper / RPC / table / bucket | Observed role | Current / legacy / unknown | Risk if still active or authoritative | Evidence type | Recommendation |
|---|---|---|---|---|---|
| `user_profiles` vs `profiles` | Canonical-looking profile table plus legacy mirror/projection | Split / legacy unclear | Stale display/avatar, inconsistent public identity, unclear RLS reliance | Local source + handbook | Reconcile or document source of truth |
| `avatar_url` vs `personal_avatar_url` vs `organizer_avatar_url` | Legacy primary avatar plus persona-specific avatars | Split but intentional-looking | Wrong persona avatar in public/event/notification surfaces | Local source | Document avatar precedence |
| Local active persona vs backend authority | UI-selected `personal` or `host` state | UI helper | Persona UI could be mistaken for capability authority | Local source | Keep backend capability authoritative |
| Public relics/highlights vs private media/profile | Public-safe profile artifacts | Current but field contract incomplete | Private media or internal source metadata exposure if RPC contract drifts | Handbook/local source | Verify public payload fields |
| Dashboard profile update mirror | Updates `user_profiles` and `profiles` display name | Current / legacy bridge | Partial updates across profile stores | Local source | Document or reconcile update ownership |
| Host identity transfer persona copy | Copies organizer identity and updates profile mirror | Current ops/admin surface | Public identity changes without accepted field semantics | Prior reports + local dashboard | Document transfer contract |
| Mobile, dashboard, and web profile readers | Multiple direct identity reads | Split-source UI usage | Field drift across clients | Local source | Centralize contract documentation |

## 22. Identity-Critical Invariants

- Public identity fields are explicitly approved and documented.
- Private account/profile fields are not exposed through public profile, event, media, notification, or venue surfaces.
- Host/organizer persona identity remains separate from personal identity unless product explicitly merges them.
- Avatar public storage does not imply all profile fields are public.
- Persona selection cannot bypass host ownership, tier, staff, or event authority.
- Event host identity, venue identity, media uploader identity, and notification actor identity follow accepted field contracts.
- Public highlights/relics do not leak internal source or uploader metadata.
- Tier/account tier display does not confer capability.
- Host identity transfer preserves personal identity and copies only accepted host persona fields.
- Ops/admin identity views do not become public identity surfaces.

## 23. Unknown / Needs Verification Surfaces

- Production `user_profiles` RLS policies and public field behavior.
- Production `profiles` RLS policies and whether `profiles` is still an accepted projection.
- Whether public profile visibility is fully backend-enforced for all personal/host tabs.
- Whether public profile, event share, venue share, and notification payloads share the same identity field contract.
- Whether all avatar writes are constrained to owner-controlled storage paths in production.
- Whether tier/account fields are public anywhere intentionally.
- Whether host identity transfer mirror updates are accepted product behavior.
- Whether public relic/highlight RPCs are production-active with the locally documented privacy shape.

## 24. Profile / Persona / Public Identity Gaps / Risk Register Seeds

| Gap ID | Domain | Current issue | Expected clean profile/persona/public identity contract | Risk | Priority candidate | Blocked by | Recommended next action |
|---|---|---|---|---|---|---|---|
| PPI-GAP-001 | Profile source of truth | `user_profiles` and `profiles` both appear in active reads/writes | One documented source of truth and projection contract | Privacy-sensitive / Product correctness | Candidate P1 | Production profile policy review and product decision | Document source-of-truth decision |
| PPI-GAP-002 | Public identity fields | Public profile/event/media/venue identity fields are not fully canonical | Approved public field list per surface | Privacy-sensitive | Candidate P1 | Product/security field contract | Create public identity field contract |
| PPI-GAP-003 | Avatar storage | `avatars` is public and avatar fields include legacy/full URLs | Public avatar semantics documented separately from profile table visibility | Privacy-sensitive | Candidate P2 | ADR/security decision | Record avatar ADR and storage policy verification |
| PPI-GAP-004 | Persona authority | Persona is stored locally and used broadly for rendering | Backend owns tier/capability/host authority; UI only mirrors | Security-sensitive / Product correctness | Candidate P1 | Backend authority mapping | Cross-reference authority boundary audit |
| PPI-GAP-005 | Host identity transfer | Transfer copy behavior affects organizer fields and profile mirror | Accepted transfer identity semantics and audit visibility | Operational/admin-sensitive | Candidate P1 / Unknown | Operator/product decision and ops policy review | Document transfer field effects |
| PPI-GAP-006 | Notification/media identity | Notifications and media hydrate actor/uploader identity across public/private contexts | Payload identity masking follows viewer authority | Privacy-sensitive | Candidate P2 | Notification/media policy review | Verify payload field contract |
| PPI-GAP-007 | Tier exposure | Tier is read by UI guards and may appear in profile/social rows | Capability is backend-authoritative and public tier display is explicit | Security-sensitive / Product correctness | Candidate P2 | Product decision on public tier badges | Define tier visibility contract |
| PPI-GAP-008 | Venue/business identity | Venue public identity may overlap owner/host identity | Venue/business identity contract separate from personal and host persona | Privacy-sensitive / Product correctness | Candidate P2 / Unknown | Venue/public profile decision | Document venue identity fields |

## 25. Product Decisions Required

- Which table is canonical for identity: `user_profiles`, `profiles`, or a documented projection pair?
- Which personal profile fields are public, authenticated-only, owner-only, or ops/admin-only?
- Which host/organizer persona fields are public on events, venues, host profiles, notifications, and public share pages?
- Is `avatar_url` a legacy fallback, canonical public avatar, or transitional projection?
- Are public tier badges intended, and if so for which tiers/surfaces?
- Is host identity transfer allowed to update the `profiles` mirror, or should it affect organizer fields only?
- What public relic/highlight identity fields are approved?
- What identity fields may notification payloads store as snapshots?

## 26. Recommended Next Audits

- Social Graph / Groups / Visibility Contract Audit.
- Staff / Host Operations Authority Audit.
- Search / Discovery / Feed Visibility Contract Audit.

## 27. Non-Goals

- No profile, persona, avatar, or transfer implementation changes.
- No Supabase migration drafting.
- No SQL creation.
- No production verification query bundle.
- No cleanup, deletion, move, or source-tree normalization.
- No acceptance of vulnerabilities, priorities, or remediation plans.
- No final claim that any profile exposure is exploitable.

## 28. Open Questions

- Are `profiles` and `user_profiles` both production-authoritative, or is one a compatibility projection?
- Should host persona be the default public identity for all `semi_pro` and `pro` users, or only for events/venues created under host persona?
- Can a personal profile be fully private while host persona remains public?
- Are public relics and host highlights intended to be visible to anonymous users or only authenticated viewers?
- Which identity fields are allowed in public event share payloads?
- Which identity fields are allowed in notification payload snapshots?
- Should host identity transfer change public profile search identity, or only host/organizer identity?

## 29. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `07_Audits/ProfilePersonaPublicIdentityContractAudit.md` was created/modified.

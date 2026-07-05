# Public Exposure Rules

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is the platform-level public exposure rules draft for JoinFolk.

This is a handbook draft. It is not a code audit and is not an accepted implementation contract. PublicExposureRules.md is the specific public/private/protected exposure document under the umbrella SecurityModel.md. SecurityModel.md is the umbrella security document. PermissionMatrix.md holds permission-axis structure. ThreatModel.md will hold threat-specific analysis.

This document does not define exact public/private/protected rules. All exact exposure values remain Unknown / Needs verification.

## 3. Public Exposure Rules Definition

Public Exposure Rules describe the axes that must be verified before JoinFolk can accept platform-level public, private, protected, or non-public exposure behavior.

Known facts:

- JoinFolk has public sharing concepts.
- JoinFolk may have Web/Public surfaces.
- JoinFolk has mobile surfaces.
- JoinFolk has dashboard surfaces.
- JoinFolk uses Supabase or Supabase-like backend concepts.
- JoinFolk has RLS, RPC, SECURITY DEFINER, storage policy, and auth concepts.
- Public exposure can be security-sensitive where private/protected/non-public data or media may be exposed.
- Backend/RPC/RLS/storage/auth must enforce public exposure rules where security-sensitive.
- Frontend public UI is UX only where security-sensitive.
- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact public exposure rules.
- Exact public/private/protected visibility model.
- Exact Web/Public behavior.
- Exact backend/RPC/RLS/storage/auth enforcement behavior.

## 4. Relationship to SecurityModel.md

SecurityModel.md is the umbrella security document.

PublicExposureRules.md focuses on public/private/protected exposure rules. It should not duplicate SecurityModel.md, PermissionMatrix.md, or ThreatModel.md in full.

Related documents:

- SecurityModel.md: umbrella security model.
- PermissionMatrix.md: permission-axis structure.
- ThreatModel.md: threat-specific analysis.

Unknown / needs verification:

- Exact boundary between PublicExposureRules.md, SecurityModel.md, PermissionMatrix.md, and ThreatModel.md.
- Which verified rules should live in each document.

## 5. Authority Model

### What frontend may own

Frontend surfaces may own user experience concerns, subject to backend/RPC/RLS/storage/auth enforcement for security-sensitive exposure behavior.

Frontend-owned behavior may include:

- Public UI presentation.
- Share UI presentation.
- Public event view layout.
- Public discovery layout.
- Loading, empty, and error states.
- Client-side display formatting.
- Non-authoritative display filtering.

Unknown / needs verification:

- Exact frontend route/component exposure behavior.
- Exact mobile, Dashboard/Ops, and Web/Public exposure UX.

### What backend/RPC/RLS/storage/auth must enforce

Backend/RPC/RLS/storage/auth must enforce public exposure rules where security-sensitive.

Security-sensitive enforcement includes:

- Public/private/protected visibility decisions.
- Public sharing visibility.
- Web/Public data exposure.
- Feed/discovery visibility.
- Event lifecycle exposure.
- Host identity exposure.
- Media/poster exposure.
- Messaging exposure where applicable.
- Storage object public access.
- Ops/admin moderation or override effects on public exposure.
- RPC and SECURITY DEFINER exposure boundaries.
- Production change approval for SQL, migrations, functions, RLS, storage, and auth.

Unknown / needs verification:

- Exact public exposure enforcement model.
- Exact RLS policies.
- Exact RPC contracts.
- Exact SECURITY DEFINER behavior.
- Exact storage policies.
- Exact auth behavior.

### What must never be frontend-only

The following must never rely only on frontend checks:

- Public/private/protected visibility decisions.
- Public sharing exposure.
- Web/Public data exposure.
- Feed/discovery visibility where private/protected/non-public data may be exposed.
- Media/poster exposure.
- Messaging exposure where applicable.
- Host identity exposure where security-sensitive.
- Ticketing/reservation public entry exposure.
- Storage object public access.
- Ops/admin moderation or override exposure effects.
- RPC, RLS, SECURITY DEFINER, storage, auth, or production change approval behavior.

## 6. Exposure Axes Draft

### Resource axis

Known resource concepts include events, feeds, public sharing, media/posters, venue/business data, profiles/avatars, host identity, ticketing, reservations, wallet/ownership, messaging, notifications, staff scanner/check-in, host identity transfer, ops/admin effects, storage objects, and RPC-backed data.

Unknown / needs verification:

- Exact resource model.
- Exact resource exposure states.

### Actor/viewer axis

Known actor/viewer concepts include viewer roles, personas, tiers, hosts, staff, ops/admin actors, authenticated users, and unauthenticated or public viewers.

Unknown / needs verification:

- Exact viewer model.
- Exact actor-specific exposure behavior.

### Visibility axis

Known exposure-related concepts include public, private, protected, and non-public visibility.

Unknown / needs verification:

- Exact visibility states.
- Exact meaning of public/private/protected/non-public.
- Exact transitions between visibility states.

### Surface axis

Known surfaces include mobile, Dashboard/Ops, Web/Public, and Supabase backend/storage.

Unknown / needs verification:

- Exact surface ownership.
- Exact surface exposure behavior.

### Context axis

Known context concepts include event lifecycle, viewer roles, personas, tiers, event ownership, host identity, feed visibility, public sharing, media visibility, messaging visibility, ticketing, reservations, staff scanner/check-in, venue/business tools, ops/admin moderation, and storage policy context.

Unknown / needs verification:

- Exact context model.
- Exact context-to-exposure relationships.

### Enforcement axis

Known enforcement concepts include auth, RLS, RPC, SECURITY DEFINER, storage policies, backend logic, and production change approval.

Unknown / needs verification:

- Exact enforcement model.
- Exact source of truth for public exposure.

## 7. Known Public Exposure Concepts Draft

Known exposure-related concepts:

- Public sharing.
- Web/Public surfaces.
- Public event views.
- Public discovery.
- Feed visibility.
- Event lifecycle visibility.
- Public/private/protected visibility.
- Host identity exposure.
- Media/poster exposure.
- Public profile/avatar or host identity media exposure.
- Public venue/business exposure.
- Public ticketing/reservation entry points where applicable.
- Messaging exposure where applicable.
- Ops/admin moderation or override effects on public exposure.

These names are known concepts only and must not be treated as accepted canonical exposure values, schema, RPC, RLS, storage, or UI contracts until verified.

Unknown / needs verification:

- Exact exposure vocabulary.
- Exact exposure values.
- Exact exposure behavior for each product domain.

## 8. Non-Accepted Public Exposure Areas

The following areas are not accepted yet:

- Exact public exposure rules.
- Exact public/private/protected visibility model.
- Exact public sharing behavior.
- Exact Web/Public behavior.
- Exact public event view behavior.
- Exact public discovery behavior.
- Exact feed visibility behavior.
- Exact event lifecycle exposure behavior.
- Exact host identity exposure behavior.
- Exact media/poster exposure behavior.
- Exact ticketing/reservation public entry behavior.
- Exact messaging public exposure behavior.
- Exact ops/admin moderation/override behavior.
- Exact RLS policies.
- Exact RPC contracts.
- Exact storage policies.
- Exact frontend route/component exposure behavior.
- Exact cross-surface exposure consistency.

These areas must remain Unknown / Needs verification until verified through accepted source material.

## 9. Event Lifecycle Exposure Draft

Known facts:

- JoinFolk has event lifecycle concepts.
- Public exposure may interact with event lifecycle visibility.

Unknown / needs verification:

- Exact event lifecycle exposure behavior.
- Exact lifecycle states that can be public, private, protected, or non-public.
- Whether lifecycle exposure differs across mobile, Dashboard/Ops, Web/Public, and backend.
- Which backend/RPC/RLS/storage/auth behavior enforces event lifecycle exposure.

No exact event lifecycle exposure behavior is accepted in this draft.

## 10. Feed / Discovery Exposure Draft

Known facts:

- JoinFolk has feed/discovery concepts.
- Feed visibility is security-sensitive where private/protected/non-public data may be exposed.
- Known exposure-related concepts include public discovery.

Unknown / needs verification:

- Exact feed visibility behavior.
- Exact public discovery behavior.
- Exact public/private/protected filtering behavior.
- Exact relationship between feed exposure, event lifecycle, host identity, media/posters, viewer roles, personas, tiers, public sharing, and ops/admin moderation.
- Which backend/RPC/RLS/storage/auth behavior enforces feed exposure.

No exact feed/discovery exposure behavior is accepted in this draft.

## 11. Web/Public Surface Exposure Draft

Known facts:

- JoinFolk may have Web/Public surfaces.
- Known exposure-related concepts include Web/Public surfaces and public event views.

Unknown / needs verification:

- Exact Web/Public behavior.
- Exact public event view behavior.
- Exact public discovery behavior.
- Exact public visibility rules for Web/Public surfaces.
- Whether Web/Public surfaces expose event data, host identity, media/posters, venue/business data, ticketing/reservation entry points, messaging entry points, notifications, or ops/admin effects.
- Which backend/RPC/RLS/storage/auth behavior enforces Web/Public exposure.

No exact Web/Public exposure behavior is accepted in this draft.

## 12. Public Sharing Draft

Known facts:

- JoinFolk has public sharing concepts.

Unknown / needs verification:

- Exact public sharing behavior.
- Exact public/private/protected sharing model.
- Whether public sharing applies to events, feeds, media/gallery, venue/business tools, ticketing, reservations, messaging, notifications, profile/avatar or host identity media, or other domains.
- Which backend/RPC/RLS/storage/auth behavior enforces public sharing exposure.

No exact public sharing behavior is accepted in this draft.

## 13. Host Identity / Profile Exposure Draft

Known facts:

- JoinFolk has event ownership and host concepts.
- JoinFolk has profile/avatar and host identity media concepts.
- Known exposure-related concepts include host identity exposure and public profile/avatar or host identity media exposure.

Unknown / needs verification:

- Exact host identity exposure behavior.
- Exact profile exposure behavior.
- Exact avatar or host identity media exposure behavior.
- Whether exposure depends on viewer roles, personas, tiers, event lifecycle, feed/discovery, public sharing, ops/admin moderation, or Web/Public surfaces.
- Which backend/RPC/RLS/storage/auth behavior enforces host identity/profile exposure.

No exact host identity or profile exposure behavior is accepted in this draft.

## 14. Media / Poster / Gallery Exposure Draft

Known facts:

- JoinFolk has media/gallery concepts.
- JoinFolk has event poster, venue media, profile/avatar, and host identity media concepts.
- Media visibility and upload behavior are security-sensitive where private/protected/non-public media may be exposed.
- Known exposure-related concepts include media/poster exposure.

Unknown / needs verification:

- Exact media/poster exposure behavior.
- Exact gallery exposure behavior.
- Exact event poster exposure behavior.
- Exact venue media exposure behavior.
- Exact profile/avatar or host identity media exposure behavior.
- Exact storage policy relationship.
- Which backend/RPC/RLS/storage/auth behavior enforces media exposure.

No exact media/poster/gallery exposure behavior is accepted in this draft.

## 15. Venue / Business Exposure Draft

Known facts:

- JoinFolk has venue/business tools.
- Known exposure-related concepts include public venue/business exposure.

Unknown / needs verification:

- Exact venue/business exposure behavior.
- Whether venue/business data, venue media, venue reservations, layout information, ticketing entry points, or public sharing data can be exposed.
- Whether exposure depends on viewer roles, personas, tiers, event lifecycle, feed/discovery, public sharing, or ops/admin moderation.
- Which backend/RPC/RLS/storage/auth behavior enforces venue/business exposure.

No exact venue/business exposure behavior is accepted in this draft.

## 16. Ticketing / Reservation Public Entry Draft

Known facts:

- JoinFolk has ticketing.
- JoinFolk has reservations.
- Known exposure-related concepts include public ticketing/reservation entry points where applicable.

Unknown / needs verification:

- Exact ticketing/reservation public entry behavior.
- Whether ticketing or reservation entry points appear on public event views, public discovery, Web/Public surfaces, mobile surfaces, or public sharing surfaces.
- Whether public entry depends on event lifecycle, viewer roles, personas, tiers, host identity, venue/business tools, wallet/ownership, ops/admin moderation, or public sharing.
- Which backend/RPC/RLS/storage/auth behavior enforces ticketing/reservation public entry exposure.

No exact ticketing/reservation public entry behavior is accepted in this draft.

## 17. Messaging Exposure Draft

Known facts:

- JoinFolk has messaging concepts or possible messaging concepts.
- Messaging is security-sensitive where private conversation, participant-only communication, staff/host communication, or operational messages may be exposed.
- Known exposure-related concepts include messaging exposure where applicable.

Unknown / needs verification:

- Whether messaging has any public exposure behavior.
- Exact messaging public exposure behavior.
- Exact private conversation, participant-only, staff/host, or operational message exposure boundaries.
- Whether messaging entry points appear on public surfaces.
- Which backend/RPC/RLS/storage/auth behavior enforces messaging exposure.

No exact messaging public exposure behavior is accepted in this draft.

## 18. Notifications Exposure Draft

Known facts:

- JoinFolk has notifications.

Unknown / needs verification:

- Exact notification exposure behavior.
- Whether notifications or notification-derived data can appear on public surfaces.
- Whether notifications expose event, host, ticketing, reservation, media, messaging, staff scanner/check-in, venue/business, or ops/admin state.
- Which backend/RPC/RLS/storage/auth behavior enforces notification exposure.

No exact notification exposure behavior is accepted in this draft.

## 19. Staff Scanner / Check-in Exposure Draft

Known facts:

- JoinFolk has staff scanner/check-in.

Unknown / needs verification:

- Exact staff scanner/check-in exposure behavior.
- Whether queue, check-in, checked-in, staff, scanner, or operational data can be exposed publicly.
- Whether exposure depends on viewer roles, personas, tiers, ticketing, reservations, event lifecycle, host identity, venue/business tools, ops/admin, or public sharing.
- Which backend/RPC/RLS/storage/auth behavior enforces staff scanner/check-in exposure.

No exact staff scanner/check-in exposure behavior is accepted in this draft.

## 20. Host Identity Transfer Exposure Draft

Known facts:

- JoinFolk has host identity transfer.

Unknown / needs verification:

- Exact host identity transfer exposure behavior.
- Whether transfer state, transferred identity, profile/avatar, host identity media, event ownership, venue/business authority, notifications, audit data, or public display changes are exposed.
- Which backend/RPC/RLS/storage/auth behavior enforces host identity transfer exposure.

No exact host identity transfer exposure behavior is accepted in this draft.

## 21. Ops/Admin Moderation / Override Exposure Draft

Known facts:

- JoinFolk has ops/admin concepts.
- Known exposure-related concepts include ops/admin moderation or override effects on public exposure.

Unknown / needs verification:

- Exact ops/admin moderation/override behavior.
- Whether ops/admin can affect public exposure for events, feeds, media, venue/business tools, ticketing/reservations, messaging, notifications, staff scanner/check-in, host identity transfer, or public sharing.
- Whether ops/admin state or audit history is exposed publicly.
- Which backend/RPC/RLS/storage/auth behavior enforces ops/admin exposure effects.

No exact ops/admin moderation or override exposure behavior is accepted in this draft.

## 22. Storage / RPC / RLS Exposure Draft

Known facts:

- JoinFolk has RLS, RPC, SECURITY DEFINER, storage policy, and auth concepts.
- Backend/RPC/RLS/storage/auth must enforce public exposure rules where security-sensitive.
- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact RLS policies.
- Exact RPC contracts.
- Exact SECURITY DEFINER behavior.
- Exact storage policies.
- Exact auth behavior.
- Exact production change enforcement.
- Exact relationship between RLS, RPC, SECURITY DEFINER, storage, auth, and public exposure.

No exact RLS, RPC, SECURITY DEFINER, storage policy, auth, or production change exposure behavior is accepted in this draft.

## 23. Draft Exposure Matrix Template

This is a template only. Do not treat any cell as an accepted public/private/protected value.

| Resource domain | Actor/viewer concept | Visibility concept | Surface | Context | Enforcement | Exposure value |
| --- | --- | --- | --- | --- | --- | --- |
| Event lifecycle | Unknown / Needs verification | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Unknown / Needs verification | Backend/RPC/RLS/auth | Unknown / Needs verification |
| Feed/discovery | Unknown / Needs verification | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Unknown / Needs verification | Backend/RPC/RLS/auth | Unknown / Needs verification |
| Public sharing | Unknown / Needs verification | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Unknown / Needs verification | Backend/RPC/RLS/storage/auth | Unknown / Needs verification |
| Host identity/profile | Unknown / Needs verification | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Unknown / Needs verification | Backend/RPC/RLS/storage/auth | Unknown / Needs verification |
| Media/poster/gallery | Unknown / Needs verification | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Unknown / Needs verification | Backend/RPC/RLS/storage/auth | Unknown / Needs verification |
| Venue/business | Unknown / Needs verification | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Unknown / Needs verification | Backend/RPC/RLS/storage/auth | Unknown / Needs verification |
| Ticketing/reservation entry | Unknown / Needs verification | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Unknown / Needs verification | Backend/RPC/RLS/auth | Unknown / Needs verification |
| Messaging | Unknown / Needs verification | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Unknown / Needs verification | Backend/RPC/RLS/auth | Unknown / Needs verification |
| Notifications | Unknown / Needs verification | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Unknown / Needs verification | Backend/RPC/RLS/auth | Unknown / Needs verification |
| Staff scanner/check-in | Unknown / Needs verification | Unknown / Needs verification | Mobile / Dashboard/Ops / Web/Public / Backend | Unknown / Needs verification | Backend/RPC/RLS/auth | Unknown / Needs verification |
| Host identity transfer | Unknown / Needs verification | Unknown / Needs verification | Dashboard/Ops / Web/Public / Backend | Unknown / Needs verification | Backend/RPC/RLS/auth | Unknown / Needs verification |
| Ops/admin effects | Unknown / Needs verification | Unknown / Needs verification | Dashboard/Ops / Web/Public / Backend | Unknown / Needs verification | Backend/RPC/RLS/auth | Unknown / Needs verification |
| Storage / RPC / RLS | Unknown / Needs verification | Unknown / Needs verification | Backend / Web/Public where applicable | Unknown / Needs verification | Backend/RPC/RLS/storage/auth | Unknown / Needs verification |

## 24. Cross-Surface Consistency Requirements

### Mobile

Unknown / needs verification:

- Exact mobile exposure behavior.
- Which exposure decisions mobile displays but does not enforce.
- Which mobile behavior must match Dashboard/Ops, Web/Public, and Supabase backend/storage.

### Dashboard/Ops

Unknown / needs verification:

- Exact Dashboard/Ops exposure behavior.
- Exact admin route/component exposure behavior.
- Which Dashboard/Ops behavior must match mobile, Web/Public, and Supabase backend/storage.

### Web/Public

Unknown / needs verification:

- Exact Web/Public exposure behavior.
- Exact public event view behavior.
- Exact public discovery behavior.
- Exact public/private/protected visibility behavior.
- Which Web/Public behavior must match mobile, Dashboard/Ops, and Supabase backend/storage.

### Supabase Backend / Storage

Unknown / needs verification:

- Exact backend/storage exposure behavior.
- Exact auth, RLS, RPC, SECURITY DEFINER, storage, and production change enforcement.
- Which backend/storage behavior is authoritative for each exposure domain.

## 25. Security Risks

Security risks to verify:

- Frontend-only checks being treated as exposure enforcement.
- Exact public/private/protected rules inferred without verification.
- Web/Public surfaces exposing private/protected/non-public data.
- Feed/discovery exposing private/protected/non-public events or data.
- Media/posters/storage objects exposed without accepted visibility rules.
- Messaging exposure leaking private conversation, participant-only, staff/host, or operational messages.
- RLS, RPC, SECURITY DEFINER, storage, or auth behavior assumed without accepted contracts.
- Ops/admin moderation or override effects not enforced by backend/RPC/RLS/storage/auth.
- Production SQL, migrations, functions, RLS, storage, or auth changes without explicit approval.

## 26. Privacy Risks

Privacy risks to verify:

- Private/protected/non-public user, profile, event, media, feed, messaging, ticketing, reservation, wallet/ownership, venue/business, or public sharing data exposed to unauthorized viewers.
- Host identity or profile/avatar media exposed beyond accepted visibility.
- Public ticketing or reservation entry points exposing protected state.
- Notifications exposing private state.
- Staff scanner/check-in data exposed publicly.
- Ops/admin or audit effects exposed without accepted rules.

## 27. Determinism Risks

Determinism risks to verify:

- Exposure decisions interpreted differently across mobile, Dashboard/Ops, Web/Public, and Supabase backend/storage.
- Public/private/protected visibility states mapped differently across product domains.
- Frontend display state diverging from backend exposure enforcement.
- Storage access diverging from media visibility rules.
- Public sharing diverging from feed, media, Web/Public, or backend authority.
- Ops/admin moderation or override effects producing inconsistent exposure state.

## 28. Maintainability Risks

Maintainability risks to verify:

- Exposure logic scattered across frontend, backend, RPCs, RLS, storage policies, and product modules without clear ownership.
- PublicExposureRules.md duplicating or contradicting SecurityModel.md, PermissionMatrix.md, or ThreatModel.md.
- Exact exposure values added without verification.
- Role, permission, exposure state, schema, RPC, RLS policy, route, component, bucket, or storage policy names treated as canonical before verification.

## 29. Current Known Implementation

Current accepted implementation knowledge is limited to the facts in this draft:

- JoinFolk has public sharing concepts.
- JoinFolk may have Web/Public surfaces.
- JoinFolk has mobile and dashboard surfaces.
- JoinFolk uses Supabase or Supabase-like backend concepts.
- JoinFolk has RLS, RPC, SECURITY DEFINER, storage policy, and auth concepts.
- JoinFolk has event lifecycle, viewer roles, personas and tiers, event ownership and host concepts, feed/discovery, media/gallery, ticketing, reservations, wallet/ownership, notifications, staff scanner/check-in, venue/business tools, messaging concepts or possible messaging concepts, host identity transfer, and ops/admin concepts.
- Public exposure can be security-sensitive where private/protected/non-public data or media may be exposed.
- Backend/RPC/RLS/storage/auth must enforce public exposure rules where security-sensitive.
- Frontend public UI is UX only where security-sensitive.
- Production SQL, migrations, functions, RLS, storage, and auth changes must not happen without explicit approval.

Unknown / needs verification:

- Exact accepted public exposure implementation across resources, actors/viewers, visibility concepts, surfaces, contexts, and enforcement layers.

## 30. Unknowns / Needs Verification

The following must be verified before v1.0:

- Exact public exposure rules.
- Exact public/private/protected visibility model.
- Exact public sharing behavior.
- Exact Web/Public behavior.
- Exact public event view behavior.
- Exact public discovery behavior.
- Exact feed visibility behavior.
- Exact event lifecycle exposure behavior.
- Exact host identity exposure behavior.
- Exact media/poster exposure behavior.
- Exact ticketing/reservation public entry behavior.
- Exact messaging public exposure behavior.
- Exact notifications exposure behavior.
- Exact staff scanner/check-in exposure behavior.
- Exact host identity transfer exposure behavior.
- Exact ops/admin moderation/override behavior.
- Exact RLS policies.
- Exact RPC contracts.
- Exact SECURITY DEFINER behavior.
- Exact storage policies.
- Exact frontend route/component exposure behavior.
- Exact cross-surface exposure consistency.
- Exact relationship to SecurityModel.md, PermissionMatrix.md, and ThreatModel.md.

## 31. Acceptance Criteria for v1.0

Public Exposure Rules v1.0 can be accepted only after verification establishes:

- Accepted public exposure domain vocabulary.
- Accepted resource exposure model.
- Accepted actor/viewer exposure model.
- Accepted visibility model.
- Accepted surface exposure model.
- Accepted context model.
- Accepted enforcement model.
- Accepted public sharing behavior.
- Accepted Web/Public behavior.
- Accepted public event view behavior.
- Accepted public discovery behavior.
- Accepted feed visibility behavior.
- Accepted event lifecycle exposure behavior.
- Accepted host identity/profile exposure behavior.
- Accepted media/poster/gallery exposure behavior.
- Accepted venue/business exposure behavior.
- Accepted ticketing/reservation public entry behavior.
- Accepted messaging exposure behavior where applicable.
- Accepted notifications exposure behavior.
- Accepted staff scanner/check-in exposure behavior.
- Accepted host identity transfer exposure behavior.
- Accepted ops/admin moderation/override exposure behavior.
- Accepted RLS policies.
- Accepted RPC contracts and SECURITY DEFINER boundaries.
- Accepted storage policies.
- Accepted cross-surface exposure consistency.
- Accepted relationship to SecurityModel.md, PermissionMatrix.md, and ThreatModel.md.

Until these criteria are met, this document remains non-canonical.

## 32. Open Questions

- What are the accepted public exposure rules?
- What public/private/protected visibility model is accepted?
- What public sharing behavior is accepted?
- What Web/Public behavior is accepted?
- What public event view behavior is accepted?
- What public discovery behavior is accepted?
- What feed visibility behavior is accepted?
- What event lifecycle exposure behavior is accepted?
- What host identity/profile exposure behavior is accepted?
- What media/poster/gallery exposure behavior is accepted?
- What venue/business exposure behavior is accepted?
- What ticketing or reservation public entry behavior is accepted?
- What messaging public exposure behavior is accepted, if any?
- What notification exposure behavior is accepted?
- What staff scanner/check-in exposure behavior is accepted?
- What host identity transfer exposure behavior is accepted?
- What ops/admin moderation or override exposure behavior is accepted?
- What RLS policies, RPC contracts, SECURITY DEFINER boundaries, and storage policies enforce public exposure?
- What frontend route/component exposure behavior is accepted?
- How should PublicExposureRules.md relate to SecurityModel.md, PermissionMatrix.md, and ThreatModel.md?
- Which surfaces support public exposure behavior today: mobile, Dashboard/Ops, Web/Public, and Supabase backend/storage?

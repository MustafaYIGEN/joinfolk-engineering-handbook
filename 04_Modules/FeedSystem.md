# Feed System

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is a platform-level draft specification for JoinFolk feed/discovery behavior.

This is a handbook draft. It is not a code audit and is not an accepted implementation contract. Feed/discovery behavior can affect what users see, what events are discoverable, public visibility, host visibility, and marketplace trust.

Feed visibility is security-sensitive where private, protected, or non-public data may be exposed. Backend/RPC/RLS must enforce security-sensitive feed visibility behavior. Frontend feed behavior is UX only where security-sensitive.

Prior implementation notes must not be treated as canonical.

## 3. Feed System Definition

JoinFolk has feed/discovery concepts.

Known facts:

- Prior project context mentioned Home, Discover, and Rising feed concepts.
- Prior project context mentioned feed-visible event lifecycle states such as published and live.
- Prior project context mentioned city/system feed items where the source may not be a user profile.
- Prior project context mentioned normal event feed items with host identity/avatar behavior.
- Mobile may expose feed/discovery behavior.
- Web/Public may expose public discovery or public event views.
- Dashboard may influence feed visibility where admin/host tooling is involved.

Unknown / needs verification:

- Exact feed schema.
- Exact feed RPC contracts.
- Exact feed RLS policies.
- Exact ranking, ordering, personalization, and visibility behavior.
- Exact mobile, Web/Public, and Dashboard/Ops ownership.

## 4. Authority Model

### What frontend may own

Frontend surfaces may own user experience concerns, subject to backend/RPC/RLS enforcement for security-sensitive behavior.

Frontend-owned behavior may include:

- Feed presentation.
- Feed tab or surface navigation.
- Card layout.
- Loading, empty, and error states.
- Client-side display formatting.
- Local filtering or grouping for usability, where not security-sensitive.

Unknown / needs verification:

- Exact mobile feed behavior.
- Exact web/public feed behavior.
- Exact dashboard route/component/service ownership.
- Exact frontend surface ownership for Home, Discover, Rising, and public discovery views.

### What backend/RPC/RLS must enforce

Backend, RPC, and RLS must enforce security-sensitive feed visibility behavior.

Security-sensitive behavior includes:

- Public/private visibility filtering.
- Event lifecycle filtering where it affects discoverability or protected data exposure.
- Viewer-role visibility rules.
- Persona/tier visibility rules.
- Host identity exposure.
- Public sharing visibility.
- Feed inclusion/exclusion for private, protected, non-public, moderated, or admin-affected content.
- Any behavior that affects ticketing, reservations, venue/business tools, media/gallery, notifications, moderation/ops/admin, mobile surfaces, or Web/Public surfaces where security-sensitive.

Unknown / needs verification:

- Exact feed RLS policies.
- Exact feed RPC authorization behavior.
- Exact visibility model.
- Exact public/private filtering behavior.
- Exact moderation/ops override behavior.

### What must never be frontend-only

The following must never rely only on frontend checks:

- Feed visibility for private, protected, or non-public data.
- Public/private filtering.
- Event lifecycle filtering where security-sensitive.
- Viewer-role visibility.
- Persona/tier visibility.
- Host identity exposure where security-sensitive.
- Public sharing visibility.
- Moderation or ops/admin exclusion/inclusion behavior.
- Any feed behavior that exposes protected event, host, ticketing, reservation, venue/business, media/gallery, or ownership-related data.

## 5. Known Feed Concepts Draft

Known feed/discovery concepts:

- Home feed.
- Discover feed.
- Rising feed.
- Feed-visible event lifecycle states.
- Published and live lifecycle concepts.
- City/system feed items.
- Normal event feed items.
- Host identity/avatar behavior.
- Public discovery.
- Public event views.
- Location/city.
- Media/posters.
- Public sharing.

Known related product areas:

- Event lifecycle.
- Event visibility.
- Viewer roles.
- Personas and tiers.
- Host identity.
- Public sharing.
- Ticketing.
- Reservations.
- Venue/business tools.
- Media/gallery.
- Notifications where applicable.
- Moderation/ops/admin.
- Web/Public surfaces.
- Mobile surfaces.

Unknown / needs verification:

- Exact feed domain vocabulary.
- Which concepts exist as persisted backend entities.
- Which concepts exist only as frontend UX.
- Which concepts are authoritative in backend/RPC/RLS.

## 6. Known Schema / Constant / RPC Concept Names Draft

Prior project context mentioned concepts or constant-like names such as:

- `Home`
- `Discover`
- `Rising`
- `FEED_VISIBLE`
- `published`
- `live`
- `city/system`
- `host_id`
- `created_under_persona`

These names are known concepts only and must not be treated as accepted canonical constants, schema, or contracts until verified.

Unknown / needs verification:

- Whether these names exist in the accepted implementation.
- Whether these names are current.
- Whether any feed RPC names exist.
- Exact feed schema names.
- Exact constant names.
- Exact RPC parameters.
- Exact RPC return shapes.
- Exact authorization behavior.
- Exact RLS behavior.

## 7. Non-Accepted Feed Areas

The following areas are not accepted yet:

- Exact feed schema.
- Exact feed RPC contracts.
- Exact feed RLS policies.
- Exact ranking behavior.
- Exact personalization behavior.
- Exact feed visibility model.
- Exact public/private filtering behavior.
- Exact event lifecycle filtering behavior.
- Exact city/system item behavior.
- Exact host identity/avatar behavior in feeds.
- Exact media/poster selection behavior.
- Exact moderation/ops override behavior.
- Exact dashboard route/component/service ownership.
- Exact mobile feed behavior.
- Exact web/public feed behavior.

These areas must remain Unknown / Needs verification until verified through accepted source material.

## 8. Feed Surfaces Draft

### Home

Known concept:

- Prior project context mentioned Home feed concepts.

Unknown / needs verification:

- Exact Home feed behavior.
- Exact Home feed visibility, ranking, ordering, personalization, and filtering behavior.
- Whether Home is mobile-only, web/public, shared, or another surface.

No exact Home feed behavior is accepted in this draft.

### Discover

Known concept:

- Prior project context mentioned Discover feed concepts.

Unknown / needs verification:

- Exact Discover feed behavior.
- Exact Discover visibility, ranking, ordering, personalization, and filtering behavior.
- Whether Discover is mobile-only, web/public, shared, or another surface.

No exact Discover feed behavior is accepted in this draft.

### Rising

Known concept:

- Prior project context mentioned Rising feed concepts.

Unknown / needs verification:

- Exact Rising feed behavior.
- Exact Rising visibility, ranking, ordering, personalization, and filtering behavior.
- Whether Rising is mobile-only, web/public, shared, or another surface.

No exact Rising feed behavior is accepted in this draft.

### Web/Public where applicable

Known facts:

- Web/Public may expose public discovery or public event views.
- Feed behavior may interact with public sharing.

Unknown / needs verification:

- Exact Web/Public feed behavior.
- Exact public discovery behavior.
- Exact public event view relationship.
- Exact public visibility rules.

No exact Web/Public feed behavior is accepted in this draft.

## 9. Event Lifecycle / Visibility Draft

Known facts:

- Prior project context mentioned feed-visible event lifecycle states such as published and live.
- Feed behavior may depend on event status and public/private visibility.
- Feed behavior may interact with event lifecycle and event visibility.

Unknown / needs verification:

- Exact event lifecycle filtering behavior.
- Exact feed-visible lifecycle states.
- Exact meaning of published and live in feed context.
- Exact public/private filtering behavior.
- Whether event lifecycle filtering differs by Home, Discover, Rising, mobile, Web/Public, Dashboard/Ops, or backend.
- Which backend/RPC/RLS behavior enforces event lifecycle and visibility filtering.

No exact event lifecycle or visibility filtering behavior is accepted in this draft.

## 10. Viewer Role / Persona / Tier Draft

Known facts:

- Feed behavior may interact with viewer roles.
- Feed behavior may interact with personas and tiers.

Unknown / needs verification:

- Exact viewer-role feed behavior.
- Exact persona/tier feed behavior.
- Whether viewer roles, personas, or tiers affect feed inclusion, ranking, personalization, host identity exposure, public sharing, ticketing, reservations, venue/business tools, media/gallery, or notifications.
- Which backend/RPC/RLS behavior enforces viewer-role, persona, and tier visibility.

No exact viewer role, persona, or tier feed behavior is accepted in this draft.

## 11. Host Identity / Avatar Draft

Known facts:

- Prior project context mentioned normal event feed items with host identity/avatar behavior.
- Feed behavior may depend on host identity.
- Prior project context mentioned `host_id` and `created_under_persona` as known concepts.

Unknown / needs verification:

- Exact host identity behavior in feeds.
- Exact avatar behavior in feeds.
- Whether host identity/avatar behavior differs for normal event feed items and city/system feed items.
- Whether host identity display depends on personas, tiers, viewer roles, public sharing, event lifecycle, or Web/Public exposure.
- Which backend/RPC/RLS behavior enforces host identity/avatar visibility where security-sensitive.

No exact host identity or avatar behavior is accepted in this draft.

## 12. City/System Item Draft

Known facts:

- Prior project context mentioned city/system feed items where the source may not be a user profile.
- Feed behavior may depend on location/city.

Unknown / needs verification:

- Exact city/system item behavior.
- Exact source model for city/system feed items.
- Whether city/system feed items have host identity/avatar behavior.
- Whether city/system feed items interact with event lifecycle, public sharing, media/posters, ticketing, reservations, venue/business tools, moderation/ops/admin, mobile, or Web/Public surfaces.
- Which backend/RPC/RLS behavior enforces city/system visibility where security-sensitive.

No exact city/system item behavior is accepted in this draft.

## 13. Media / Poster / Gallery Relationship Draft

Known facts:

- Feed behavior may depend on media/posters.
- Feed behavior may interact with media/gallery.

Unknown / needs verification:

- Exact media/poster selection behavior.
- Whether feeds use event posters, venue media, gallery media, host avatars, city/system imagery, or other media.
- Whether media/poster selection differs across Home, Discover, Rising, mobile, Web/Public, and Dashboard/Ops.
- Whether media visibility depends on public/private rules, event lifecycle, viewer roles, personas and tiers, public sharing, or moderation/ops/admin.
- Which backend/RPC/RLS behavior enforces media/poster visibility where security-sensitive.

No exact media, poster, or gallery behavior is accepted in this draft.

## 14. Ranking / Ordering / Personalization Draft

Known facts:

- Exact ranking behavior is not accepted yet.
- Exact personalization behavior is not accepted yet.

Unknown / needs verification:

- Exact ranking behavior.
- Exact ordering behavior.
- Exact personalization behavior.
- Whether ranking or ordering differs by Home, Discover, Rising, mobile, Web/Public, Dashboard/Ops, or backend.
- Whether ranking or personalization depends on event lifecycle, visibility, viewer roles, personas and tiers, host identity, location/city, ticketing, reservations, venue/business tools, media/gallery, notifications, moderation/ops/admin, or public sharing.
- Which backend/RPC/RLS behavior enforces ranking inputs where security-sensitive.

No exact ranking, ordering, or personalization behavior is accepted in this draft.

## 15. Public Sharing Draft

Known facts:

- Feed behavior may interact with public sharing.
- Web/Public may expose public discovery or public event views.
- Feed visibility is security-sensitive where private/protected/non-public data may be exposed.

Unknown / needs verification:

- Exact public sharing feed behavior.
- Exact public discovery behavior.
- Exact public/private filtering behavior.
- Whether public sharing affects feed inclusion, host identity/avatar behavior, media/posters, ticketing, reservations, venue/business tools, or Web/Public event views.
- Which backend/RPC/RLS behavior enforces public sharing visibility.

No exact public sharing feed behavior is accepted in this draft.

## 16. Moderation / Ops/Admin Draft

Known facts:

- Feed behavior may interact with moderation/ops/admin.
- Dashboard may influence feed visibility where admin/host tooling is involved.
- Exact moderation/ops override behavior is not accepted yet.

Unknown / needs verification:

- Exact moderation behavior.
- Exact ops/admin override behavior.
- Whether moderation or ops/admin can affect feed inclusion, exclusion, ranking, public sharing, host visibility, media/poster selection, event lifecycle, or public/private visibility.
- Which Dashboard/Ops tools influence feed visibility.
- Which backend/RPC/RLS behavior enforces moderation/ops/admin feed authority.

No exact moderation or ops/admin feed behavior is accepted in this draft.

## 17. Relationship to Product Domains

### Event lifecycle

Known relationship:

- Feed behavior may interact with event lifecycle.
- Prior project context mentioned feed-visible event lifecycle states such as published and live.

Unknown / needs verification:

- Exact event lifecycle relationship.

### Viewer roles

Known relationship:

- Feed behavior may interact with viewer roles.

Unknown / needs verification:

- Exact viewer-role relationship.

### Personas and tiers

Known relationship:

- Feed behavior may interact with personas and tiers.

Unknown / needs verification:

- Exact persona/tier relationship.

### Host identity

Known relationship:

- Feed behavior may interact with host identity.
- Prior project context mentioned normal event feed items with host identity/avatar behavior.

Unknown / needs verification:

- Exact host identity and avatar relationship.

### Public sharing

Known relationship:

- Feed behavior may interact with public sharing.

Unknown / needs verification:

- Exact public sharing relationship.

### Ticketing

Known relationship:

- Feed behavior may interact with ticketing.

Unknown / needs verification:

- Exact ticketing relationship.

### Reservations

Known relationship:

- Feed behavior may interact with reservations.

Unknown / needs verification:

- Exact reservation relationship.

### Venue/business tools

Known relationship:

- Feed behavior may interact with venue/business tools.

Unknown / needs verification:

- Exact venue/business relationship.

### Media/gallery

Known relationship:

- Feed behavior may interact with media/gallery.

Unknown / needs verification:

- Exact media/gallery relationship.

### Notifications

Known relationship:

- Feed behavior may interact with notifications where applicable.

Unknown / needs verification:

- Whether feed/discovery behavior triggers, consumes, or displays notifications.

### Ops/admin

Known relationship:

- Feed behavior may interact with moderation/ops/admin.
- Dashboard may influence feed visibility where admin/host tooling is involved.

Unknown / needs verification:

- Exact ops/admin relationship.

## 18. Cross-Surface Consistency Requirements

### Mobile

Known facts:

- Mobile may expose feed/discovery behavior.
- Exact mobile feed behavior is not accepted yet.

Unknown / needs verification:

- Exact mobile feed behavior.
- Whether mobile exposes Home, Discover, Rising, public discovery, or other feed surfaces.
- Which mobile behavior must match Web/Public, Dashboard/Ops, or backend behavior.

### Web/Public

Known facts:

- Web/Public may expose public discovery or public event views.
- Exact web/public feed behavior is not accepted yet.

Unknown / needs verification:

- Exact Web/Public feed behavior.
- Exact public discovery and public event view behavior.
- Exact public visibility rules.
- Which Web/Public behavior must match mobile, Dashboard/Ops, or backend behavior.

### Dashboard/Ops

Known facts:

- Dashboard may influence feed visibility where admin/host tooling is involved.
- Exact dashboard route/component/service ownership is not accepted yet.

Unknown / needs verification:

- Exact Dashboard/Ops ownership for feed visibility tooling.
- Exact host/admin tools that influence feed visibility.
- Exact moderation/ops override behavior.
- Which Dashboard/Ops behavior must match mobile, Web/Public, or backend behavior.

### Supabase Backend

Known requirement:

- Backend/RPC/RLS must enforce security-sensitive feed visibility behavior.

Unknown / needs verification:

- Exact feed schema.
- Exact feed RPC contracts.
- Exact feed RLS policies.
- Exact backend ownership boundaries.
- Exact enforcement model for visibility, lifecycle filtering, public/private filtering, viewer roles, personas and tiers, host identity, public sharing, media/posters, and moderation/ops/admin behavior.

## 19. Security Risks

Known risks:

- Feed/discovery behavior can affect what users see, what events are discoverable, public visibility, host visibility, and marketplace trust.
- Feed visibility is security-sensitive where private/protected/non-public data may be exposed.
- Backend/RPC/RLS must enforce security-sensitive feed visibility behavior.
- Frontend feed behavior is UX only where security-sensitive.

Security risks to verify:

- Private, protected, or non-public event exposure.
- Incorrect public/private filtering.
- Incorrect event lifecycle filtering.
- Unauthorized host identity/avatar exposure.
- Unauthorized media/poster exposure.
- Viewer-role, persona, or tier visibility bypass.
- Public sharing visibility mismatch.
- Moderation/ops/admin overrides not enforced by backend/RPC/RLS.
- Frontend-only checks being treated as enforcement.

## 20. Determinism Risks

Known determinism risks:

- Exact ranking behavior is not accepted yet.
- Exact personalization behavior is not accepted yet.
- Exact feed visibility model is not accepted yet.
- Exact event lifecycle filtering behavior is not accepted yet.
- Exact host identity/avatar behavior is not accepted yet.

Risks to verify:

- Feed inclusion differing across mobile, Web/Public, Dashboard/Ops, and backend.
- Lifecycle state filtering producing inconsistent feed visibility.
- Public/private filtering producing inconsistent exposure.
- Host identity/avatar display differing across feed surfaces.
- City/system items being handled inconsistently.
- Media/poster selection differing across surfaces.
- Ranking, ordering, or personalization producing inconsistent or unexplainable results.
- Moderation/ops/admin feed effects diverging from backend authority.

## 21. Maintainability Risks

Known maintainability risks:

- Exact feed schema is not accepted yet.
- Exact feed RPC contracts are not accepted yet.
- Exact feed RLS policies are not accepted yet.
- Exact dashboard route/component/service ownership is not accepted yet.
- Prior schema, constant, RPC, route, and component names are known concepts only, not accepted contracts.

Risks to verify:

- Prior implementation names being treated as canonical before verification.
- Feed behavior scattered across mobile, Web/Public, Dashboard/Ops, and backend without clear ownership.
- Frontend code encoding security-sensitive visibility rules.
- Ranking, personalization, visibility, host identity, media/poster, public sharing, and moderation behavior duplicated without accepted ownership documentation.

## 22. Current Known Implementation

Current accepted implementation knowledge is limited to the facts in this draft:

- JoinFolk has feed/discovery concepts.
- Prior project context mentioned Home, Discover, and Rising feed concepts.
- Prior project context mentioned feed-visible event lifecycle states such as published and live.
- Prior project context mentioned city/system feed items where the source may not be a user profile.
- Prior project context mentioned normal event feed items with host identity/avatar behavior.
- Mobile may expose feed/discovery behavior.
- Web/Public may expose public discovery or public event views.
- Dashboard may influence feed visibility where admin/host tooling is involved.
- Prior context mentioned concept or constant-like names, but none are accepted canonical constants, schema, or contracts.

Unknown / needs verification:

- Exact accepted implementation across mobile, Web/Public, Dashboard/Ops, Supabase backend, event lifecycle, event visibility, viewer roles, personas and tiers, host identity, public sharing, ticketing, reservations, venue/business tools, media/gallery, notifications, and moderation/ops/admin.

## 23. Unknowns / Needs Verification

The following must be verified before v1.0:

- Exact feed schema.
- Exact feed RPC contracts.
- Exact feed RLS policies.
- Exact ranking behavior.
- Exact ordering behavior.
- Exact personalization behavior.
- Exact feed visibility model.
- Exact public/private filtering behavior.
- Exact event lifecycle filtering behavior.
- Exact city/system item behavior.
- Exact host identity/avatar behavior in feeds.
- Exact media/poster selection behavior.
- Exact moderation/ops override behavior.
- Exact dashboard route/component/service ownership.
- Exact mobile feed behavior.
- Exact web/public feed behavior.
- Exact relationship between feed behavior and event lifecycle.
- Exact relationship between feed behavior and viewer roles.
- Exact relationship between feed behavior and personas/tiers.
- Exact relationship between feed behavior and host identity.
- Exact relationship between feed behavior and public sharing.
- Exact relationship between feed behavior and ticketing.
- Exact relationship between feed behavior and reservations.
- Exact relationship between feed behavior and venue/business tools.
- Exact relationship between feed behavior and media/gallery.
- Exact relationship between feed behavior and notifications where applicable.
- Exact relationship between feed behavior and ops/admin.

## 24. Acceptance Criteria for v1.0

Feed System v1.0 can be accepted only after verification establishes:

- Accepted feed/discovery domain vocabulary.
- Accepted feed schema.
- Accepted feed RPC contracts, including parameters, return shapes, errors, and authorization behavior.
- Accepted feed RLS policies.
- Accepted feed surface definitions for Home, Discover, Rising, and Web/Public where applicable.
- Accepted event lifecycle and visibility filtering behavior.
- Accepted viewer-role, persona, and tier visibility behavior.
- Accepted host identity/avatar behavior.
- Accepted city/system item behavior.
- Accepted media/poster/gallery behavior.
- Accepted ranking, ordering, and personalization behavior.
- Accepted public sharing behavior.
- Accepted moderation/ops/admin behavior.
- Accepted cross-surface ownership for mobile, Web/Public, Dashboard/Ops, and Supabase backend.
- Accepted security-sensitive enforcement boundaries.
- Accepted maintainability ownership for feed UX, backend contracts, visibility, ranking, personalization, host identity, media/posters, public sharing, and moderation/ops/admin behavior.

Until these criteria are met, this document remains non-canonical.

## 25. Open Questions

- What is the accepted feed schema?
- What feed RPC contracts and RLS policies are accepted?
- What are the accepted Home, Discover, and Rising feed behaviors?
- What Web/Public feed or public discovery behavior is accepted?
- What event lifecycle states are feed-visible?
- What public/private filtering behavior is accepted?
- How do viewer roles affect feed visibility?
- How do personas and tiers affect feed visibility?
- What host identity/avatar behavior is accepted in feeds?
- How should city/system feed items behave when the source may not be a user profile?
- What media/poster/gallery selection behavior is accepted?
- What ranking or ordering behavior is accepted?
- What personalization behavior is accepted, if any?
- How does public sharing affect feed inclusion and public discovery?
- What moderation or ops/admin override behavior is accepted?
- What dashboard routes, components, and services own feed visibility tooling?
- What mobile feed behavior is accepted?
- What web/public feed behavior is accepted?
- Which surfaces support feeds today: mobile, Web/Public, Dashboard/Ops, and Supabase backend?

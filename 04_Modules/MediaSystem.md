# Media System

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is a platform-level umbrella draft specification for JoinFolk media concepts.

This is a handbook draft. It is not a code audit and is not an accepted implementation contract. MediaSystem.md covers platform-wide media boundaries across posters, venue media, profile/avatar media, host identity media, feed/discovery media, public media, storage, and gallery relationships.

MediaGallerySystem.md already exists as the detailed draft for gallery/media-gallery behavior. This document does not duplicate that specification.

All exact implementation behavior remains Unknown / Needs verification unless already accepted elsewhere. All schema names, storage bucket names, RPC names, route names, component names, and field names are known concepts only until verified.

## 3. Media System Definition

JoinFolk has media concepts.

Known facts:

- JoinFolk has media/gallery concepts.
- JoinFolk has event poster concepts.
- JoinFolk has venue/business media concepts.
- JoinFolk has profile/avatar or host identity media concepts.
- JoinFolk has feed/discovery media or poster concepts.
- JoinFolk has public sharing concepts.
- JoinFolk has mobile surfaces.
- JoinFolk has dashboard surfaces.
- JoinFolk may have Web/Public surfaces.

Unknown / needs verification:

- Exact media schema.
- Exact storage bucket model.
- Exact storage policy model.
- Exact upload, replace, remove, visibility, transformation, moderation, and public sharing behavior.

## 4. Relationship to MediaGallerySystem.md

MediaSystem.md is the umbrella document for platform-wide media concepts.

MediaGallerySystem.md is the detailed draft for media/gallery-specific behavior and should remain the primary reference for gallery module details, including:

- Gallery-specific schema questions.
- Gallery-specific media ordering and cover behavior.
- Gallery-specific upload, visibility, ownership, delete, moderation, and public sharing questions.
- Event media and venue media behavior where the concern is specifically gallery/media-gallery behavior.

Boundary guidance:

- Use MediaSystem.md for cross-platform media ownership, visibility, storage, poster, avatar, feed media, and public media boundaries.
- Use MediaGallerySystem.md for detailed gallery/media-gallery rules and open questions.

Unknown / needs verification:

- Exact boundary between general media behavior and media/gallery behavior.
- Whether any accepted behavior should be moved from this umbrella document into MediaGallerySystem.md or vice versa.

## 5. Authority Model

### What frontend may own

Frontend surfaces may own user experience concerns, subject to backend/RPC/RLS/storage enforcement for security-sensitive behavior.

Frontend-owned behavior may include:

- Media display UX.
- Upload form UX.
- Poster preview UX.
- Avatar/image display UX.
- Carousel presentation UX.
- Replace/remove interaction UX.
- Loading, empty, and error states.
- Client-side validation for usability.

Unknown / needs verification:

- Exact mobile media behavior.
- Exact dashboard media behavior.
- Exact Web/Public media behavior.
- Exact route/component/service ownership.

### What backend/RPC/RLS/storage policies must enforce

Backend, RPC, RLS, and storage policies must enforce security-sensitive media visibility and upload behavior.

Security-sensitive behavior includes:

- Upload authorization.
- Replace/remove authorization.
- Protected or non-public media visibility.
- Public sharing media visibility.
- Storage object access.
- Event poster access where security-sensitive.
- Venue media access where security-sensitive.
- Profile/avatar or host identity media access where security-sensitive.
- Feed/discovery media exposure where security-sensitive.
- Media moderation/ops/admin actions.
- Any behavior affecting event lifecycle, viewer roles, personas and tiers, venue/business tools, host identity, ticketing, reservations, notifications, staff/scanner, public sharing, or storage.

Unknown / needs verification:

- Exact RPC contracts.
- Exact RLS policies.
- Exact storage policies.
- Exact upload authorization behavior.
- Exact media visibility model.

### What must never be frontend-only

The following must never rely only on frontend checks:

- Media upload authorization.
- Media replace/remove authorization.
- Private, protected, or non-public media visibility.
- Public sharing media visibility.
- Storage object access.
- Event poster visibility where security-sensitive.
- Venue media visibility where security-sensitive.
- Profile/avatar or host identity media visibility where security-sensitive.
- Feed/discovery media exposure where security-sensitive.
- Moderation/ops/admin media actions.
- Any behavior exposing protected event, venue, profile, host, ticketing, reservation, ownership, staff, or public sharing data.

## 6. Known Media Concepts Draft

Known media concepts:

- Media.
- Media/gallery.
- Event posters.
- Venue media.
- Venue/business media.
- Profile/avatar media.
- Host identity media.
- Feed/discovery media or posters.
- Public media.
- Storage.
- Event poster upload.
- Venue media upload.
- Venue gallery carousel.
- Media carousel.
- Poster preview.
- Replace/remove poster.
- Cover/thumbnail behavior.
- Avatar/image display.

Known related product areas:

- Event lifecycle.
- Viewer roles.
- Personas and tiers.
- Venue/business tools.
- Host identity.
- Feed/discovery.
- Media gallery.
- Ticketing where applicable.
- Reservations where applicable.
- Notifications where applicable.
- Staff/scanner where applicable.
- Ops/admin/moderation.
- Public sharing.
- Storage.
- Supabase backend/RPC/RLS where security-sensitive.

Unknown / needs verification:

- Exact media vocabulary.
- Which concepts exist as persisted backend entities.
- Which concepts exist only as frontend UX.
- Which concepts are authoritative in backend/RPC/RLS/storage.

## 7. Known Storage / Schema / RPC / Component Concept Names Draft

Prior project context mentioned storage bucket-like concepts such as:

- `posters`
- `venue-posters`
- `venue-media`

Prior project context mentioned upload or media concepts such as:

- Event poster upload.
- Venue media upload.
- Venue gallery carousel.
- Media carousel.
- Poster preview.
- Replace/remove poster.
- Cover/thumbnail behavior.
- Avatar/image display.

These names and concepts are known concepts only and must not be treated as accepted canonical storage, schema, bucket, RPC, route, component, or behavior contracts until verified.

Unknown / needs verification:

- Exact schema names.
- Exact storage bucket names.
- Exact storage object paths.
- Exact storage policies.
- Exact RPC names.
- Exact RPC parameters.
- Exact RPC return shapes.
- Exact route/component names.
- Exact field names.

## 8. Non-Accepted Media Areas

The following areas are not accepted yet:

- Exact media schema.
- Exact storage bucket model.
- Exact storage policy model.
- Exact upload behavior.
- Exact delete/remove behavior.
- Exact replace behavior.
- Exact poster behavior.
- Exact venue media behavior.
- Exact gallery relationship behavior.
- Exact profile/avatar or host identity media behavior.
- Exact feed media/poster selection behavior.
- Exact public sharing/media visibility behavior.
- Exact image transformation, compression, thumbnail, CDN, or cache behavior.
- Exact moderation/ops/admin behavior.
- Exact mobile media behavior.
- Exact dashboard media behavior.
- Exact web/public media behavior.
- Exact RPC contracts.
- Exact RLS policies.
- Exact storage policies.

These areas must remain Unknown / Needs verification until verified through accepted source material.

## 9. Event Poster Draft

Known facts:

- JoinFolk has event poster concepts.
- Prior project context mentioned event poster upload, poster preview, and replace/remove poster.

Unknown / needs verification:

- Exact poster behavior.
- Exact event poster schema.
- Exact event poster storage model.
- Exact event poster upload, replace, remove, preview, visibility, and public sharing behavior.
- Whether event poster behavior depends on event lifecycle, viewer roles, personas and tiers, feed/discovery, ticketing, reservations, notifications, ops/admin/moderation, or Web/Public surfaces.
- Which backend/RPC/RLS/storage behavior enforces event poster authority.

No exact event poster behavior is accepted in this draft.

## 10. Venue / Business Media Draft

Known facts:

- JoinFolk has venue/business media concepts.
- Media behavior may interact with venue/business tools.
- Prior project context mentioned `venue-posters`, `venue-media`, venue media upload, venue gallery carousel, and media carousel.

Unknown / needs verification:

- Exact venue media behavior.
- Exact venue/business media schema.
- Exact venue media storage model.
- Exact venue media upload, replace, remove, visibility, carousel, and public sharing behavior.
- Exact relationship between venue media and MediaGallerySystem.md.
- Which backend/RPC/RLS/storage behavior enforces venue media authority.

No exact venue/business media behavior is accepted in this draft.

## 11. Profile / Avatar / Host Identity Media Draft

Known facts:

- JoinFolk has profile/avatar or host identity media concepts.
- Prior project context mentioned avatar/image display.

Unknown / needs verification:

- Exact profile/avatar media behavior.
- Exact host identity media behavior.
- Exact upload, replace, remove, visibility, and public sharing behavior for profile/avatar or host identity media.
- Whether profile/avatar or host identity media affects feed/discovery, public sharing, event posters, venue/business tools, notifications, or ops/admin/moderation.
- Which backend/RPC/RLS/storage behavior enforces profile/avatar or host identity media authority.

No exact profile/avatar or host identity media behavior is accepted in this draft.

## 12. Feed / Discovery Media Draft

Known facts:

- JoinFolk has feed/discovery media or poster concepts.
- Media behavior may interact with feed/discovery.

Unknown / needs verification:

- Exact feed media/poster selection behavior.
- Whether feed/discovery media uses event posters, venue media, profile/avatar media, host identity media, gallery media, or another source.
- Whether feed media visibility depends on event lifecycle, viewer roles, personas and tiers, public sharing, ticketing, reservations, ops/admin/moderation, or storage policies.
- Which backend/RPC/RLS/storage behavior enforces feed media visibility.

No exact feed/discovery media behavior is accepted in this draft.

## 13. Public Sharing / Public Media Draft

Known facts:

- JoinFolk has public sharing concepts.
- Media behavior may interact with public sharing.
- Media visibility is security-sensitive where private/protected/non-public media may be exposed.

Unknown / needs verification:

- Exact public sharing/media visibility behavior.
- Which media can be public.
- Whether public media includes event posters, venue media, profile/avatar media, host identity media, feed media, or gallery media.
- Whether public media visibility depends on event lifecycle, viewer roles, personas and tiers, venue/business tools, feed/discovery, ticketing, reservations, notifications, staff/scanner, or ops/admin/moderation.
- Which backend/RPC/RLS/storage behavior enforces public media visibility.

No exact public sharing or public media behavior is accepted in this draft.

## 14. Media Gallery Relationship Draft

Known facts:

- JoinFolk has media/gallery concepts.
- MediaGallerySystem.md is the detailed draft for media/gallery-specific behavior and should remain the primary reference for gallery module details.

Unknown / needs verification:

- Exact gallery relationship behavior.
- Whether event posters, venue media, profile/avatar media, host identity media, feed media, or public media are part of media/gallery behavior.
- Which media concepts are owned by MediaSystem.md versus MediaGallerySystem.md.

No exact media gallery relationship behavior is accepted in this draft.

## 15. Upload / Replace / Remove Draft

Known facts:

- Prior project context mentioned event poster upload.
- Prior project context mentioned venue media upload.
- Prior project context mentioned replace/remove poster.
- Backend/RPC/RLS/storage policies must enforce security-sensitive media visibility and upload behavior.

Unknown / needs verification:

- Exact upload behavior.
- Exact replace behavior.
- Exact delete/remove behavior.
- Exact file validation behavior.
- Exact storage bucket selection.
- Exact storage path selection.
- Exact authorization behavior.
- Exact failure and retry behavior.
- Whether upload, replace, or remove behavior differs across event posters, venue media, profile/avatar media, host identity media, feed media, public media, or gallery media.

No exact upload, replace, or remove behavior is accepted in this draft.

## 16. Transformation / Thumbnail / Cache Draft

Known facts:

- Prior project context mentioned cover/thumbnail behavior.

Unknown / needs verification:

- Exact image transformation behavior.
- Exact compression behavior.
- Exact thumbnail behavior.
- Exact cover behavior.
- Exact CDN behavior.
- Exact cache behavior.
- Whether transformation, thumbnail, CDN, or cache behavior differs by event posters, venue media, profile/avatar media, host identity media, feed media, public media, or gallery media.
- Which backend/RPC/RLS/storage behavior enforces protected transformed media access where security-sensitive.

No transformation, compression, thumbnail, CDN, or cache behavior is accepted in this draft.

## 17. Moderation / Ops/Admin Draft

Known facts:

- Media behavior may interact with ops/admin/moderation.

Unknown / needs verification:

- Exact moderation behavior.
- Exact ops/admin media behavior.
- Whether ops/admin can review, approve, reject, hide, remove, restore, replace, audit, or otherwise manage media.
- Whether moderation differs for event posters, venue media, profile/avatar media, host identity media, feed media, public media, or gallery media.
- Which backend/RPC/RLS/storage behavior enforces moderation/ops/admin media authority.

No moderation or ops/admin media behavior is accepted in this draft.

## 18. Relationship to Product Domains

### Event lifecycle

Known relationship:

- Media behavior may interact with event lifecycle.

Unknown / needs verification:

- Exact event lifecycle relationship.

### Viewer roles

Known relationship:

- Media behavior may interact with viewer roles.

Unknown / needs verification:

- Exact viewer-role media visibility and upload relationship.

### Personas and tiers

Known relationship:

- Media behavior may interact with personas and tiers.

Unknown / needs verification:

- Exact persona/tier media relationship.

### Venue/business tools

Known relationship:

- Media behavior may interact with venue/business tools.

Unknown / needs verification:

- Exact venue/business tools media relationship.

### Host identity

Known relationship:

- Media behavior may interact with host identity.

Unknown / needs verification:

- Exact host identity media relationship.

### Feed/discovery

Known relationship:

- Media behavior may interact with feed/discovery.

Unknown / needs verification:

- Exact feed/discovery media relationship.

### Media gallery

Known relationship:

- JoinFolk has media/gallery concepts.
- MediaGallerySystem.md is the detailed draft for media/gallery-specific behavior.

Unknown / needs verification:

- Exact media gallery relationship.

### Ticketing

Known relationship:

- Media behavior may interact with ticketing where applicable.

Unknown / needs verification:

- Exact ticketing relationship.

### Reservations

Known relationship:

- Media behavior may interact with reservations where applicable.

Unknown / needs verification:

- Exact reservation relationship.

### Notifications

Known relationship:

- Media behavior may interact with notifications where applicable.

Unknown / needs verification:

- Exact notification relationship.

### Staff scanner

Known relationship:

- Media behavior may interact with staff/scanner where applicable.

Unknown / needs verification:

- Exact staff/scanner relationship.

### Ops/admin/moderation

Known relationship:

- Media behavior may interact with ops/admin/moderation.

Unknown / needs verification:

- Exact ops/admin/moderation relationship.

### Public sharing

Known relationship:

- Media behavior may interact with public sharing.

Unknown / needs verification:

- Exact public sharing relationship.

### Supabase storage/backend

Known relationship:

- Media behavior may interact with storage.
- Media behavior may interact with Supabase backend/RPC/RLS where security-sensitive.

Unknown / needs verification:

- Exact Supabase storage/backend relationship.

## 19. Cross-Surface Consistency Requirements

### Mobile

Known facts:

- JoinFolk has mobile surfaces.
- Exact mobile media behavior is not accepted yet.

Unknown / needs verification:

- Exact mobile media behavior.
- Whether mobile uploads, displays, replaces, removes, previews, shares, or moderates media.
- Which mobile behavior must match dashboard, Web/Public, or backend/storage behavior.

### Dashboard

Known facts:

- JoinFolk has dashboard surfaces.
- Exact dashboard media behavior is not accepted yet.

Unknown / needs verification:

- Exact dashboard media behavior.
- Whether dashboard uploads, displays, replaces, removes, previews, shares, or moderates media.
- Exact dashboard route/component/service ownership.
- Which dashboard behavior must match mobile, Web/Public, or backend/storage behavior.

### Web/Public where applicable

Known facts:

- JoinFolk may have Web/Public surfaces.
- Media behavior may interact with public sharing.
- Exact web/public media behavior is not accepted yet.

Unknown / needs verification:

- Exact Web/Public media behavior.
- Exact public media visibility behavior.
- Whether Web/Public displays event posters, venue media, profile/avatar media, host identity media, feed media, public media, or gallery media.
- Which Web/Public behavior must match mobile, dashboard, or backend/storage behavior.

### Supabase Backend / Storage

Known requirement:

- Backend/RPC/RLS/storage policies must enforce security-sensitive media visibility and upload behavior.

Unknown / needs verification:

- Exact media schema.
- Exact storage bucket model.
- Exact storage policy model.
- Exact RPC contracts.
- Exact RLS policies.
- Exact backend/storage ownership boundaries.
- Exact enforcement model for upload, replace, remove, visibility, public sharing, transformations, moderation, and storage access.

## 20. Security Risks

Known risks:

- Media visibility is security-sensitive where private/protected/non-public media may be exposed.
- Backend/RPC/RLS/storage policies must enforce security-sensitive media visibility and upload behavior.
- Frontend media behavior is UX only where security-sensitive.

Security risks to verify:

- Unauthorized upload.
- Unauthorized replace or remove.
- Unauthorized storage object access.
- Private, protected, or non-public media exposure.
- Public sharing visibility mismatch.
- Feed/discovery media exposing protected content.
- Profile/avatar or host identity media exposing protected content.
- Venue media or event poster visibility bypass.
- Moderation/ops/admin media actions not enforced by backend/RPC/RLS/storage.
- Frontend-only checks being treated as enforcement.

## 21. Privacy Risks

Privacy risks to verify:

- Private or protected media exposed on public surfaces.
- Profile/avatar or host identity media exposed beyond accepted visibility.
- Feed/discovery media exposing private event, host, venue, or profile data.
- Media metadata exposing protected event, venue, profile, ownership, ticketing, reservation, or staff/scanner information.
- Thumbnails, cached images, CDN entries, or transformed media remaining visible after visibility changes.
- Public sharing exposing media without accepted rules.

## 22. Determinism Risks

Known determinism risks:

- Exact upload, replace, remove, poster, venue media, gallery relationship, feed media/poster selection, and public sharing behavior is not accepted yet.

Risks to verify:

- Media visibility differing across mobile, dashboard, Web/Public, and backend/storage.
- Event poster selection differing across surfaces.
- Venue media carousel behavior differing across surfaces.
- Feed media/poster selection differing across feed surfaces.
- Profile/avatar or host identity media display differing across surfaces.
- Public sharing media exposure diverging from backend/storage authority.
- Gallery behavior and umbrella media behavior becoming inconsistent.

## 23. Performance Risks

Known performance risks:

- Exact image transformation, compression, thumbnail, CDN, or cache behavior is not accepted yet.

Risks to verify:

- Large media causing slow mobile, dashboard, or Web/Public rendering.
- Missing thumbnail or compression behavior causing excessive bandwidth use.
- Cache or CDN behavior showing stale or unauthorized media.
- Media carousel or gallery surfaces loading too much media without accepted performance boundaries.

## 24. Maintainability Risks

Known maintainability risks:

- Exact media schema is not accepted yet.
- Exact storage bucket model is not accepted yet.
- Exact storage policy model is not accepted yet.
- Exact mobile, dashboard, and Web/Public media behavior is not accepted yet.
- Prior schema, table, bucket, storage policy, RPC, route, component, and field names are known concepts only, not accepted contracts.

Risks to verify:

- Media behavior scattered across mobile, dashboard, Web/Public, gallery, feed, profiles, venues, events, and storage without clear ownership.
- Frontend code encoding security-sensitive media visibility or upload rules.
- Upload, replace, remove, visibility, transformation, moderation, feed media selection, and gallery relationship logic duplicated without accepted ownership documentation.
- MediaSystem.md and MediaGallerySystem.md duplicating or contradicting each other.

## 25. Current Known Implementation

Current accepted implementation knowledge is limited to the facts in this draft:

- JoinFolk has media concepts.
- JoinFolk has media/gallery concepts.
- JoinFolk has event poster concepts.
- JoinFolk has venue/business media concepts.
- JoinFolk has profile/avatar or host identity media concepts.
- JoinFolk has feed/discovery media or poster concepts.
- JoinFolk has public sharing concepts.
- JoinFolk has mobile surfaces.
- JoinFolk has dashboard surfaces.
- JoinFolk may have Web/Public surfaces.
- MediaGallerySystem.md is the detailed draft for media/gallery-specific behavior.
- Prior context mentioned storage bucket-like concepts and upload/media concepts, but none are accepted canonical contracts.

Unknown / needs verification:

- Exact accepted implementation across mobile, dashboard, Web/Public, Supabase backend/storage, event posters, venue media, profile/avatar media, host identity media, feed/discovery media, public sharing, and media gallery behavior.

## 26. Unknowns / Needs Verification

The following must be verified before v1.0:

- Exact media schema.
- Exact storage bucket model.
- Exact storage policy model.
- Exact upload behavior.
- Exact delete/remove behavior.
- Exact replace behavior.
- Exact poster behavior.
- Exact venue media behavior.
- Exact gallery relationship behavior.
- Exact profile/avatar or host identity media behavior.
- Exact feed media/poster selection behavior.
- Exact public sharing/media visibility behavior.
- Exact image transformation, compression, thumbnail, CDN, or cache behavior.
- Exact moderation/ops/admin behavior.
- Exact mobile media behavior.
- Exact dashboard media behavior.
- Exact web/public media behavior.
- Exact RPC contracts.
- Exact RLS policies.
- Exact storage policies.
- Exact relationship between MediaSystem.md and MediaGallerySystem.md.
- Exact relationship between media and event lifecycle.
- Exact relationship between media and viewer roles.
- Exact relationship between media and personas/tiers.
- Exact relationship between media and venue/business tools.
- Exact relationship between media and host identity.
- Exact relationship between media and feed/discovery.
- Exact relationship between media and ticketing where applicable.
- Exact relationship between media and reservations where applicable.
- Exact relationship between media and notifications where applicable.
- Exact relationship between media and staff/scanner where applicable.
- Exact relationship between media and public sharing.
- Exact relationship between media and Supabase storage/backend.

## 27. Acceptance Criteria for v1.0

Media System v1.0 can be accepted only after verification establishes:

- Accepted media domain vocabulary.
- Accepted boundary between MediaSystem.md and MediaGallerySystem.md.
- Accepted media schema.
- Accepted storage bucket model.
- Accepted storage policies.
- Accepted upload, replace, and remove behavior.
- Accepted event poster behavior.
- Accepted venue/business media behavior.
- Accepted profile/avatar and host identity media behavior.
- Accepted feed/discovery media behavior.
- Accepted public sharing/media visibility behavior.
- Accepted media gallery relationship.
- Accepted transformation, compression, thumbnail, CDN, and cache behavior, if applicable.
- Accepted moderation/ops/admin media behavior.
- Accepted RPC contracts, including parameters, return shapes, errors, and authorization behavior.
- Accepted RLS policies.
- Accepted cross-surface ownership for mobile, dashboard, Web/Public where applicable, and Supabase backend/storage.
- Accepted security-sensitive enforcement boundaries.
- Accepted privacy boundaries.
- Accepted maintainability ownership for media UX, backend contracts, storage policies, upload, replace, remove, visibility, transformation, moderation, feed media selection, public sharing, and media gallery boundaries.

Until these criteria are met, this document remains non-canonical.

## 28. Open Questions

- What is the accepted media schema?
- What is the accepted storage bucket model?
- What storage policies are accepted?
- What upload behavior is accepted?
- What replace and remove behavior is accepted?
- What event poster behavior is accepted?
- What venue/business media behavior is accepted?
- What profile/avatar or host identity media behavior is accepted?
- What feed media/poster selection behavior is accepted?
- What public sharing/media visibility behavior is accepted?
- What exact boundary should exist between MediaSystem.md and MediaGallerySystem.md?
- Which media behavior belongs in MediaGallerySystem.md rather than this umbrella document?
- What image transformation, compression, thumbnail, CDN, or cache behavior is accepted, if any?
- What moderation or ops/admin media behavior is accepted?
- What mobile media behavior is accepted?
- What dashboard media behavior is accepted?
- What Web/Public media behavior is accepted?
- What RPC contracts, RLS policies, and storage policies enforce media behavior?
- Which surfaces support media today: mobile, dashboard, Web/Public, and Supabase backend/storage?

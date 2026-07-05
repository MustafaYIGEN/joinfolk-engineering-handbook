# Media / Gallery System

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is a platform-level draft specification for the JoinFolk Media / Gallery System.

It records known media/gallery and storage-related concepts, known risk areas, and areas that must be verified before any behavior is treated as accepted or canonical. It does not define exact media/gallery schema, storage buckets, storage policies, upload pipeline behavior, public/private visibility rules, file size/type rules, ownership behavior, moderation/deletion behavior, ordering/cover behavior, public sharing behavior, event media behavior, venue media behavior, dashboard ownership, or mobile behavior.

## 3. Media / Gallery System Definition

Media/gallery is a JoinFolk product domain.

Known facts:

- JoinFolk uses Supabase storage or storage-related behavior.
- Storage/media behavior is security-sensitive where protected media or protected files are involved.
- Mobile may interact with media/gallery behavior.
- Dashboard may support media/gallery management.
- Venue/business behavior may interact with media/gallery and storage.

Unknown / needs verification:

- The exact media/gallery schema.
- The exact accepted storage buckets.
- The exact accepted storage policies.
- The exact media ownership and visibility model.
- The exact mobile and dashboard behavior.
- The exact relationship between media/gallery, storage, event lifecycle, ticketing/check-in, reservations, venue/business tools, notifications, staff scanner, ops/admin, and public sharing.

## 4. Media Authority Model

### What frontend may own

Frontend surfaces may own user experience concerns, subject to backend/RPC/RLS/storage enforcement for security-sensitive behavior.

Frontend-owned behavior may include:

- Upload form presentation.
- Gallery display UX.
- Media management UX.
- Cover or ordering interaction UX.
- Local draft state.
- Client-side validation for usability.
- Loading, empty, and error states.
- Mobile media/gallery flow UX.
- Dashboard media/gallery workflow composition.

Unknown / needs verification:

- Exact mobile media/gallery behavior.
- Exact dashboard routes for media/gallery tools.
- Exact dashboard components for media/gallery tools.
- Exact frontend service boundaries.
- Exact upload/display behavior.

### What backend/RPC/RLS/storage must enforce

Backend, RPC, RLS, and storage policies must enforce security-sensitive media/gallery behavior.

Security-sensitive behavior includes:

- Protected media or protected file access.
- Public/private media visibility.
- Media ownership.
- Which viewer roles may upload, view, update, remove, moderate, or manage media.
- Which viewer roles may manage event media.
- Which viewer roles may manage venue media.
- Which viewer roles may access protected storage objects.
- Any media/gallery behavior that affects event lifecycle, ticketing/check-in, reservations, venue/business tools, public sharing, notifications, staff scanner, or ops/admin workflows.

Unknown / needs verification:

- Exact media/gallery RLS policies.
- Exact storage policies.
- Exact RPC authorization model.
- Exact accepted backend contracts.
- Exact accepted media ownership model.
- Exact accepted protected media/file visibility model.

### What must never be frontend-only

The following must never rely only on frontend checks:

- Protected media or protected file access.
- Public/private media visibility.
- Media ownership.
- Media upload authorization.
- Media update authorization.
- Media delete or moderation authorization.
- Event media access or management.
- Venue media access or management.
- Storage object read/write/update/delete authorization.
- Any operation involving viewer roles, personas, tiers, ticketing/check-in, reservations, venue/business tools, notifications, staff scanner, ops/admin, or public sharing.

## 5. Known Media / Storage Concepts Draft

Known media/storage concepts:

- Media/gallery.
- Supabase storage or storage-related behavior.
- Protected media.
- Protected files.
- Event media.
- Venue media.
- Posters.
- Venue posters.
- Event videos.
- Media ownership.
- Public/private media visibility.
- Upload pipeline.
- Media ordering.
- Cover behavior.
- Moderation/deletion.

Known related product areas:

- Event lifecycle.
- Viewer roles.
- Personas and tiers.
- Ticketing/check-in.
- Reservations.
- Venue/business tools.
- Storage.
- Public sharing.
- Notifications where applicable.
- Staff scanner where applicable.
- Ops/admin.

Unknown / needs verification:

- Exact domain vocabulary.
- Which concepts exist as persisted backend entities.
- Which concepts exist only as frontend UX.
- Which concepts are authoritative in backend/RPC/RLS/storage.
- Whether event media and venue media share schema, storage, visibility, or behavior.

## 6. Known Schema / RPC / Storage Concept Names Draft

Prior project context mentioned storage concepts such as:

- `posters`
- `venue-posters`
- `venue-media`
- `event videos`

Prior audit/project context mentioned RPC or RPC-like names related to venue media. These names are known concepts only and are not accepted canonical storage buckets, schema, or RPC contracts until verified.

Known RPC/RPC-like concept names:

- `list_venue_media_v1`
- `add_venue_media_v1`
- `remove_venue_media_v1`
- `update_venue_media_v1`

Unknown / needs verification:

- Whether these storage/RPC-like names exist in the accepted backend.
- Whether these names are current.
- Whether these names are exposed to frontend clients.
- Exact media/gallery table schemas.
- Exact storage bucket names.
- Exact storage object paths.
- Exact RPC parameters.
- Exact RPC return shapes.
- Exact error behavior.
- Exact authorization behavior.
- Exact RLS behavior.
- Exact storage policy behavior.

## 7. Non-Accepted Media Areas

The following areas are not accepted yet:

- Exact media/gallery schema.
- Exact storage buckets.
- Exact storage policies.
- Exact media upload pipeline.
- Exact public/private media visibility rules.
- Exact file size/type rules.
- Exact media ownership model.
- Exact media moderation/deletion behavior.
- Exact media ordering/cover behavior.
- Exact public sharing media behavior.
- Exact event media behavior.
- Exact venue media behavior.
- Exact dashboard route/component/service ownership for media/gallery tools.
- Exact mobile media/gallery behavior.

These areas must remain Unknown / Needs verification until verified through accepted source material.

## 8. Event Media Draft

Known facts:

- Media/gallery behavior may interact with event lifecycle.
- Prior project context mentioned event videos as a storage concept.

Unknown / needs verification:

- Exact event media schema.
- Exact event media storage model.
- Exact event media upload behavior.
- Exact event media visibility rules.
- Exact event media ownership model.
- Exact event media ordering or cover behavior.
- Exact event media deletion or moderation behavior.
- Whether event media interacts with ticketing/check-in, reservations, notifications, staff scanner, ops/admin, or public sharing.

No exact event media behavior is accepted in this draft.

## 9. Venue Media Draft

Known facts:

- Venue/business behavior may interact with media/gallery and storage.
- Prior project context mentioned storage concepts such as `venue-posters` and `venue-media`.
- Prior audit/project context mentioned RPC or RPC-like concepts for listing, adding, removing, and updating venue media.

Unknown / needs verification:

- Exact venue media schema.
- Exact venue media storage model.
- Exact venue media upload behavior.
- Exact venue media visibility rules.
- Exact venue media ownership model.
- Exact venue media ordering or cover behavior.
- Exact venue media deletion or moderation behavior.
- Whether venue media interacts with reservations, notifications, staff scanner, ops/admin, public sharing, ticketing/check-in, or event lifecycle.
- Exact RPC contracts for venue media reads or updates.

No exact venue media behavior is accepted in this draft.

## 10. Upload Pipeline Draft

Known facts:

- JoinFolk uses Supabase storage or storage-related behavior.
- Storage/media behavior is security-sensitive where protected media or protected files are involved.
- Frontend upload/display behavior is UX only where security-sensitive.

Unknown / needs verification:

- Exact upload pipeline behavior.
- Exact file size rules.
- Exact file type rules.
- Exact validation behavior.
- Exact storage bucket selection.
- Exact storage path selection.
- Exact transformation or processing behavior.
- Exact failure and retry behavior.
- Which viewer roles may upload media.
- Which backend/RPC/RLS/storage policies enforce upload authorization.

No exact upload pipeline behavior is accepted in this draft.

## 11. Media Ownership / Visibility Draft

Known facts:

- Storage/media behavior is security-sensitive where protected media or protected files are involved.
- Exact media ownership model is not accepted yet.
- Exact public/private media visibility rules are not accepted yet.

Unknown / needs verification:

- Exact media ownership model.
- Exact public/private media visibility rules.
- Whether ownership differs for event media and venue media.
- Whether visibility differs for event media and venue media.
- Whether visibility depends on viewer roles, personas and tiers, event lifecycle, ticketing/check-in, reservations, venue/business tools, ops/admin, or public sharing.
- Which backend/RPC/RLS/storage behavior enforces ownership and visibility.

No media ownership or visibility behavior is accepted in this draft.

## 12. Media Ordering / Cover / Gallery Presentation Draft

Known facts:

- Exact media ordering/cover behavior is not accepted yet.
- Frontend upload/display behavior is UX only where security-sensitive.

Unknown / needs verification:

- Exact media ordering behavior.
- Exact cover behavior.
- Exact gallery presentation behavior.
- Whether ordering or cover behavior differs for event media and venue media.
- Whether ordering or cover behavior affects public sharing.
- Which backend/RPC/RLS/storage behavior, if any, is authoritative for ordering or cover state.

No media ordering, cover, or gallery presentation behavior is accepted in this draft.

## 13. Delete / Moderation Draft

Known facts:

- Exact media moderation/deletion behavior is not accepted yet.
- Storage/media behavior is security-sensitive where protected media or protected files are involved.

Unknown / needs verification:

- Exact media deletion behavior.
- Exact media moderation behavior.
- Whether deletion or moderation differs for event media and venue media.
- Which viewer roles may delete or moderate media.
- Whether deletion removes storage objects, metadata, public visibility, or some combination.
- Whether moderation interacts with public sharing, notifications, ops/admin, event lifecycle, reservations, or venue/business tools.
- Which backend/RPC/RLS/storage behavior enforces deletion or moderation.

No media deletion or moderation behavior is accepted in this draft.

## 14. Public Sharing Media Draft

Known facts:

- Media/gallery behavior may interact with public sharing.
- Exact public sharing media behavior is not accepted yet.

Unknown / needs verification:

- Exact public sharing media behavior.
- Whether event media can be publicly shared.
- Whether venue media can be publicly shared.
- Whether posters, venue posters, venue media, or event videos are exposed publicly.
- Whether public sharing respects protected media or protected file rules.
- Which backend/RPC/RLS/storage behavior enforces public sharing visibility.

No public sharing media behavior is accepted in this draft.

## 15. Relationship to Product Domains

### Personas and tiers

Known relationship:

- Media/gallery behavior may interact with personas and tiers.

Unknown / needs verification:

- Whether personas or tiers affect media upload, display, ownership, visibility, moderation, deletion, ordering, cover behavior, public sharing, or storage access.

### Event lifecycle

Known relationship:

- Media/gallery behavior may interact with event lifecycle.

Unknown / needs verification:

- Whether event lifecycle state affects media upload, display, ownership, visibility, moderation, deletion, ordering, cover behavior, notifications, or public sharing.

### Viewer roles

Known relationship:

- Media/gallery behavior may interact with viewer roles.

Unknown / needs verification:

- Exact viewer-role rules for media upload, viewing, update, deletion, moderation, ownership, visibility, storage access, dashboard management, mobile behavior, and public sharing.

### Ticketing/check-in

Known relationship:

- Media/gallery behavior may interact with ticketing/check-in.

Unknown / needs verification:

- Whether ticketing/check-in affects media access, upload, display, ownership, visibility, notifications, staff scanner behavior, or public sharing.

### Reservations

Known relationship:

- Media/gallery behavior may interact with reservations.

Unknown / needs verification:

- Whether reservations affect media access, upload, display, ownership, visibility, notifications, venue/business tools, or public sharing.

### Venue/business tools

Known relationship:

- Media/gallery behavior may interact with venue/business tools.
- Venue/business behavior may interact with media/gallery and storage.

Unknown / needs verification:

- Exact relationship between media/gallery and venue/business tools.
- Whether venue/business tools affect venue media, venue posters, upload, ownership, visibility, ordering, cover behavior, deletion, moderation, notifications, ops/admin, or public sharing.

### Storage

Known relationship:

- JoinFolk uses Supabase storage or storage-related behavior.
- Storage/media behavior is security-sensitive where protected media or protected files are involved.

Unknown / needs verification:

- Exact storage bucket model.
- Exact storage object path model.
- Exact storage policies.
- Exact relationship between storage objects and media/gallery metadata.

### Notifications

Known relationship:

- Media/gallery behavior may interact with notifications where applicable.

Unknown / needs verification:

- Whether media upload, update, deletion, moderation, ownership, visibility, ordering, cover behavior, or public sharing triggers notifications.

### Staff scanner

Known relationship:

- Media/gallery behavior may interact with staff scanner where applicable.

Unknown / needs verification:

- Whether staff scanner behavior consumes, displays, uploads, or gates media/gallery behavior.
- Whether staff scanner authority affects protected media or protected files.

### Ops/admin

Known relationship:

- Media/gallery behavior may interact with ops/admin.

Unknown / needs verification:

- Exact admin workflows.
- Exact operational override, support, deletion, or moderation behavior.
- Exact auditability requirements.

### Public sharing

Known relationship:

- Media/gallery behavior may interact with public sharing.

Unknown / needs verification:

- Exact public sharing media behavior.
- Whether public sharing exposes posters, venue posters, venue media, event videos, metadata, ownership, visibility, ordering, cover state, or gallery presentation data.

## 16. Cross-Surface Consistency Requirements

### Mobile

Known facts:

- Mobile may interact with media/gallery behavior.
- Exact mobile media/gallery behavior is not accepted yet.

Unknown / needs verification:

- Whether mobile supports upload, display, management, deletion, moderation, ordering, cover behavior, public sharing, or protected media access.
- Whether mobile consumes event media, venue media, storage-backed media, public sharing data, notification data, ticketing/check-in data, reservation data, or staff scanner data.
- Which mobile behavior must match dashboard, web/public, or backend behavior.

### Dashboard

Known facts:

- Dashboard may support media/gallery management.

Unknown / needs verification:

- Exact dashboard route/component/service ownership for media/gallery tools.
- Exact dashboard event media management behavior.
- Exact dashboard venue media management behavior.
- Exact dashboard upload, deletion, moderation, ordering, cover, ownership, visibility, notification, public sharing, storage, ticketing/check-in, reservation, venue/business, staff scanner, or ops/admin behavior related to media/gallery.

### Web/Public

Known relationship:

- Media/gallery behavior may interact with public sharing.

Unknown / needs verification:

- Whether media/gallery has public pages, public share views, or public media exposure.
- Exact public visibility rules.
- Exact public event media, venue media, posters, venue posters, event videos, metadata, ownership, visibility, ordering, cover, notification, ticketing/check-in, reservation, or venue/business exposure.

### Supabase Backend

Known requirement:

- Backend/RPC/RLS/storage policies must enforce security-sensitive media/gallery behavior.

Unknown / needs verification:

- Exact schema.
- Exact RPC contracts.
- Exact RLS policies.
- Exact storage buckets.
- Exact storage policies.
- Exact backend ownership boundaries.
- Exact enforcement model for protected media, protected files, ownership, visibility, event media, venue media, upload, deletion, moderation, ordering, cover, and public sharing.

## 17. Security Risks

Known risks:

- Storage/media behavior is security-sensitive where protected media or protected files are involved.
- Backend/RPC/RLS/storage policies must enforce security-sensitive media/gallery behavior.
- Frontend upload/display behavior is UX only where security-sensitive.

Security risks to verify:

- Unauthorized protected media access.
- Unauthorized protected file access.
- Unauthorized media upload.
- Unauthorized media update.
- Unauthorized media deletion or moderation.
- Unauthorized event media management.
- Unauthorized venue media management.
- Unauthorized public exposure of protected media or protected files.
- Incorrect public/private visibility enforcement.
- Storage object access that bypasses backend/RPC/RLS/storage policy expectations.
- Frontend-only checks being treated as enforcement.

## 18. Determinism Risks

Known determinism risks:

- Exact media upload pipeline is not accepted yet.
- Exact public/private media visibility rules are not accepted yet.
- Exact media ordering/cover behavior is not accepted yet.
- Exact event media behavior is not accepted yet.
- Exact venue media behavior is not accepted yet.

Risks to verify:

- Inconsistent media visibility across mobile, dashboard, web/public, and backend.
- Different surfaces deriving media ownership differently.
- Storage objects and media metadata diverging.
- Media ordering or cover behavior differing across surfaces.
- Public sharing exposing different media than protected backend/storage rules allow.
- Event media and venue media behavior being conflated without accepted rules.

## 19. Maintainability Risks

Known maintainability risks:

- Exact media/gallery schema is not accepted yet.
- Exact storage buckets are not accepted yet.
- Exact storage policies are not accepted yet.
- Exact dashboard route/component/service ownership for media/gallery tools is not accepted yet.
- Exact mobile media/gallery behavior is not accepted yet.

Risks to verify:

- Media/gallery behavior scattered across surfaces without clear ownership.
- RPC-like names, schema-like names, or storage-like names used as implicit contracts before verification.
- Frontend components encoding security-sensitive rules.
- Event media and venue media behavior duplicated or conflated across surfaces.
- Upload, ownership, visibility, deletion, moderation, ordering, cover, storage, notification, public sharing, and ops/admin logic duplicated without accepted ownership documentation.

## 20. Current Known Implementation

Current accepted implementation knowledge is limited to the facts in this draft:

- Media/gallery is a JoinFolk product domain.
- JoinFolk uses Supabase storage or storage-related behavior.
- Storage/media behavior is security-sensitive where protected media or protected files are involved.
- Mobile may interact with media/gallery behavior.
- Dashboard may support media/gallery management.
- Venue/business behavior may interact with media/gallery and storage.
- Prior project context mentioned storage concepts such as `posters`, `venue-posters`, `venue-media`, and `event videos`.
- Prior context mentioned RPC or RPC-like venue media names, but none are accepted canonical contracts.

Unknown / needs verification:

- Exact accepted implementation across frontend, backend, RLS, storage, dashboard, mobile, web/public, event media, venue media, ticketing/check-in, reservations, venue/business tools, notifications, staff scanner, ops/admin, and public sharing surfaces.

## 21. Unknowns / Needs Verification

The following must be verified before v1.0:

- Exact media/gallery schema.
- Exact storage buckets.
- Exact storage policies.
- Exact media upload pipeline.
- Exact public/private media visibility rules.
- Exact file size/type rules.
- Exact media ownership model.
- Exact media moderation/deletion behavior.
- Exact media ordering/cover behavior.
- Exact public sharing media behavior.
- Exact event media behavior.
- Exact venue media behavior.
- Exact dashboard route/component/service ownership for media/gallery tools.
- Exact mobile media/gallery behavior.
- Exact relationship between media/gallery and personas/tiers.
- Exact relationship between media/gallery and event lifecycle.
- Exact relationship between media/gallery and viewer roles.
- Exact relationship between media/gallery and ticketing/check-in.
- Exact relationship between media/gallery and reservations.
- Exact relationship between media/gallery and venue/business tools.
- Exact relationship between media/gallery and storage.
- Exact relationship between media/gallery and notifications where applicable.
- Exact relationship between media/gallery and staff scanner where applicable.
- Exact relationship between media/gallery and ops/admin.
- Exact relationship between media/gallery and public sharing.

## 22. Acceptance Criteria for v1.0

Media / Gallery System v1.0 can be accepted only after verification establishes:

- Accepted media/gallery domain vocabulary.
- Accepted media/gallery schema.
- Accepted storage bucket model.
- Accepted storage policies.
- Accepted media upload pipeline.
- Accepted public/private media visibility rules.
- Accepted file size/type rules.
- Accepted media ownership model.
- Accepted media moderation/deletion behavior.
- Accepted media ordering/cover/gallery presentation behavior.
- Accepted public sharing media behavior.
- Accepted event media behavior.
- Accepted venue media behavior.
- Accepted RPC contracts, including parameters, return shapes, errors, and authorization behavior.
- Accepted RLS policies.
- Accepted cross-surface ownership for mobile, dashboard, web/public, and Supabase backend.
- Accepted security-sensitive enforcement boundaries.
- Accepted maintainability ownership for routes, components, services, backend contracts, RLS, storage policies, upload, ownership, visibility, deletion, moderation, ordering, cover, notifications, staff scanner, ops/admin, and public sharing.

Until these criteria are met, this document remains non-canonical.

## 23. Open Questions

- What is the accepted media/gallery schema?
- What storage buckets are accepted for media/gallery behavior?
- What storage policies are accepted for protected media and protected files?
- What is the accepted upload pipeline?
- What file size and file type rules are accepted?
- What are the accepted public/private media visibility rules?
- What is the accepted media ownership model?
- What is the accepted event media behavior?
- What is the accepted venue media behavior?
- How do posters, venue posters, venue media, and event videos relate to each other?
- What media ordering, cover, and gallery presentation behavior is accepted?
- What deletion and moderation behavior is accepted?
- Which viewer roles can upload, view, update, delete, moderate, or manage media?
- How does media/gallery behavior interact with personas and tiers?
- How does media/gallery behavior interact with event lifecycle?
- How does media/gallery behavior interact with ticketing/check-in?
- How does media/gallery behavior interact with reservations?
- How does media/gallery behavior interact with venue/business tools?
- Which media/gallery behavior triggers notifications, if any?
- Where does staff scanner behavior apply to media/gallery, if anywhere?
- What media can be publicly shared?
- What dashboard routes, components, and services own media/gallery tools?
- What mobile media/gallery behavior is accepted?
- Which surfaces support media/gallery today: mobile, dashboard, web/public, and Supabase backend?

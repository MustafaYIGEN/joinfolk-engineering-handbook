# Venue System

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is a platform-level draft specification for JoinFolk venue and business tools.

It records known venue/business concepts, known risk areas, and areas that must be verified before any behavior is treated as accepted or canonical. It does not define exact database schema, RPC contracts, RLS policies, storage policies, frontend ownership, reservation flows, media behavior, layout behavior, or public sharing behavior.

## 3. Venue System Definition

The Venue System is the JoinFolk product domain for venue and business tools.

Known facts:

- JoinFolk has venue/business tools.
- Venue/business tools are a product domain.
- Dashboard may support venue/business tools.
- Venue/business tools include reservations.
- JoinFolk has venue reservations.

Unknown / needs verification:

- The exact venue schema.
- The exact accepted venue lifecycle.
- The exact dashboard route, component, and service ownership for venue tools.
- The exact relationship between venues, businesses, events, tickets, reservations, media, storage, and public sharing.

## 4. Venue Authority Model

### What frontend may own

Frontend surfaces may own user experience concerns, subject to backend enforcement for security-sensitive behavior.

Frontend-owned behavior may include:

- Form presentation.
- Editor interaction design.
- Local draft state.
- Client-side validation for usability.
- Loading, empty, and error states.
- Dashboard navigation and workflow composition.

Unknown / needs verification:

- Exact dashboard routes for venue/business tools.
- Exact dashboard components for venue/business tools.
- Exact frontend service boundaries.
- Exact editor behavior and persistence model.

### What backend/RPC/RLS/storage must enforce

Backend, RPC, RLS, and storage policies must enforce security-sensitive venue behavior.

Security-sensitive behavior includes:

- Which viewer roles may list, view, create, update, archive, or otherwise manage venues.
- Which viewer roles may access venue reservations.
- Which viewer roles may update venue reservation status.
- Which viewer roles may access, add, remove, or update venue media.
- Which viewer roles may create, edit, save, or link venue layouts.
- Which viewer roles may access storage objects related to venues.
- Any venue behavior that affects commerce, ticketing, reservations, public sharing, or operational/admin workflows.

Unknown / needs verification:

- Exact venue RLS policies.
- Exact venue storage policies.
- Exact RPC authorization model.
- Exact accepted backend contracts.

### What must never be frontend-only

The following must never rely only on frontend checks:

- Venue access control.
- Venue ownership or management permissions.
- Reservation access or status changes.
- Media upload, removal, update, or read authorization.
- Layout creation, update, save, or event-linking authorization.
- Product-section or ticket mapping behavior that affects commerce or ticketing.
- Public sharing visibility rules.
- Any operation involving viewer roles, personas, tiers, commerce, reservations, ticketing, storage, or ops/admin access.

## 5. Known Venue / Business Concepts Draft

Known venue/business concepts:

- Venue/business tools.
- Venue reservations.
- Venue media.
- Venue posters.
- Venue layout/editor concepts.
- Possible dashboard support for venue/business tools.

Known related product areas:

- Event lifecycle.
- Personas and tiers.
- Viewer roles.
- Commerce.
- Ticketing.
- Reservations.
- Media/gallery.
- Storage.
- Public sharing.
- Ops/admin.

Unknown / needs verification:

- Exact domain vocabulary.
- Whether "venue" and "business" are separate accepted entities or one product area with multiple concepts.
- Which concepts exist as persisted backend entities.
- Which concepts are dashboard-only UX.

## 6. Known RPC / Storage Concept Names Draft

Prior audit/project context mentioned RPC or RPC-like names related to venues. These names are known concepts only and are not accepted canonical RPC contracts until verified.

Known RPC/RPC-like concept names:

- `list_my_venues_v1`
- `get_venue_v1`
- `create_venue_v1`
- `update_venue_v1`
- `archive_venue_v1`
- `get_venue_reservations_v1`
- `update_venue_reservation_status_v1`
- `list_venue_media_v1`
- `add_venue_media_v1`
- `remove_venue_media_v1`
- `update_venue_media_v1`
- `get_venue_layout_v1`
- `create_visual_venue_layout_v1`
- `save_venue_layout_v1`
- `link_event_venue_layout_v1`

Prior project context mentioned storage concepts:

- `venue-posters`
- `venue-media`

Unknown / needs verification:

- Whether these RPC/RPC-like names exist in the accepted backend.
- Whether these names are current.
- Whether these names are exposed to frontend clients.
- Exact RPC parameters.
- Exact RPC return shapes.
- Exact error behavior.
- Exact authorization behavior.
- Exact storage bucket names.
- Exact storage object paths.
- Exact storage read/write policies.

## 7. Non-Accepted Venue Areas

The following areas are not accepted yet:

- Exact venue schema.
- Exact venue RPC contracts.
- Exact venue RLS policies.
- Exact venue storage policies.
- Exact venue media behavior.
- Exact venue reservation behavior.
- Exact venue layout/editor behavior.
- Exact product-section mapping behavior.
- Exact public sharing behavior for venues.
- Exact dashboard route/component/service ownership for venue tools.

These areas must remain Unknown / Needs verification until verified through accepted source material.

## 8. Venue Lifecycle / State Draft

Known facts:

- Venue/business tools exist as a product domain.
- Event reservations and venue reservations may use different status flows.

Unknown / needs verification:

- Exact venue lifecycle states.
- Whether venues can be archived, deleted, hidden, published, drafted, or otherwise transitioned.
- Whether any lifecycle state affects public sharing.
- Whether any lifecycle state affects reservations, media, layout, commerce, or ticketing.
- Whether any lifecycle state is represented in frontend state, backend schema, RPC behavior, RLS, or storage policies.

No exact venue statuses are accepted in this draft.

## 9. Venue Reservations Draft

Known facts:

- JoinFolk has venue reservations.
- Venue/business tools include reservations.
- Event reservations and venue reservations may use different status flows.

Unknown / needs verification:

- Exact venue reservation schema.
- Exact venue reservation status flow.
- Whether venue reservations share behavior with event reservations.
- Which viewer roles may view venue reservations.
- Which viewer roles may update venue reservation status.
- Whether venue reservations interact with commerce, ticketing, public sharing, ops/admin workflows, or notifications.
- Exact RPC contracts for venue reservation reads or updates.

No exact venue reservation behavior is accepted in this draft.

## 10. Venue Media / Storage Draft

Known facts:

- Venue/business behavior may interact with media/gallery and storage.
- Prior project context mentioned storage concepts such as `venue-posters` and `venue-media`.
- Prior audit/project context mentioned RPC or RPC-like concepts for listing, adding, removing, and updating venue media.

Unknown / needs verification:

- Exact venue media schema.
- Exact media/gallery behavior for venues.
- Exact storage buckets.
- Exact storage paths.
- Exact allowed file types, sizes, transformations, or ordering behavior.
- Exact storage read/write/update/delete policies.
- Exact relationship between venue posters and venue media.
- Whether public sharing can expose venue media.

No exact venue media or storage behavior is accepted in this draft.

## 11. Venue Layout / Editor Draft

Known facts:

- Dashboard audit found the venue editor as a high-risk area.
- Prior audit/project context mentioned RPC or RPC-like concepts for venue layouts, visual venue layout creation, saving venue layouts, and linking event venue layouts.

Unknown / needs verification:

- Exact venue layout schema.
- Exact venue editor behavior.
- Exact visual layout model.
- Exact save behavior.
- Exact event-to-venue-layout linking behavior.
- Whether venue layouts affect reservations, ticketing, commerce, event lifecycle, or public sharing.
- Which frontend surface owns the editor UX.
- Which backend contracts enforce editor persistence and authorization.

No exact venue layout/editor behavior is accepted in this draft.

## 12. Ticket / Product-Section Mapping Relationship

Known facts:

- Dashboard audit found ticket/product-section mapping as a high-risk area.
- Venue/business behavior may interact with commerce and ticketing.

Unknown / needs verification:

- Exact product-section mapping behavior.
- Whether product-section mapping is venue-specific, event-specific, or shared.
- Whether product-section mapping affects ticket availability, reservation availability, commerce, venue layouts, or public sharing.
- Which system is authoritative for mapping validation.
- Which backend/RPC/RLS behavior enforces mapping rules.
- Which dashboard components expose mapping UX.

No exact product-section mapping behavior is accepted in this draft.

## 13. Relationship to Product Domains

### Personas and tiers

Known relationship:

- Venue/business behavior may interact with personas and tiers.

Unknown / needs verification:

- Whether personas or tiers affect venue access, management, reservation behavior, media access, layout editing, commerce, or public sharing.

### Event lifecycle

Known relationship:

- Venue/business behavior may interact with event lifecycle.

Unknown / needs verification:

- Whether event lifecycle state affects venue selection, venue reservations, layout linking, ticketing, product-section mapping, media, or public sharing.

### Viewer roles

Known relationship:

- Venue/business behavior may interact with viewer roles.

Unknown / needs verification:

- Exact viewer-role rules for venue listing, viewing, creation, update, archiving, reservations, media, layouts, commerce, and public sharing.

### Commerce

Known relationship:

- Venue/business behavior may interact with commerce.

Unknown / needs verification:

- Exact commerce relationship.
- Whether venue reservations, product-section mapping, layouts, or public sharing affect commerce.

### Ticketing

Known relationship:

- Venue/business behavior may interact with ticketing.
- Ticket/product-section mapping is a high-risk area.

Unknown / needs verification:

- Exact ticketing relationship.
- Exact product-section mapping behavior.
- Whether venue layout or reservation behavior affects ticketing.

### Reservations

Known relationship:

- Venue/business tools include reservations.
- JoinFolk has venue reservations.
- Event reservations and venue reservations may use different status flows.

Unknown / needs verification:

- Exact reservation model for venues.
- Exact relationship between venue reservations and event reservations.

### Media/gallery

Known relationship:

- Venue/business behavior may interact with media/gallery.

Unknown / needs verification:

- Exact gallery behavior.
- Exact relationship between venue media, venue posters, and public sharing.

### Storage

Known relationship:

- Venue/business behavior may interact with storage.
- Prior project context mentioned `venue-posters` and `venue-media`.

Unknown / needs verification:

- Exact storage policy model.
- Exact bucket and object path model.

### Ops/admin

Known relationship:

- Venue/business behavior may interact with ops/admin.

Unknown / needs verification:

- Exact admin workflows.
- Exact operational override or support behavior.
- Exact auditability requirements.

### Public sharing

Known relationship:

- Venue/business behavior may interact with public sharing.

Unknown / needs verification:

- Exact public sharing behavior for venues.
- Whether public sharing exposes venue data, venue media, reservation availability, layout information, commerce information, or ticketing information.

## 14. Cross-Surface Consistency Requirements

### Mobile

Unknown / needs verification:

- Whether mobile supports venue/business tools.
- Whether mobile consumes venue data, venue reservations, venue media, layout information, public sharing data, or ticket/product-section mapping data.
- Which mobile behavior must match dashboard, web/public, or backend behavior.

### Dashboard

Known facts:

- Dashboard may support venue/business tools.
- Dashboard audit found venue editor as a high-risk area.
- Dashboard audit found ticket/product-section mapping as a high-risk area.

Unknown / needs verification:

- Exact dashboard route/component/service ownership for venue tools.
- Exact dashboard editor behavior.
- Exact dashboard reservation behavior.
- Exact dashboard media behavior.
- Exact dashboard mapping behavior.

### Web/Public

Known relationship:

- Venue/business behavior may interact with public sharing.

Unknown / needs verification:

- Whether venues have public pages or public share views.
- Exact public visibility rules.
- Exact public media, reservation, commerce, ticketing, or layout exposure.

### Supabase Backend

Known requirement:

- Backend/RPC/RLS/storage policies must enforce security-sensitive venue behavior.

Unknown / needs verification:

- Exact schema.
- Exact RPC contracts.
- Exact RLS policies.
- Exact storage policies.
- Exact backend ownership boundaries.

## 15. Security Risks

Known risks:

- Venue editor behavior is high risk.
- Ticket/product-section mapping is high risk.
- Security-sensitive venue behavior must be enforced by backend/RPC/RLS/storage, not frontend-only checks.

Security risks to verify:

- Unauthorized venue access.
- Unauthorized venue updates or archiving.
- Unauthorized reservation access or status changes.
- Unauthorized media access, upload, update, or removal.
- Unauthorized layout editing or event-layout linking.
- Unauthorized public exposure of venue data or media.
- Commerce or ticketing manipulation through product-section mapping.
- Dashboard-only checks being treated as enforcement.

## 16. Determinism Risks

Known determinism risks:

- Event reservations and venue reservations may use different status flows.
- Exact venue reservation behavior is not accepted yet.
- Exact venue layout/editor behavior is not accepted yet.
- Exact product-section mapping behavior is not accepted yet.

Risks to verify:

- Inconsistent reservation status interpretation across event and venue workflows.
- Different surfaces deriving venue state differently.
- Dashboard editor state diverging from backend persisted state.
- Media ordering or gallery presentation differing across surfaces.
- Product-section mapping producing different results across dashboard, backend, ticketing, or commerce surfaces.

## 17. Maintainability Risks

Known maintainability risks:

- Dashboard route/component/service ownership for venue tools is not accepted yet.
- Exact venue RPC contracts are not accepted yet.
- Exact venue schema is not accepted yet.

Risks to verify:

- Venue/business behavior scattered across surfaces without clear ownership.
- RPC-like names used as implicit contracts before verification.
- Frontend components encoding security-sensitive rules.
- Reservation, media, layout, and product-section mapping logic duplicated across surfaces.
- Storage concepts used without accepted policy and ownership documentation.

## 18. Current Known Implementation

Current accepted implementation knowledge is limited to the facts in this draft:

- JoinFolk has venue/business tools.
- Venue/business tools are a product domain.
- Dashboard may support venue/business tools.
- Dashboard audit found venue editor as a high-risk area.
- Dashboard audit found ticket/product-section mapping as a high-risk area.
- JoinFolk has venue reservations.
- Venue/business tools include reservations.
- Prior context mentioned storage concepts such as `venue-posters` and `venue-media`.
- Prior context mentioned RPC or RPC-like venue names, but none are accepted canonical contracts.

Unknown / needs verification:

- Exact accepted implementation across frontend, backend, storage, RLS, dashboard, mobile, and web/public surfaces.

## 19. Unknowns / Needs Verification

The following must be verified before v1.0:

- Exact venue schema.
- Exact venue lifecycle and statuses.
- Exact venue RPC contracts.
- Exact venue RLS policies.
- Exact venue storage policies.
- Exact venue media behavior.
- Exact venue reservation behavior.
- Exact venue layout/editor behavior.
- Exact product-section mapping behavior.
- Exact public sharing behavior for venues.
- Exact dashboard route/component/service ownership for venue tools.
- Exact relationship between venues and businesses.
- Exact relationship between venue reservations and event reservations.
- Exact relationship between venues and personas/tiers.
- Exact relationship between venues and viewer roles.
- Exact relationship between venues and commerce.
- Exact relationship between venues and ticketing.
- Exact relationship between venues and ops/admin workflows.

## 20. Acceptance Criteria for v1.0

Venue System v1.0 can be accepted only after verification establishes:

- Accepted venue/business domain vocabulary.
- Accepted venue schema.
- Accepted venue lifecycle and state model.
- Accepted venue reservation model and status flow.
- Accepted venue media and storage model.
- Accepted venue layout/editor model.
- Accepted product-section mapping behavior.
- Accepted public sharing behavior for venues.
- Accepted RPC contracts, including parameters, return shapes, errors, and authorization behavior.
- Accepted RLS policies.
- Accepted storage policies.
- Accepted cross-surface ownership for mobile, dashboard, web/public, and Supabase backend.
- Accepted security-sensitive enforcement boundaries.
- Accepted maintainability ownership for routes, components, services, backend contracts, storage, and policies.

Until these criteria are met, this document remains non-canonical.

## 21. Open Questions

- What is the accepted venue schema?
- Are "venue" and "business" separate accepted entities, or one product area with multiple concepts?
- What is the accepted venue lifecycle and state model?
- What are the accepted venue reservation statuses and transitions?
- How do venue reservations differ from event reservations?
- Which viewer roles can list, view, create, update, archive, or manage venues?
- Which viewer roles can view or update venue reservations?
- Which viewer roles can access, add, remove, or update venue media?
- What are the accepted storage buckets, paths, and policies for venue posters and venue media?
- What is the accepted venue layout/editor model?
- What does linking an event to a venue layout mean?
- What is the accepted ticket/product-section mapping behavior?
- Which backend/RPC/RLS/storage rules enforce ticket/product-section mapping?
- What venue data, media, layout, reservation, commerce, or ticketing information can be publicly shared?
- What dashboard routes, components, and services own venue/business tools?
- Which surfaces support venue/business tools today: mobile, dashboard, web/public, and Supabase backend?

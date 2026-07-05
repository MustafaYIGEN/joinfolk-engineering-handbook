# Reservation System

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is a platform-level draft specification for the JoinFolk Reservation System.

It records known reservation concepts, known risk areas, and areas that must be verified before any behavior is treated as accepted or canonical. It does not define exact reservation schema, event reservation schema, venue reservation schema, status flows, create/update/cancel/approve/reject behavior, ownership behavior, RPC contracts, RLS policies, notification behavior, public sharing behavior, dashboard ownership, or mobile behavior.

## 3. Reservation System Definition

Reservations are a JoinFolk product domain.

Known facts:

- JoinFolk has event reservations.
- JoinFolk has venue reservations.
- Event reservations and venue reservations may use different status flows.
- Venue/business tools include reservations.
- Mobile may create or interact with reservations.
- Dashboard may support reservations management.
- Dashboard may support venue reservations management.

Unknown / needs verification:

- The exact reservation schema.
- The exact event reservation schema.
- The exact venue reservation schema.
- The exact reservation lifecycle/status flows.
- The exact ownership model.
- The exact mobile and dashboard behavior.

## 4. Reservation Authority Model

### What frontend may own

Frontend surfaces may own user experience concerns, subject to backend enforcement for security-sensitive behavior.

Frontend-owned behavior may include:

- Reservation form presentation.
- Reservation management UX.
- Local draft state.
- Client-side validation for usability.
- Loading, empty, and error states.
- Mobile reservation flow UX.
- Dashboard reservation workflow composition.

Unknown / needs verification:

- Exact mobile reservation behavior.
- Exact dashboard routes for reservation tools.
- Exact dashboard components for reservation tools.
- Exact frontend service boundaries.
- Exact UX behavior for event reservations and venue reservations.

### What backend/RPC/RLS must enforce

Backend, RPC, and RLS must enforce security-sensitive reservation behavior.

Security-sensitive behavior includes:

- Reservation authority.
- Protected public/private visibility.
- Which viewer roles may create or interact with reservations.
- Which viewer roles may view, update, cancel, approve, or reject reservations.
- Which viewer roles may manage event reservations.
- Which viewer roles may manage venue reservations.
- Which viewer roles may access protected public/private reservation data.
- Any reservation behavior that affects commerce, ticketing, wallet/ownership, venue/business tools, notifications, staff scanner/check-in, ops/admin, or public sharing.

Unknown / needs verification:

- Exact reservation RLS policies.
- Exact reservation RPC authorization model.
- Exact accepted backend contracts.
- Exact accepted reservation authority model.
- Exact protected public/private visibility model.

### What must never be frontend-only

The following must never rely only on frontend checks:

- Reservation authority.
- Protected public/private visibility.
- Reservation ownership.
- Reservation create/update/cancel/approve/reject behavior.
- Event reservation status changes.
- Venue reservation status changes.
- Public/private visibility decisions.
- Any operation involving viewer roles, personas, tiers, commerce, ticketing, wallet/ownership, venue/business tools, notifications, staff scanner/check-in, ops/admin, or public sharing.

## 5. Known Reservation Concepts Draft

Known reservation concepts:

- Reservations.
- Event reservations.
- Venue reservations.
- Reservation ownership.
- Reservation status flows.
- Create/update/cancel/approve/reject concepts.
- Protected public/private visibility.
- Reservation management.
- Venue reservation management.

Known related product areas:

- Event lifecycle.
- Viewer roles.
- Personas and tiers.
- Commerce.
- Ticketing.
- Wallet/ownership.
- Venue/business tools.
- Media/gallery where applicable.
- Notifications.
- Staff scanner or check-in where applicable.
- Ops/admin.
- Public sharing.

Unknown / needs verification:

- Exact domain vocabulary.
- Which concepts exist as persisted backend entities.
- Which concepts exist only as frontend UX.
- Which concepts are authoritative in backend/RPC/RLS.
- Whether event reservations and venue reservations share schema or behavior.

## 6. Known Schema / RPC Concept Names Draft

Prior project context mentioned tables or concepts such as:

- `reservations`

Prior audit/project context mentioned RPC or RPC-like names related to reservations. These names are known concepts only and are not accepted canonical schema or RPC contracts until verified.

Known RPC/RPC-like concept names:

- `get_event_reservations_v1`
- `update_reservation_status_v1`
- `get_venue_reservations_v1`
- `update_venue_reservation_status_v1`
- `create_reservation_v1`
- `create_reservation_v2`

Unknown / needs verification:

- Whether these schema/RPC-like names exist in the accepted backend.
- Whether these names are current.
- Whether these names are exposed to frontend clients.
- Exact table schemas.
- Exact RPC parameters.
- Exact RPC return shapes.
- Exact error behavior.
- Exact authorization behavior.
- Exact RLS behavior.

## 7. Non-Accepted Reservation Areas

The following areas are not accepted yet:

- Exact reservation schema.
- Exact event reservation schema.
- Exact venue reservation schema.
- Exact reservation status flow.
- Exact event reservation status flow.
- Exact venue reservation status flow.
- Exact create/update/cancel/approve/reject behavior.
- Exact reservation ownership model.
- Exact reservation RPC contracts.
- Exact reservation RLS policies.
- Exact notification behavior for reservations.
- Exact public sharing behavior for reservations.
- Exact dashboard route/component/service ownership for reservation tools.
- Exact mobile reservation behavior.

These areas must remain Unknown / Needs verification until verified through accepted source material.

## 8. Event Reservations Draft

Known facts:

- JoinFolk has event reservations.
- Event reservations and venue reservations may use different status flows.
- Reservation behavior may interact with event lifecycle.

Unknown / needs verification:

- Exact event reservation schema.
- Exact event reservation status flow.
- Exact event reservation create/update/cancel/approve/reject behavior.
- Exact event reservation ownership model.
- Which viewer roles may create, view, update, cancel, approve, or reject event reservations.
- Whether event reservations interact with commerce, ticketing, wallet/ownership, notifications, staff scanner/check-in, ops/admin, media/gallery, or public sharing.
- Exact RPC contracts for event reservation reads or updates.

No exact event reservation behavior is accepted in this draft.

## 9. Venue Reservations Draft

Known facts:

- JoinFolk has venue reservations.
- Venue/business tools include reservations.
- Dashboard may support venue reservations management.
- Event reservations and venue reservations may use different status flows.

Unknown / needs verification:

- Exact venue reservation schema.
- Exact venue reservation status flow.
- Exact venue reservation create/update/cancel/approve/reject behavior.
- Exact venue reservation ownership model.
- Which viewer roles may create, view, update, cancel, approve, or reject venue reservations.
- Whether venue reservations interact with event lifecycle, commerce, ticketing, wallet/ownership, notifications, staff scanner/check-in, ops/admin, media/gallery, or public sharing.
- Exact RPC contracts for venue reservation reads or updates.

No exact venue reservation behavior is accepted in this draft.

## 10. Reservation Lifecycle / Status Draft

Known facts:

- Event reservations and venue reservations may use different status flows.
- Exact reservation status flow is not accepted yet.
- Exact event reservation status flow is not accepted yet.
- Exact venue reservation status flow is not accepted yet.

Unknown / needs verification:

- Exact reservation lifecycle states.
- Exact reservation statuses.
- Whether event reservations and venue reservations share any status concepts.
- Whether reservation statuses are affected by create, update, cancel, approve, reject, commerce, ticketing, wallet/ownership, notifications, staff scanner/check-in, ops/admin, or public sharing behavior.
- Whether reservation lifecycle state is represented in frontend state, backend schema, RPC behavior, or RLS policies.

No exact reservation statuses are accepted in this draft.

## 11. Create / Update / Cancel / Approve / Reject Draft

Known facts:

- Exact create/update/cancel/approve/reject behavior is not accepted yet.
- Mobile may create or interact with reservations.
- Dashboard may support reservations management.

Unknown / needs verification:

- Whether create, update, cancel, approve, or reject behavior applies to event reservations, venue reservations, or both.
- Exact create behavior.
- Exact update behavior.
- Exact cancel behavior.
- Exact approve behavior.
- Exact reject behavior.
- Which viewer roles may perform each action.
- Which backend/RPC/RLS rules enforce each action.
- Whether these actions affect reservation status, ownership, commerce, ticketing, wallet/ownership, notifications, staff scanner/check-in, ops/admin, or public sharing.

No create/update/cancel/approve/reject behavior is accepted in this draft.

## 12. Reservation Ownership Draft

Known facts:

- Reservation authority is security-sensitive.
- Exact reservation ownership model is not accepted yet.
- Reservation behavior may interact with wallet/ownership.

Unknown / needs verification:

- Exact reservation ownership model.
- Whether event reservations and venue reservations use the same ownership model.
- Whether reservation ownership relates to wallet ownership or ticket ownership.
- Which backend/RPC/RLS behavior enforces reservation ownership.
- Whether ownership affects create/update/cancel/approve/reject behavior, status changes, notifications, public sharing, staff scanner/check-in, or ops/admin behavior.

No exact reservation ownership behavior is accepted in this draft.

## 13. Notifications Relationship Draft

Known facts:

- Reservation behavior may interact with notifications.
- Exact notification behavior for reservations is not accepted yet.

Unknown / needs verification:

- Which reservation actions or status changes trigger notifications.
- Whether event reservations and venue reservations trigger different notifications.
- Which surfaces receive or display reservation notifications.
- Whether notifications are affected by viewer roles, personas and tiers, ownership, public/private visibility, commerce, ticketing, venue/business tools, staff scanner/check-in, or ops/admin workflows.

No notification behavior for reservations is accepted in this draft.

## 14. Public Sharing / Visibility Draft

Known facts:

- Reservation behavior may interact with public sharing.
- Protected public/private visibility is security-sensitive.
- Exact public sharing behavior for reservations is not accepted yet.

Unknown / needs verification:

- Exact public sharing behavior for reservations.
- Exact protected public/private visibility rules.
- Whether event reservations or venue reservations can be publicly visible.
- Whether public sharing exposes reservation status, ownership, availability, commerce, ticketing, venue/business, media/gallery, staff scanner/check-in, or notification information.
- Which backend/RPC/RLS behavior enforces public/private visibility.

No public sharing or visibility behavior for reservations is accepted in this draft.

## 15. Relationship to Product Domains

### Personas and tiers

Known relationship:

- Reservation behavior may interact with personas and tiers.

Unknown / needs verification:

- Whether personas or tiers affect reservation creation, interaction, ownership, status changes, visibility, notifications, commerce, ticketing, venue/business tools, or ops/admin workflows.

### Event lifecycle

Known relationship:

- Reservation behavior may interact with event lifecycle.
- JoinFolk has event reservations.

Unknown / needs verification:

- Whether event lifecycle state affects reservation creation, status, ownership, visibility, commerce, ticketing, notifications, staff scanner/check-in, or public sharing.

### Viewer roles

Known relationship:

- Reservation behavior may interact with viewer roles.

Unknown / needs verification:

- Exact viewer-role rules for reservation creation, interaction, ownership, status changes, visibility, management, notifications, staff scanner/check-in, ops/admin, and public sharing.

### Commerce

Known relationship:

- Reservation behavior may interact with commerce.

Unknown / needs verification:

- Exact commerce relationship.
- Whether reservations affect commerce or commerce affects reservation lifecycle, ownership, visibility, ticketing, notifications, or public sharing.

### Ticketing

Known relationship:

- Reservation behavior may interact with ticketing.

Unknown / needs verification:

- Exact ticketing relationship.
- Whether reservations affect ticket lifecycle, ticket availability, ownership, wallet behavior, check-in, staff scanner, notifications, or public sharing.

### Wallet/ownership

Known relationship:

- Reservation behavior may interact with wallet/ownership.

Unknown / needs verification:

- Exact wallet/ownership relationship.
- Whether reservation ownership affects or is affected by wallet ownership or ticket ownership.

### Venue/business tools

Known relationship:

- Reservation behavior may interact with venue/business tools.
- Venue/business tools include reservations.
- JoinFolk has venue reservations.

Unknown / needs verification:

- Exact relationship between reservations and venue/business tools.
- Whether venue/business tools affect reservation creation, status, ownership, visibility, media/gallery, notifications, ops/admin, or public sharing.

### Media/gallery

Known relationship:

- Reservation behavior may interact with media/gallery where applicable.

Unknown / needs verification:

- Whether media/gallery behavior applies to reservations.
- Whether event reservations or venue reservations have media/gallery relationships.
- Whether media/gallery behavior affects public sharing or protected visibility.

### Notifications

Known relationship:

- Reservation behavior may interact with notifications.

Unknown / needs verification:

- Exact notification relationship.
- Which reservation actions, status changes, ownership changes, or visibility changes trigger notifications.

### Staff scanner / check-in

Known relationship:

- Reservation behavior may interact with staff scanner or check-in where applicable.

Unknown / needs verification:

- Whether staff scanner or check-in applies to event reservations, venue reservations, or both.
- Whether staff scanner/check-in affects reservation lifecycle, ownership, visibility, ticketing, notifications, or ops/admin workflows.

### Ops/admin

Known relationship:

- Reservation behavior may interact with ops/admin.

Unknown / needs verification:

- Exact admin workflows.
- Exact operational override or support behavior.
- Exact auditability requirements.

### Public sharing

Known relationship:

- Reservation behavior may interact with public sharing.
- Protected public/private visibility is security-sensitive.

Unknown / needs verification:

- Exact public sharing behavior for reservations.
- Exact protected public/private visibility rules.
- Whether public sharing exposes reservation data, status, ownership, availability, commerce, ticketing, venue/business, media/gallery, notification, staff scanner, or check-in information.

## 16. Cross-Surface Consistency Requirements

### Mobile

Known facts:

- Mobile may create or interact with reservations.
- Exact mobile reservation behavior is not accepted yet.

Unknown / needs verification:

- Whether mobile supports event reservations, venue reservations, or both.
- Exact mobile create or interaction behavior.
- Whether mobile consumes reservation data, ownership data, visibility data, commerce data, ticketing data, venue/business data, notification data, public sharing data, or staff scanner/check-in data.
- Which mobile behavior must match dashboard, web/public, or backend behavior.

### Dashboard

Known facts:

- Dashboard may support reservations management.
- Dashboard may support venue reservations management.

Unknown / needs verification:

- Exact dashboard route/component/service ownership for reservation tools.
- Exact dashboard event reservation management behavior.
- Exact dashboard venue reservation management behavior.
- Exact dashboard create/update/cancel/approve/reject behavior.
- Exact dashboard ownership, visibility, notification, public sharing, staff scanner/check-in, commerce, ticketing, or venue/business behavior related to reservations.

### Web/Public

Known relationship:

- Reservation behavior may interact with public sharing.
- Protected public/private visibility is security-sensitive.

Unknown / needs verification:

- Whether reservations have public pages or public share views.
- Exact public visibility rules.
- Exact public reservation status, ownership, availability, commerce, ticketing, venue/business, media/gallery, notification, staff scanner, or check-in exposure.

### Supabase Backend

Known requirement:

- Backend/RPC/RLS must enforce security-sensitive reservation behavior.

Unknown / needs verification:

- Exact schema.
- Exact RPC contracts.
- Exact RLS policies.
- Exact backend ownership boundaries.
- Exact enforcement model for reservation authority, ownership, protected public/private visibility, event reservations, and venue reservations.

## 17. Security Risks

Known risks:

- Reservation authority is security-sensitive.
- Protected public/private visibility is security-sensitive.
- Security-sensitive reservation behavior must be enforced by backend/RPC/RLS, not frontend-only checks.

Security risks to verify:

- Unauthorized reservation access.
- Unauthorized reservation creation or interaction.
- Unauthorized reservation update, cancel, approve, or reject behavior.
- Unauthorized reservation ownership changes.
- Unauthorized event reservation management.
- Unauthorized venue reservation management.
- Unauthorized public exposure of reservation data.
- Incorrect public/private visibility enforcement.
- Commerce, ticketing, wallet/ownership, venue/business, notification, staff scanner/check-in, or ops/admin manipulation through reservation behavior.
- Frontend-only checks being treated as enforcement.

## 18. Determinism Risks

Known determinism risks:

- Event reservations and venue reservations may use different status flows.
- Exact reservation status flow is not accepted yet.
- Exact event reservation status flow is not accepted yet.
- Exact venue reservation status flow is not accepted yet.
- Exact create/update/cancel/approve/reject behavior is not accepted yet.

Risks to verify:

- Inconsistent reservation status interpretation across surfaces.
- Event reservation and venue reservation status flows being treated as interchangeable when they are not.
- Different surfaces deriving reservation authority differently.
- Protected public/private visibility differing across mobile, dashboard, web/public, and backend.
- Notifications triggered inconsistently by reservation behavior.
- Staff scanner/check-in behavior, where applicable, producing inconsistent reservation results across surfaces.

## 19. Maintainability Risks

Known maintainability risks:

- Exact reservation RPC contracts are not accepted yet.
- Exact reservation schema is not accepted yet.
- Exact dashboard route/component/service ownership for reservation tools is not accepted yet.
- Exact mobile reservation behavior is not accepted yet.

Risks to verify:

- Reservation behavior scattered across surfaces without clear ownership.
- RPC-like names or schema-like names used as implicit contracts before verification.
- Frontend components encoding security-sensitive rules.
- Event reservation and venue reservation behavior duplicated or conflated across surfaces.
- Ownership, visibility, notifications, commerce, ticketing, wallet/ownership, venue/business, staff scanner/check-in, and ops/admin logic duplicated without accepted ownership documentation.

## 20. Current Known Implementation

Current accepted implementation knowledge is limited to the facts in this draft:

- Reservations are a JoinFolk product domain.
- JoinFolk has event reservations.
- JoinFolk has venue reservations.
- Event reservations and venue reservations may use different status flows.
- Venue/business tools include reservations.
- Mobile may create or interact with reservations.
- Dashboard may support reservations management.
- Dashboard may support venue reservations management.
- Prior project context mentioned tables or concepts such as `reservations`.
- Prior context mentioned RPC or RPC-like reservation names, but none are accepted canonical contracts.

Unknown / needs verification:

- Exact accepted implementation across frontend, backend, RLS, dashboard, mobile, web/public, event reservations, venue reservations, commerce, ticketing, wallet/ownership, notifications, staff scanner/check-in, ops/admin, and public sharing surfaces.

## 21. Unknowns / Needs Verification

The following must be verified before v1.0:

- Exact reservation schema.
- Exact event reservation schema.
- Exact venue reservation schema.
- Exact reservation status flow.
- Exact event reservation status flow.
- Exact venue reservation status flow.
- Exact create/update/cancel/approve/reject behavior.
- Exact reservation ownership model.
- Exact reservation RPC contracts.
- Exact reservation RLS policies.
- Exact notification behavior for reservations.
- Exact public sharing behavior for reservations.
- Exact dashboard route/component/service ownership for reservation tools.
- Exact mobile reservation behavior.
- Exact relationship between reservations and personas/tiers.
- Exact relationship between reservations and event lifecycle.
- Exact relationship between reservations and viewer roles.
- Exact relationship between reservations and commerce.
- Exact relationship between reservations and ticketing.
- Exact relationship between reservations and wallet/ownership.
- Exact relationship between reservations and venue/business tools.
- Exact relationship between reservations and media/gallery where applicable.
- Exact relationship between reservations and staff scanner/check-in where applicable.
- Exact relationship between reservations and ops/admin.

## 22. Acceptance Criteria for v1.0

Reservation System v1.0 can be accepted only after verification establishes:

- Accepted reservation domain vocabulary.
- Accepted reservation schema.
- Accepted event reservation schema.
- Accepted venue reservation schema.
- Accepted reservation lifecycle and status model.
- Accepted event reservation status flow.
- Accepted venue reservation status flow.
- Accepted create/update/cancel/approve/reject behavior.
- Accepted reservation ownership model.
- Accepted notification behavior for reservations.
- Accepted public sharing and protected public/private visibility behavior for reservations.
- Accepted RPC contracts, including parameters, return shapes, errors, and authorization behavior.
- Accepted RLS policies.
- Accepted cross-surface ownership for mobile, dashboard, web/public, and Supabase backend.
- Accepted security-sensitive enforcement boundaries.
- Accepted maintainability ownership for routes, components, services, backend contracts, RLS, ownership, visibility, notifications, commerce, ticketing, wallet/ownership, venue/business tools, staff scanner/check-in, ops/admin, and public sharing.

Until these criteria are met, this document remains non-canonical.

## 23. Open Questions

- What is the accepted reservation schema?
- What is the accepted event reservation schema?
- What is the accepted venue reservation schema?
- What are the accepted reservation statuses and transitions?
- How do event reservation status flows differ from venue reservation status flows?
- Which create, update, cancel, approve, and reject behaviors are accepted for event reservations?
- Which create, update, cancel, approve, and reject behaviors are accepted for venue reservations?
- What is the accepted reservation ownership model?
- Which viewer roles can create, interact with, manage, approve, reject, or cancel reservations?
- How does reservation authority relate to personas and tiers?
- How does reservation behavior interact with event lifecycle?
- How does reservation behavior interact with commerce?
- How does reservation behavior interact with ticketing?
- How does reservation behavior interact with wallet/ownership?
- How does reservation behavior interact with venue/business tools?
- Where does media/gallery apply to reservations?
- Which reservation actions or status changes trigger notifications?
- Where do staff scanner or check-in workflows apply to reservations?
- What public/private reservation visibility rules are accepted?
- What reservation data can be publicly shared?
- What dashboard routes, components, and services own reservation tools?
- What mobile reservation behavior is accepted?
- Which surfaces support reservations today: mobile, dashboard, web/public, and Supabase backend?

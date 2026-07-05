# Staff Scanner

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is a platform-level draft specification for the JoinFolk Staff Scanner and Check-in System.

It records known staff scanner and check-in concepts, known risk areas, and areas that must be verified before any behavior is treated as accepted or canonical. It does not define exact staff scanner schema, check-in schema, staff assignment behavior, scanner permissions, check-in behavior, ticket queue behavior, RPC contracts, RLS policies, reservation relationships, notification behavior, dashboard ownership, or mobile behavior.

## 3. Staff Scanner System Definition

Staff scanner is a JoinFolk product domain or security-sensitive operational capability.

Known facts:

- Staff scanner behavior may interact with ticketing and check-in.
- Check-in authority is security-sensitive.
- Staff scanner authority is security-sensitive.
- Mobile may participate in staff scanner or scanner-related flows where applicable.
- Dashboard may support staff/scanner-related management.
- Ticketing behavior may interact with check-in and staff scanner.

Unknown / needs verification:

- The exact staff scanner schema.
- The exact check-in schema.
- The exact staff assignment model.
- The exact scanner permission model.
- The exact ticket queue and check-in behavior.
- The exact mobile and dashboard behavior.

## 4. Scanner Authority Model

### What frontend may own

Frontend surfaces may own user experience concerns, subject to backend/RPC/RLS enforcement for security-sensitive behavior.

Frontend-owned behavior may include:

- Scanner UI presentation.
- Ticket queue display UX.
- Check-in interaction UX.
- Local visual state.
- Client-side validation for usability.
- Loading, empty, and error states.
- Mobile scanner flow UX where applicable.
- Dashboard scanner management workflow composition.

Unknown / needs verification:

- Exact mobile scanner behavior.
- Exact dashboard routes for scanner management.
- Exact dashboard components for scanner management.
- Exact frontend service boundaries.
- Exact scanner UI and ticket queue display behavior.

### What backend/RPC/RLS must enforce

Backend, RPC, and RLS must enforce security-sensitive staff scanner and check-in behavior.

Security-sensitive behavior includes:

- Check-in authority.
- Staff scanner authority.
- Scanner permission authority.
- Staff assignment authority.
- Ticket queue access.
- Which viewer roles may access scanner-related flows.
- Which viewer roles may check in tickets.
- Which viewer roles may access ticket queue data.
- Which viewer roles may manage staff/scanner-related settings.
- Any scanner/check-in behavior that affects event lifecycle, ticketing, wallet/ownership, reservations, venue/business tools, media/gallery, notifications, ops/admin, or public sharing.

Unknown / needs verification:

- Exact scanner RLS policies.
- Exact scanner RPC authorization model.
- Exact accepted backend contracts.
- Exact accepted staff assignment model.
- Exact accepted scanner permission model.

### What must never be frontend-only

The following must never rely only on frontend checks:

- Check-in authority.
- Staff scanner authority.
- Staff assignment.
- Scanner permissions.
- Ticket queue access.
- Check-in state changes.
- Scanner relationship to ticket ownership.
- Scanner relationship to wallet ownership.
- Scanner relationship to reservations.
- Notification behavior triggered by scanner/check-in flows.
- Any operation involving viewer roles, personas, tiers, event lifecycle, ticketing, wallet/ownership, reservations, venue/business tools, media/gallery, notifications, ops/admin, or public sharing.

## 5. Known Scanner / Check-in Concepts Draft

Known scanner/check-in concepts:

- Staff scanner.
- Check-in.
- Staff assignment.
- Scanner permission model.
- Ticket queue.
- Ticket ownership relationship.
- Wallet ownership relationship.
- Reservation relationship where applicable.
- Scanner-related notifications.
- Mobile scanner or scanner-related flows where applicable.
- Dashboard staff/scanner-related management.

Known related product areas:

- Event lifecycle.
- Viewer roles.
- Personas and tiers.
- Ticketing.
- Wallet/ownership.
- Reservations where applicable.
- Venue/business tools.
- Media/gallery where applicable.
- Notifications.
- Ops/admin.
- Public sharing where applicable.

Unknown / needs verification:

- Exact domain vocabulary.
- Which concepts exist as persisted backend entities.
- Which concepts exist only as frontend UX.
- Which concepts are authoritative in backend/RPC/RLS.
- Whether staff scanner is modeled as a product domain, an operational capability, or both.

## 6. Known Schema / RPC Concept Names Draft

Prior audit/project context mentioned RPC or RPC-like names related to scanner/check-in. These names are known concepts only and are not accepted canonical RPC contracts until verified.

Known RPC/RPC-like concept names:

- `get_event_ticket_queue_v2`
- `checkin_ticket_by_id_v2`

Unknown / needs verification:

- Whether these RPC-like names exist in the accepted backend.
- Whether these names are current.
- Whether these names are exposed to frontend clients.
- Whether any staff scanner or check-in schema names exist.
- Exact table schemas.
- Exact RPC parameters.
- Exact RPC return shapes.
- Exact error behavior.
- Exact authorization behavior.
- Exact RLS behavior.

## 7. Non-Accepted Scanner Areas

The following areas are not accepted yet:

- Exact staff scanner schema.
- Exact check-in schema.
- Exact staff assignment model.
- Exact scanner permission model.
- Exact check-in behavior.
- Exact ticket queue behavior.
- Exact scanner RPC contracts.
- Exact scanner RLS policies.
- Exact scanner relationship to ticket ownership.
- Exact scanner relationship to wallet ownership.
- Exact scanner relationship to reservations.
- Exact scanner relationship to notifications.
- Exact dashboard route/component/service ownership for scanner management.
- Exact mobile scanner behavior.

These areas must remain Unknown / Needs verification until verified through accepted source material.

## 8. Staff Assignment Draft

Known facts:

- Staff scanner authority is security-sensitive.
- Exact staff assignment model is not accepted yet.
- Dashboard may support staff/scanner-related management.

Unknown / needs verification:

- Exact staff assignment behavior.
- Whether staff assignment is event-specific, venue-specific, role-specific, persona/tier-specific, or another model.
- Which viewer roles may assign, revoke, view, or manage scanner staff.
- Whether staff assignment affects ticket queue access, check-in authority, notifications, ops/admin workflows, or public sharing.
- Which backend/RPC/RLS rules enforce staff assignment authority.

No staff assignment behavior is accepted in this draft.

## 9. Ticket Queue Draft

Known facts:

- Prior audit/project context mentioned an RPC or RPC-like concept named `get_event_ticket_queue_v2`.
- Exact ticket queue behavior is not accepted yet.
- Staff scanner behavior may interact with ticketing and check-in.

Unknown / needs verification:

- Exact ticket queue behavior.
- Exact ticket queue schema or data model.
- Which viewer roles may access the ticket queue.
- Whether ticket queue behavior depends on event lifecycle, ticketing, wallet/ownership, reservations, venue/business tools, media/gallery, notifications, ops/admin, or public sharing.
- Whether ticket queue behavior differs across mobile, dashboard, Web/Public, or backend surfaces.
- Exact RPC contracts for ticket queue behavior.

No ticket queue behavior is accepted in this draft.

## 10. Check-in Draft

Known facts:

- Staff scanner behavior may interact with ticketing and check-in.
- Check-in authority is security-sensitive.
- Ticketing behavior may interact with check-in and staff scanner.
- Prior audit/project context mentioned an RPC or RPC-like concept named `checkin_ticket_by_id_v2`.

Unknown / needs verification:

- Exact check-in schema.
- Exact check-in behavior.
- Which viewer roles may check in tickets.
- Whether check-in changes ticket status, ownership, wallet state, reservation state, notification state, event lifecycle behavior, venue/business behavior, media/gallery behavior, ops/admin state, or public sharing behavior.
- Whether check-in behavior differs across mobile, dashboard, Web/Public, or backend surfaces.
- Exact RPC contracts for check-in behavior.
- Exact RLS policies for check-in behavior.

No check-in behavior is accepted in this draft.

## 11. Scanner Permission Draft

Known facts:

- Staff scanner authority is security-sensitive.
- Exact scanner permission model is not accepted yet.
- Backend/RPC/RLS must enforce security-sensitive staff scanner and check-in behavior.

Unknown / needs verification:

- Exact scanner permission model.
- Whether scanner permissions are based on viewer roles, staff assignments, personas and tiers, event lifecycle, venue/business tools, ops/admin rules, or another authority.
- Whether scanner permissions control ticket queue access, check-in action, scanner management, notification behavior, or public sharing behavior.
- Which backend/RPC/RLS behavior enforces scanner permissions.

No scanner permission behavior is accepted in this draft.

## 12. Ticket Ownership / Wallet Relationship Draft

Known facts:

- Staff scanner behavior may interact with ticketing.
- Staff scanner behavior may interact with wallet/ownership.
- Exact scanner relationship to ticket ownership is not accepted yet.
- Exact scanner relationship to wallet ownership is not accepted yet.

Unknown / needs verification:

- Exact scanner relationship to ticket ownership.
- Exact scanner relationship to wallet ownership.
- Whether ticket ownership or wallet ownership affects ticket queue visibility.
- Whether ticket ownership or wallet ownership affects check-in authority.
- Whether check-in affects ticket ownership or wallet ownership.
- Which backend/RPC/RLS behavior enforces ownership-related scanner rules.

No scanner relationship to ticket ownership or wallet ownership is accepted in this draft.

## 13. Reservation Relationship Draft

Known facts:

- Staff scanner behavior may interact with reservations where applicable.
- Reservation behavior may interact with staff scanner or check-in where applicable.
- Exact scanner relationship to reservations is not accepted yet.

Unknown / needs verification:

- Whether reservations are involved in scanner/check-in flows.
- Whether event reservations, venue reservations, or both may interact with scanner/check-in.
- Whether reservations affect ticket queue visibility, check-in authority, check-in behavior, notifications, ops/admin workflows, or public sharing.
- Whether check-in affects reservation state.
- Which backend/RPC/RLS behavior enforces reservation-related scanner rules.

No scanner relationship to reservations is accepted in this draft.

## 14. Notifications Relationship Draft

Known facts:

- Staff scanner behavior may interact with notifications.
- Exact scanner relationship to notifications is not accepted yet.

Unknown / needs verification:

- Whether scanner/check-in actions trigger notifications.
- Whether ticket queue changes trigger notifications.
- Whether staff assignment or scanner permission changes trigger notifications.
- Which recipients receive scanner-related notifications.
- Whether notification behavior differs by event lifecycle, viewer roles, personas and tiers, ticketing, wallet/ownership, reservations, venue/business tools, media/gallery, ops/admin, or public sharing.
- Which backend/RPC/RLS behavior enforces notification-related scanner rules.

No notification behavior for scanner/check-in is accepted in this draft.

## 15. Relationship to Product Domains

### Personas and tiers

Known relationship:

- Staff scanner behavior may interact with personas and tiers.

Unknown / needs verification:

- Whether personas or tiers affect staff assignment, scanner permissions, ticket queue access, check-in authority, notifications, ops/admin workflows, or public sharing.

### Event lifecycle

Known relationship:

- Staff scanner behavior may interact with event lifecycle.

Unknown / needs verification:

- Whether event lifecycle state affects staff assignment, scanner permissions, ticket queue access, check-in behavior, notifications, ops/admin workflows, or public sharing.

### Viewer roles

Known relationship:

- Staff scanner behavior may interact with viewer roles.

Unknown / needs verification:

- Exact viewer-role rules for staff assignment, scanner permissions, ticket queue access, check-in authority, scanner management, notifications, ops/admin workflows, and public sharing.

### Ticketing

Known relationship:

- Staff scanner behavior may interact with ticketing and check-in.
- Ticketing behavior may interact with check-in and staff scanner.

Unknown / needs verification:

- Exact ticketing relationship.
- Whether ticket state, ticket products, ticket ownership, or ticket queue data affect scanner/check-in behavior.

### Wallet/ownership

Known relationship:

- Staff scanner behavior may interact with wallet/ownership.

Unknown / needs verification:

- Exact wallet/ownership relationship.
- Whether wallet ownership affects ticket queue visibility, check-in authority, or scanner behavior.

### Reservations

Known relationship:

- Staff scanner behavior may interact with reservations where applicable.
- Reservation behavior may interact with staff scanner or check-in where applicable.

Unknown / needs verification:

- Exact reservation relationship.
- Whether reservations affect scanner/check-in behavior or scanner/check-in affects reservation behavior.

### Venue/business tools

Known relationship:

- Staff scanner behavior may interact with venue/business tools.

Unknown / needs verification:

- Exact venue/business relationship.
- Whether venue/business tools affect staff assignment, scanner permissions, ticket queue access, check-in behavior, notifications, ops/admin workflows, or public sharing.

### Media/gallery

Known relationship:

- Staff scanner behavior may interact with media/gallery where applicable.
- Media/gallery behavior may interact with staff scanner where applicable.

Unknown / needs verification:

- Whether media/gallery is displayed, consumed, gated, or modified by scanner/check-in flows.
- Whether media/gallery affects scanner permissions, ticket queue access, check-in behavior, notifications, ops/admin workflows, or public sharing.

### Notifications

Known relationship:

- Staff scanner behavior may interact with notifications.

Unknown / needs verification:

- Exact notification relationship.
- Which scanner/check-in actions, if any, trigger notifications.

### Ops/admin

Known relationship:

- Staff scanner behavior may interact with ops/admin.

Unknown / needs verification:

- Exact admin workflows.
- Exact operational override or support behavior.
- Exact auditability requirements.
- Whether admins can manage staff assignments, scanner permissions, ticket queues, or check-in state.

### Public sharing

Known relationship:

- Staff scanner behavior may interact with public sharing where applicable.

Unknown / needs verification:

- Exact public sharing relationship.
- Whether public sharing exposes scanner, ticket queue, check-in, staff assignment, permission, notification, ownership, reservation, or ops/admin information.

## 16. Cross-Surface Consistency Requirements

### Mobile

Known facts:

- Mobile may participate in staff scanner or scanner-related flows where applicable.
- Exact mobile scanner behavior is not accepted yet.

Unknown / needs verification:

- Whether mobile supports scanner UI, ticket queue display, check-in behavior, staff assignment, scanner permissions, scanner management, or notification behavior.
- Whether mobile consumes ticketing data, wallet/ownership data, reservation data, venue/business data, media/gallery data, notification data, ops/admin data, or public sharing data for scanner/check-in flows.
- Which mobile behavior must match dashboard, Web/Public, or backend behavior.

### Dashboard

Known facts:

- Dashboard may support staff/scanner-related management.
- Exact dashboard route/component/service ownership for scanner management is not accepted yet.

Unknown / needs verification:

- Exact dashboard route/component/service ownership for scanner management.
- Exact dashboard staff assignment behavior.
- Exact dashboard scanner permission behavior.
- Exact dashboard ticket queue behavior.
- Exact dashboard check-in behavior.
- Exact dashboard notification, ops/admin, reservation, venue/business, media/gallery, ownership, or public sharing behavior related to scanner/check-in flows.

### Web/Public

Known relationship:

- Staff scanner behavior may interact with public sharing where applicable.

Unknown / needs verification:

- Whether Web/Public participates in scanner/check-in behavior.
- Whether Web/Public exposes scanner, ticket queue, check-in, staff assignment, scanner permission, ownership, reservation, notification, ops/admin, or public sharing data.
- Exact public visibility rules for scanner/check-in data.

### Supabase Backend

Known requirement:

- Backend/RPC/RLS must enforce security-sensitive staff scanner and check-in behavior.

Unknown / needs verification:

- Exact schema.
- Exact RPC contracts.
- Exact RLS policies.
- Exact backend ownership boundaries.
- Exact enforcement model for staff assignment, scanner permissions, ticket queue access, check-in authority, ownership relationships, reservation relationships, notifications, ops/admin, and public sharing.

## 17. Security Risks

Known risks:

- Check-in authority is security-sensitive.
- Staff scanner authority is security-sensitive.
- Backend/RPC/RLS must enforce security-sensitive staff scanner and check-in behavior.
- Frontend scanner behavior is UX only where security-sensitive.

Security risks to verify:

- Unauthorized scanner access.
- Unauthorized ticket queue access.
- Unauthorized check-in.
- Unauthorized staff assignment changes.
- Unauthorized scanner permission changes.
- Unauthorized scanner management.
- Incorrect relationship to ticket ownership or wallet ownership.
- Incorrect relationship to reservations where applicable.
- Unauthorized scanner-related notification behavior.
- Unauthorized public exposure of scanner, queue, check-in, staff assignment, permission, ownership, reservation, or ops/admin data.
- Frontend-only checks being treated as enforcement.

## 18. Determinism Risks

Known determinism risks:

- Exact check-in behavior is not accepted yet.
- Exact ticket queue behavior is not accepted yet.
- Exact scanner permission model is not accepted yet.
- Exact scanner relationship to ticket ownership is not accepted yet.
- Exact scanner relationship to wallet ownership is not accepted yet.
- Exact scanner relationship to reservations is not accepted yet.

Risks to verify:

- Inconsistent check-in results across mobile, dashboard, Web/Public, and backend.
- Ticket queue data differing across surfaces.
- Different surfaces deriving scanner permissions differently.
- Ticket ownership or wallet ownership interpreted differently by scanner/check-in flows.
- Reservation relationships applied inconsistently where applicable.
- Scanner-related notifications triggered inconsistently.

## 19. Maintainability Risks

Known maintainability risks:

- Exact staff scanner schema is not accepted yet.
- Exact check-in schema is not accepted yet.
- Exact scanner RPC contracts are not accepted yet.
- Exact scanner RLS policies are not accepted yet.
- Exact dashboard route/component/service ownership for scanner management is not accepted yet.
- Exact mobile scanner behavior is not accepted yet.

Risks to verify:

- Scanner/check-in behavior scattered across surfaces without clear ownership.
- RPC-like names or schema-like names used as implicit contracts before verification.
- Frontend components encoding security-sensitive rules.
- Staff assignment, scanner permissions, ticket queue, check-in, ownership, reservation, notification, ops/admin, and public sharing logic duplicated without accepted ownership documentation.
- Scanner and ticketing behavior coupled without accepted authority boundaries.

## 20. Current Known Implementation

Current accepted implementation knowledge is limited to the facts in this draft:

- Staff scanner is a JoinFolk product domain or security-sensitive operational capability.
- Staff scanner behavior may interact with ticketing and check-in.
- Check-in authority is security-sensitive.
- Staff scanner authority is security-sensitive.
- Mobile may participate in staff scanner or scanner-related flows where applicable.
- Dashboard may support staff/scanner-related management.
- Ticketing behavior may interact with check-in and staff scanner.
- Reservation behavior may interact with staff scanner or check-in where applicable.
- Media/gallery behavior may interact with staff scanner where applicable.
- Prior context mentioned RPC or RPC-like scanner/check-in names, but none are accepted canonical contracts.

Unknown / needs verification:

- Exact accepted implementation across frontend, backend, RLS, dashboard, mobile, Web/Public, ticketing, wallet/ownership, reservations, venue/business tools, media/gallery, notifications, ops/admin, and public sharing surfaces.

## 21. Unknowns / Needs Verification

The following must be verified before v1.0:

- Exact staff scanner schema.
- Exact check-in schema.
- Exact staff assignment model.
- Exact scanner permission model.
- Exact check-in behavior.
- Exact ticket queue behavior.
- Exact scanner RPC contracts.
- Exact scanner RLS policies.
- Exact scanner relationship to ticket ownership.
- Exact scanner relationship to wallet ownership.
- Exact scanner relationship to reservations.
- Exact scanner relationship to notifications.
- Exact dashboard route/component/service ownership for scanner management.
- Exact mobile scanner behavior.
- Exact relationship between staff scanner and personas/tiers.
- Exact relationship between staff scanner and event lifecycle.
- Exact relationship between staff scanner and viewer roles.
- Exact relationship between staff scanner and ticketing.
- Exact relationship between staff scanner and venue/business tools.
- Exact relationship between staff scanner and media/gallery where applicable.
- Exact relationship between staff scanner and ops/admin.
- Exact relationship between staff scanner and public sharing where applicable.

## 22. Acceptance Criteria for v1.0

Staff Scanner v1.0 can be accepted only after verification establishes:

- Accepted staff scanner and check-in domain vocabulary.
- Accepted staff scanner schema.
- Accepted check-in schema.
- Accepted staff assignment model.
- Accepted scanner permission model.
- Accepted ticket queue behavior.
- Accepted check-in behavior.
- Accepted ticket ownership relationship.
- Accepted wallet ownership relationship.
- Accepted reservation relationship where applicable.
- Accepted notification relationship.
- Accepted public sharing relationship where applicable.
- Accepted RPC contracts, including parameters, return shapes, errors, and authorization behavior.
- Accepted RLS policies.
- Accepted cross-surface ownership for mobile, dashboard, Web/Public, and Supabase backend.
- Accepted security-sensitive enforcement boundaries.
- Accepted maintainability ownership for routes, components, services, backend contracts, RLS, staff assignment, scanner permissions, ticket queue, check-in, ownership, reservations, notifications, ops/admin, and public sharing.

Until these criteria are met, this document remains non-canonical.

## 23. Open Questions

- What is the accepted staff scanner schema?
- What is the accepted check-in schema?
- Is staff scanner an accepted product domain, an operational capability, or both?
- What is the accepted staff assignment model?
- What is the accepted scanner permission model?
- Which viewer roles can access scanner flows, ticket queues, and check-in actions?
- What is the accepted ticket queue behavior?
- What is the accepted check-in behavior?
- How does scanner/check-in behavior interact with ticket ownership?
- How does scanner/check-in behavior interact with wallet ownership?
- Where do reservations apply to scanner/check-in flows, if anywhere?
- Which scanner/check-in actions trigger notifications, if any?
- How does event lifecycle affect scanner/check-in behavior?
- How do personas and tiers affect scanner/check-in behavior?
- How do venue/business tools affect scanner/check-in behavior?
- Where does media/gallery apply to scanner/check-in flows, if anywhere?
- What ops/admin workflows are accepted for scanner/check-in behavior?
- What scanner/check-in data can be publicly shared, if any?
- What dashboard routes, components, and services own scanner management?
- What mobile scanner behavior is accepted?
- Which surfaces support staff scanner/check-in today: mobile, dashboard, Web/Public, and Supabase backend?

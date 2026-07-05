# Ticketing System

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is a platform-level draft specification for the JoinFolk Ticketing System.

It records known ticketing concepts, known risk areas, and areas that must be verified before any behavior is treated as accepted or canonical. It does not define exact ticketing schema, ticket product schema, ownership rules, lifecycle/status flows, purchase/request/claim/gift/transfer behavior, check-in behavior, staff scanner behavior, RPC contracts, RLS policies, product-section mapping behavior, or public sharing behavior.

## 3. Ticketing System Definition

The Ticketing System is the JoinFolk product domain for ticketing behavior.

Known facts:

- Ticketing is a JoinFolk product domain.
- Commerce model may include ticketing.
- Dashboard may support ticketing/product management.

Unknown / needs verification:

- The exact ticketing schema.
- The exact ticket product schema.
- The exact ticket lifecycle/status flow.
- The exact dashboard route, component, and service ownership for ticketing tools.
- The exact relationship between ticketing, commerce, reservations, wallet/ownership, check-in, staff scanner, venue/business tools, notifications, and public sharing.

## 4. Ticketing Authority Model

### What frontend may own

Frontend surfaces may own user experience concerns, subject to backend enforcement for security-sensitive behavior.

Frontend-owned behavior may include:

- Form presentation.
- Ticket/product management UX.
- Scanner UX.
- Local draft state.
- Client-side validation for usability.
- Loading, empty, and error states.
- Dashboard navigation and workflow composition.

Unknown / needs verification:

- Exact dashboard routes for ticketing/product management.
- Exact dashboard components for ticketing tools.
- Exact frontend service boundaries.
- Exact mobile, dashboard, web/public, and scanner UX behavior.

### What backend/RPC/RLS must enforce

Backend, RPC, and RLS must enforce security-sensitive ticketing behavior.

Security-sensitive behavior includes:

- Ticket ownership authority.
- Wallet ownership authority.
- Check-in authority.
- Staff scanner authority.
- Which viewer roles may access ticketing data.
- Which viewer roles may create, update, or manage ticket products.
- Which viewer roles may check in tickets.
- Which viewer roles may operate staff scanner behavior.
- Which viewer roles may access ticket queues.
- Any ticketing behavior that affects commerce, reservations, wallet/ownership, check-in, public sharing, notifications, or venue/business tools.

Unknown / needs verification:

- Exact ticketing RLS policies.
- Exact ticketing RPC authorization model.
- Exact accepted backend contracts.
- Exact accepted ownership authority model.

### What must never be frontend-only

The following must never rely only on frontend checks:

- Ticket ownership authority.
- Wallet ownership authority.
- Check-in authority.
- Staff scanner authority.
- Ticket lifecycle/status changes.
- Ticket product creation, update, or management.
- Purchase, request, claim, gift, or transfer behavior.
- Product-section mapping behavior.
- Public sharing visibility rules for ticketing.
- Any operation involving viewer roles, personas, tiers, commerce, reservations, venue/business tools, notifications, ownership, check-in, or staff scanner access.

## 5. Known Ticketing Concepts Draft

Known ticketing concepts:

- Tickets.
- Ticket products.
- Ticket ownership.
- Wallet ownership.
- Ticket lifecycle/status flow.
- Purchase/request/claim/gift/transfer concepts.
- Check-in.
- Staff scanner.
- Ticket/product-section mapping.
- Dashboard ticketing/product management.

Known related product areas:

- Event lifecycle.
- Viewer roles.
- Personas and tiers.
- Commerce.
- Reservations.
- Wallet/ownership.
- Check-in.
- Staff scanner.
- Venue/business tools.
- Product-section mapping.
- Notifications.
- Public sharing.

Unknown / needs verification:

- Exact domain vocabulary.
- Which concepts exist as persisted backend entities.
- Which concepts exist only as frontend UX.
- Which concepts are authoritative in backend/RPC/RLS.

## 6. Known Schema / RPC Concept Names Draft

Prior project context mentioned tables or concepts such as:

- `tickets`
- `event_ticket_products_v1`

Prior audit/project context mentioned RPC or RPC-like names related to ticketing. These names are known concepts only and are not accepted canonical schema or RPC contracts until verified.

Known RPC/RPC-like concept names:

- `get_event_ticket_queue_v2`
- `checkin_ticket_by_id_v2`
- `get_event_ticket_products_v1`
- `upsert_event_ticket_product_v2`
- `get_event_product_section_usage_v1`

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

## 7. Non-Accepted Ticketing Areas

The following areas are not accepted yet:

- Exact ticketing schema.
- Exact ticket product schema.
- Exact ticket ownership model.
- Exact wallet ownership model.
- Exact ticket lifecycle/status flow.
- Exact purchase/request/claim/gift/transfer behavior.
- Exact check-in behavior.
- Exact staff scanner behavior.
- Exact ticketing RPC contracts.
- Exact ticketing RLS policies.
- Exact product-section mapping behavior.
- Exact public sharing behavior for ticketing.
- Exact dashboard route/component/service ownership for ticketing tools.

These areas must remain Unknown / Needs verification until verified through accepted source material.

## 8. Ticket Lifecycle / Status Draft

Known facts:

- Ticketing is a JoinFolk product domain.
- Ticketing behavior may interact with event lifecycle.
- Ticketing behavior may interact with check-in and staff scanner.

Unknown / needs verification:

- Exact ticket lifecycle states.
- Exact ticket statuses.
- Whether ticket statuses are affected by purchase, request, claim, gift, transfer, reservation, wallet ownership, check-in, or staff scanner behavior.
- Whether ticket lifecycle state affects public sharing.
- Whether ticket lifecycle state affects commerce, reservations, notifications, or venue/business tools.
- Whether lifecycle/status is represented in frontend state, backend schema, RPC behavior, or RLS policies.

No exact ticket statuses are accepted in this draft.

## 9. Ticket Products Draft

Known facts:

- Dashboard may support ticketing/product management.
- Prior project context mentioned `event_ticket_products_v1`.
- Prior audit/project context mentioned RPC or RPC-like concepts for getting and upserting event ticket products.

Unknown / needs verification:

- Exact ticket product schema.
- Exact ticket product lifecycle.
- Exact ticket product management behavior.
- Whether ticket products are event-specific, venue-related, commerce-related, or shared across contexts.
- Which viewer roles may create, update, or manage ticket products.
- Whether ticket products interact with reservations, product-section mapping, public sharing, notifications, wallet/ownership, or check-in.
- Exact RPC contracts for ticket product reads or updates.

No exact ticket product behavior is accepted in this draft.

## 10. Ticket Ownership / Wallet Draft

Known facts:

- Ticket ownership authority is security-sensitive.
- Wallet ownership authority is security-sensitive.
- Ticketing behavior may interact with wallet/ownership.

Unknown / needs verification:

- Exact ticket ownership model.
- Exact wallet ownership model.
- Whether ticket ownership and wallet ownership are separate or overlapping authorities.
- Which backend/RPC/RLS behavior enforces ownership authority.
- Whether ownership changes through purchase, request, claim, gift, transfer, reservation, or check-in flows.
- Whether ownership is visible in mobile, dashboard, web/public, or staff scanner surfaces.

No exact ticket ownership or wallet ownership behavior is accepted in this draft.

## 11. Purchase / Request / Claim / Gift / Transfer Draft

Known facts:

- Exact purchase/request/claim/gift/transfer behavior is not accepted yet.
- Ticketing behavior may interact with commerce, reservations, wallet/ownership, notifications, and public sharing.

Unknown / needs verification:

- Whether purchase, request, claim, gift, or transfer flows exist as accepted product behavior.
- Exact behavior for purchase flows.
- Exact behavior for request flows.
- Exact behavior for claim flows.
- Exact behavior for gift flows.
- Exact behavior for transfer flows.
- Which viewer roles may initiate or complete each flow.
- Which backend/RPC/RLS rules enforce each flow.
- Whether these flows affect ticket lifecycle/status, ownership, wallet, reservations, notifications, commerce, check-in, staff scanner, or public sharing.

No purchase/request/claim/gift/transfer behavior is accepted in this draft.

## 12. Check-in / Staff Scanner Draft

Known facts:

- Check-in authority is security-sensitive.
- Staff scanner authority is security-sensitive.
- Ticketing behavior may interact with check-in and staff scanner.
- Prior audit/project context mentioned RPC or RPC-like concepts for ticket queue and check-in by ticket id.

Unknown / needs verification:

- Exact check-in behavior.
- Exact staff scanner behavior.
- Exact ticket queue behavior.
- Which viewer roles may check in tickets.
- Which viewer roles may use staff scanner behavior.
- Whether check-in affects ticket lifecycle/status, wallet/ownership, reservations, notifications, public sharing, commerce, or venue/business tools.
- Exact RPC contracts for ticket queue and check-in behavior.
- Exact RLS policies for ticket queue and check-in behavior.

No exact check-in or staff scanner behavior is accepted in this draft.

## 13. Ticket / Product-Section Mapping Relationship

Known facts:

- Dashboard audit found ticket/product-section mapping as a high-risk area.
- Ticketing behavior may interact with product-section mapping.
- Prior audit/project context mentioned an RPC or RPC-like concept named `get_event_product_section_usage_v1`.

Unknown / needs verification:

- Exact product-section mapping behavior.
- Whether product-section mapping is ticket-specific, event-specific, venue-specific, commerce-specific, or shared.
- Whether product-section mapping affects ticket availability, reservation availability, commerce, venue/business tools, check-in, staff scanner, or public sharing.
- Which system is authoritative for mapping validation.
- Which backend/RPC/RLS behavior enforces mapping rules.
- Which dashboard components expose mapping UX.

No exact product-section mapping behavior is accepted in this draft.

## 14. Relationship to Product Domains

### Personas and tiers

Known relationship:

- Ticketing behavior may interact with personas and tiers.

Unknown / needs verification:

- Whether personas or tiers affect ticket access, ticket product management, ownership, wallet behavior, purchase/request/claim/gift/transfer behavior, check-in, staff scanner, notifications, or public sharing.

### Event lifecycle

Known relationship:

- Ticketing behavior may interact with event lifecycle.

Unknown / needs verification:

- Whether event lifecycle state affects ticket products, ticket availability, ownership, reservations, purchase/request/claim/gift/transfer behavior, check-in, staff scanner, notifications, or public sharing.

### Viewer roles

Known relationship:

- Ticketing behavior may interact with viewer roles.

Unknown / needs verification:

- Exact viewer-role rules for ticket access, product management, ownership, wallet behavior, purchase/request/claim/gift/transfer behavior, check-in, staff scanner, reservations, notifications, and public sharing.

### Commerce

Known relationship:

- Commerce model may include ticketing.
- Ticketing behavior may interact with commerce.

Unknown / needs verification:

- Exact commerce relationship.
- Whether ticket products, ownership, purchase/request/claim/gift/transfer behavior, reservations, product-section mapping, or public sharing affect commerce.

### Reservations

Known relationship:

- Ticketing behavior may interact with reservations.

Unknown / needs verification:

- Exact relationship between ticketing and reservations.
- Whether reservations affect ticket lifecycle/status, ownership, purchase/request/claim/gift/transfer behavior, check-in, staff scanner, or public sharing.

### Wallet/ownership

Known relationship:

- Ticketing behavior may interact with wallet/ownership.
- Ticket ownership authority is security-sensitive.
- Wallet ownership authority is security-sensitive.

Unknown / needs verification:

- Exact ownership model.
- Exact wallet model.
- Exact authority boundary between tickets and wallet ownership.

### Venue/business tools

Known relationship:

- Ticketing behavior may interact with venue/business tools.
- Ticketing behavior may interact with product-section mapping.

Unknown / needs verification:

- Exact relationship between ticketing and venue/business tools.
- Whether venue/business tools affect ticket products, product-section mapping, reservations, check-in, staff scanner, commerce, or public sharing.

### Notifications

Known relationship:

- Ticketing behavior may interact with notifications.

Unknown / needs verification:

- Which ticketing flows trigger notifications.
- Whether notifications are triggered by ownership changes, purchase/request/claim/gift/transfer behavior, reservations, check-in, ticket product changes, or public sharing.

### Staff scanner

Known relationship:

- Ticketing behavior may interact with staff scanner.
- Staff scanner authority is security-sensitive.

Unknown / needs verification:

- Exact staff scanner authority model.
- Exact relationship between staff scanner, check-in, ticket queue, viewer roles, and event lifecycle.

### Public sharing

Known relationship:

- Ticketing behavior may interact with public sharing.

Unknown / needs verification:

- Exact public sharing behavior for ticketing.
- Whether public sharing exposes ticket products, availability, ownership information, reservation information, check-in information, commerce information, or venue/business information.

## 15. Cross-Surface Consistency Requirements

### Mobile

Unknown / needs verification:

- Whether mobile supports ticketing tools.
- Whether mobile consumes ticket data, ticket products, ownership/wallet data, reservation data, check-in data, scanner data, notifications, public sharing data, or product-section mapping data.
- Which mobile behavior must match dashboard, web/public, scanner, or backend behavior.

### Dashboard

Known facts:

- Dashboard may support ticketing/product management.
- Dashboard audit found ticket/product-section mapping as a high-risk area.

Unknown / needs verification:

- Exact dashboard route/component/service ownership for ticketing tools.
- Exact dashboard ticket product management behavior.
- Exact dashboard product-section mapping behavior.
- Exact dashboard reservation, ownership, check-in, staff scanner, notification, or public sharing behavior related to ticketing.

### Web/Public

Known relationship:

- Ticketing behavior may interact with public sharing.

Unknown / needs verification:

- Whether ticketing has public pages or public share views.
- Exact public visibility rules.
- Exact public ticket product, availability, ownership, reservation, commerce, venue/business, check-in, or scanner exposure.

### Supabase Backend

Known requirement:

- Backend/RPC/RLS must enforce security-sensitive ticketing behavior.

Unknown / needs verification:

- Exact schema.
- Exact RPC contracts.
- Exact RLS policies.
- Exact backend ownership boundaries.
- Exact enforcement model for ownership, wallet, check-in, staff scanner, and product-section mapping.

## 16. Security Risks

Known risks:

- Ticket/product-section mapping is high risk.
- Ticket ownership authority is security-sensitive.
- Wallet ownership authority is security-sensitive.
- Check-in authority is security-sensitive.
- Staff scanner authority is security-sensitive.
- Security-sensitive ticketing behavior must be enforced by backend/RPC/RLS, not frontend-only checks.

Security risks to verify:

- Unauthorized ticket access.
- Unauthorized ticket product creation or update.
- Unauthorized ticket ownership or wallet ownership changes.
- Unauthorized purchase/request/claim/gift/transfer behavior.
- Unauthorized check-in.
- Unauthorized staff scanner access.
- Unauthorized ticket queue access.
- Unauthorized public exposure of ticketing data.
- Commerce or reservation manipulation through ticketing behavior.
- Commerce or ticketing manipulation through product-section mapping.
- Dashboard-only checks being treated as enforcement.

## 17. Determinism Risks

Known determinism risks:

- Exact ticket lifecycle/status flow is not accepted yet.
- Exact purchase/request/claim/gift/transfer behavior is not accepted yet.
- Exact check-in behavior is not accepted yet.
- Exact staff scanner behavior is not accepted yet.
- Exact product-section mapping behavior is not accepted yet.

Risks to verify:

- Inconsistent ticket status interpretation across surfaces.
- Different surfaces deriving ownership authority differently.
- Wallet ownership diverging from ticket ownership.
- Check-in or staff scanner behavior producing inconsistent results across surfaces.
- Product-section mapping producing different results across dashboard, backend, commerce, reservations, venue/business tools, or public sharing surfaces.
- Notifications triggered inconsistently by ticketing behavior.

## 18. Maintainability Risks

Known maintainability risks:

- Dashboard route/component/service ownership for ticketing tools is not accepted yet.
- Exact ticketing RPC contracts are not accepted yet.
- Exact ticketing schema is not accepted yet.
- Exact product-section mapping behavior is not accepted yet.

Risks to verify:

- Ticketing behavior scattered across surfaces without clear ownership.
- RPC-like names or schema-like names used as implicit contracts before verification.
- Frontend components encoding security-sensitive rules.
- Ownership, wallet, check-in, scanner, ticket product, and product-section mapping logic duplicated across surfaces.
- Commerce, reservation, notification, or public sharing coupling without accepted ownership documentation.

## 19. Current Known Implementation

Current accepted implementation knowledge is limited to the facts in this draft:

- Ticketing is a JoinFolk product domain.
- Commerce model may include ticketing.
- Dashboard may support ticketing/product management.
- Dashboard audit found ticket/product-section mapping as a high-risk area.
- Ticket ownership authority is security-sensitive.
- Wallet ownership authority is security-sensitive.
- Check-in authority is security-sensitive.
- Staff scanner authority is security-sensitive.
- Prior project context mentioned tables or concepts such as `tickets` and `event_ticket_products_v1`.
- Prior context mentioned RPC or RPC-like ticketing names, but none are accepted canonical contracts.

Unknown / needs verification:

- Exact accepted implementation across frontend, backend, RLS, dashboard, mobile, web/public, check-in, staff scanner, wallet/ownership, commerce, reservations, notifications, and venue/business surfaces.

## 20. Unknowns / Needs Verification

The following must be verified before v1.0:

- Exact ticketing schema.
- Exact ticket product schema.
- Exact ticket ownership model.
- Exact wallet ownership model.
- Exact ticket lifecycle/status flow.
- Exact purchase/request/claim/gift/transfer behavior.
- Exact check-in behavior.
- Exact staff scanner behavior.
- Exact ticketing RPC contracts.
- Exact ticketing RLS policies.
- Exact product-section mapping behavior.
- Exact public sharing behavior for ticketing.
- Exact dashboard route/component/service ownership for ticketing tools.
- Exact relationship between ticketing and personas/tiers.
- Exact relationship between ticketing and event lifecycle.
- Exact relationship between ticketing and viewer roles.
- Exact relationship between ticketing and commerce.
- Exact relationship between ticketing and reservations.
- Exact relationship between ticketing and wallet/ownership.
- Exact relationship between ticketing and venue/business tools.
- Exact relationship between ticketing and notifications.

## 21. Acceptance Criteria for v1.0

Ticketing System v1.0 can be accepted only after verification establishes:

- Accepted ticketing domain vocabulary.
- Accepted ticketing schema.
- Accepted ticket product schema.
- Accepted ticket lifecycle and status model.
- Accepted ticket ownership model.
- Accepted wallet ownership model.
- Accepted purchase/request/claim/gift/transfer behavior.
- Accepted check-in behavior.
- Accepted staff scanner behavior.
- Accepted product-section mapping behavior.
- Accepted public sharing behavior for ticketing.
- Accepted RPC contracts, including parameters, return shapes, errors, and authorization behavior.
- Accepted RLS policies.
- Accepted cross-surface ownership for mobile, dashboard, web/public, staff scanner, and Supabase backend.
- Accepted security-sensitive enforcement boundaries.
- Accepted maintainability ownership for routes, components, services, backend contracts, RLS, ownership, check-in, scanner, commerce, reservations, notifications, and public sharing.

Until these criteria are met, this document remains non-canonical.

## 22. Open Questions

- What is the accepted ticketing schema?
- What is the accepted ticket product schema?
- What are the accepted ticket lifecycle statuses and transitions?
- What is the accepted ticket ownership model?
- What is the accepted wallet ownership model?
- How do ticket ownership and wallet ownership relate?
- Which purchase, request, claim, gift, or transfer flows are accepted product behavior?
- Which viewer roles can access tickets, manage ticket products, change ownership, check in tickets, or use staff scanner behavior?
- What is the accepted check-in behavior?
- What is the accepted staff scanner behavior?
- What is the accepted ticket queue behavior?
- What is the accepted ticket/product-section mapping behavior?
- Which backend/RPC/RLS rules enforce ticket/product-section mapping?
- How does ticketing interact with event lifecycle?
- How does ticketing interact with commerce?
- How does ticketing interact with reservations?
- How does ticketing interact with venue/business tools?
- Which ticketing flows trigger notifications?
- What ticketing data, products, availability, ownership, reservation, commerce, venue/business, check-in, or scanner information can be publicly shared?
- What dashboard routes, components, and services own ticketing/product management?
- Which surfaces support ticketing today: mobile, dashboard, web/public, staff scanner, and Supabase backend?

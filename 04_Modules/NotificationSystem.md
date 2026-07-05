# Notification System

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is a platform-level draft specification for the JoinFolk Notification System.

It records known notification concepts, known risk areas, and areas that must be verified before any behavior is treated as accepted or canonical. It does not define exact notification schema, trigger behavior, recipient model, read/unread behavior, delivery channels, in-app/push/email behavior, preferences behavior, retention/deletion behavior, RPC contracts, RLS policies, cross-surface behavior, dashboard ownership, mobile behavior, or public sharing notification behavior.

## 3. Notification System Definition

Notifications are a JoinFolk product domain.

Known facts:

- Security-sensitive notifications require backend authority.
- Backend/RPC/RLS must enforce security-sensitive notification behavior.
- Frontend notification display behavior is UX only where security-sensitive.
- Mobile may display or consume notifications where applicable.
- Dashboard may display or manage notifications where applicable.
- Web/Public may interact with notifications where applicable.

Unknown / needs verification:

- The exact notification schema.
- The exact notification trigger model.
- The exact recipient model.
- The exact read/unread behavior.
- The exact delivery channels.
- The exact cross-surface notification behavior.

## 4. Notification Authority Model

### What frontend may own

Frontend surfaces may own user experience concerns, subject to backend/RPC/RLS enforcement for security-sensitive behavior.

Frontend-owned behavior may include:

- Notification display UX.
- Notification list presentation.
- Local visual state.
- Client-side filtering or grouping for usability.
- Loading, empty, and error states.
- Mobile notification UX.
- Dashboard notification workflow composition.
- Web/Public notification-related UX where applicable.

Unknown / needs verification:

- Exact mobile notification behavior.
- Exact dashboard routes for notifications.
- Exact dashboard components for notifications.
- Exact frontend service boundaries.
- Exact Web/Public notification behavior.

### What backend/RPC/RLS must enforce

Backend, RPC, and RLS must enforce security-sensitive notification behavior.

Security-sensitive behavior includes:

- Notification creation authority.
- Trigger authority for security-sensitive product events.
- Recipient/audience authority.
- Notification visibility.
- Read/unread state authority where security-sensitive.
- Preference enforcement where security-sensitive.
- Retention/deletion authorization.
- Which viewer roles may view, update, manage, or delete notifications.
- Any notification behavior that affects personas and tiers, event lifecycle, commerce, ticketing, reservations, wallet/ownership, venue/business tools, media/gallery, staff scanner/check-in, ops/admin, host identity transfer, or public sharing.

Unknown / needs verification:

- Exact notification RLS policies.
- Exact notification RPC authorization model.
- Exact accepted backend contracts.
- Exact accepted trigger authority model.
- Exact accepted recipient/audience model.

### What must never be frontend-only

The following must never rely only on frontend checks:

- Security-sensitive notification creation.
- Security-sensitive trigger behavior.
- Recipient/audience selection for protected notifications.
- Notification visibility.
- Notification access control.
- Read/unread state where security-sensitive.
- Delivery decisions where security-sensitive.
- Preference enforcement where security-sensitive.
- Retention or deletion authorization.
- Any operation involving viewer roles, personas, tiers, commerce, ticketing, reservations, wallet/ownership, venue/business tools, media/gallery, staff scanner/check-in, ops/admin, host identity transfer, or public sharing.

## 5. Known Notification Concepts Draft

Known notification concepts:

- Notifications.
- Security-sensitive notifications.
- Trigger model.
- Recipient/audience model.
- Read/unread state.
- Delivery channels.
- In-app behavior.
- Push behavior.
- Email behavior.
- Notification preferences.
- Notification retention/deletion.
- Cross-surface notification behavior.

Known related product areas:

- Personas and tiers.
- Event lifecycle.
- Viewer roles.
- Commerce.
- Ticketing.
- Reservations.
- Wallet/ownership.
- Venue/business tools.
- Media/gallery.
- Staff scanner or check-in where applicable.
- Ops/admin.
- Host identity transfer.
- Public sharing.

Unknown / needs verification:

- Exact domain vocabulary.
- Which concepts exist as persisted backend entities.
- Which concepts exist only as frontend UX.
- Which concepts are authoritative in backend/RPC/RLS.
- Which delivery channels are accepted.

## 6. Known Schema / RPC Concept Names Draft

Prior project context mentioned tables or concepts such as:

- `notifications_v1`

This name is a known concept only and is not an accepted canonical schema until verified.

Unknown / needs verification:

- Whether `notifications_v1` exists in the accepted backend.
- Whether `notifications_v1` is current.
- Whether any notification RPC names exist.
- Whether any notification RPC names are exposed to frontend clients.
- Exact table schemas.
- Exact RPC parameters.
- Exact RPC return shapes.
- Exact error behavior.
- Exact authorization behavior.
- Exact RLS behavior.

## 7. Non-Accepted Notification Areas

The following areas are not accepted yet:

- Exact notification schema.
- Exact notification trigger model.
- Exact recipient model.
- Exact read/unread behavior.
- Exact delivery channels.
- Exact in-app/push/email behavior.
- Exact notification preferences behavior.
- Exact notification retention/deletion behavior.
- Exact notification RPC contracts.
- Exact notification RLS policies.
- Exact cross-surface notification behavior.
- Exact dashboard route/component/service ownership for notifications.
- Exact mobile notification behavior.
- Exact public sharing notification behavior.

These areas must remain Unknown / Needs verification until verified through accepted source material.

## 8. Trigger Model Draft

Known facts:

- Exact notification trigger model is not accepted yet.
- Security-sensitive notifications require backend authority.
- Notification behavior may interact with multiple product domains.

Unknown / needs verification:

- Exact trigger behavior.
- Which product events trigger notifications.
- Whether triggers are synchronous, asynchronous, user-initiated, system-initiated, or admin-initiated.
- Whether triggers differ across mobile, dashboard, Web/Public, or backend surfaces.
- Whether triggers are affected by personas and tiers, viewer roles, event lifecycle, commerce, ticketing, reservations, wallet/ownership, venue/business tools, media/gallery, staff scanner/check-in, ops/admin, host identity transfer, or public sharing.
- Which backend/RPC/RLS rules enforce trigger authority.

No exact notification trigger behavior is accepted in this draft.

## 9. Recipient / Audience Draft

Known facts:

- Exact recipient model is not accepted yet.
- Security-sensitive notifications require backend authority.

Unknown / needs verification:

- Exact recipient model.
- Exact audience model.
- Whether recipients are users, roles, personas, tiers, owners, hosts, staff, admins, public viewers, or other concepts.
- Whether recipient selection is affected by event lifecycle, commerce, ticketing, reservations, wallet/ownership, venue/business tools, media/gallery, staff scanner/check-in, ops/admin, host identity transfer, or public sharing.
- Which backend/RPC/RLS rules enforce recipient/audience authority.

No exact recipient or audience behavior is accepted in this draft.

## 10. Read / Unread / State Draft

Known facts:

- Exact read/unread behavior is not accepted yet.

Unknown / needs verification:

- Exact read/unread behavior.
- Whether notifications have other states.
- Whether read/unread state is per recipient, per notification, per device, or another model.
- Whether read/unread state affects visibility, delivery, retention, deletion, preferences, or public sharing.
- Which surfaces may display or change notification state.
- Which backend/RPC/RLS behavior enforces state changes where security-sensitive.

No read/unread or notification state behavior is accepted in this draft.

## 11. Delivery Channel Draft

Known facts:

- Exact delivery channels are not accepted yet.
- Exact in-app/push/email behavior is not accepted yet.
- Mobile may display or consume notifications where applicable.
- Dashboard may display or manage notifications where applicable.
- Web/Public may interact with notifications where applicable.

Unknown / needs verification:

- Exact delivery channels.
- Exact in-app behavior.
- Exact push behavior.
- Exact email behavior.
- Whether delivery differs by surface.
- Whether delivery is affected by notification preferences.
- Whether delivery is affected by personas and tiers, viewer roles, public/private visibility, event lifecycle, commerce, ticketing, reservations, wallet/ownership, venue/business tools, media/gallery, staff scanner/check-in, ops/admin, host identity transfer, or public sharing.
- Which backend/RPC/RLS behavior enforces security-sensitive delivery decisions.

No delivery channel, in-app, push, or email behavior is accepted in this draft.

## 12. Preferences Draft

Known facts:

- Exact notification preferences behavior is not accepted yet.

Unknown / needs verification:

- Whether notification preferences exist as accepted product behavior.
- Exact preferences behavior.
- Whether preferences apply by user, role, persona, tier, event, venue, product domain, delivery channel, or another model.
- Whether preferences can suppress security-sensitive notifications.
- Which surfaces expose preference UX.
- Which backend/RPC/RLS behavior enforces preferences where security-sensitive.

No notification preferences behavior is accepted in this draft.

## 13. Retention / Deletion Draft

Known facts:

- Exact notification retention/deletion behavior is not accepted yet.

Unknown / needs verification:

- Exact retention behavior.
- Exact deletion behavior.
- Whether retention/deletion differs by notification type, recipient, surface, delivery channel, product domain, ops/admin behavior, or public sharing.
- Which viewer roles may delete or manage notifications.
- Whether deletion affects notification state, delivery records, auditability, or public visibility.
- Which backend/RPC/RLS behavior enforces retention/deletion authorization.

No notification retention or deletion behavior is accepted in this draft.

## 14. Public Sharing Notification Draft

Known facts:

- Notification behavior may interact with public sharing.
- Exact public sharing notification behavior is not accepted yet.
- Web/Public may interact with notifications where applicable.

Unknown / needs verification:

- Exact public sharing notification behavior.
- Whether public sharing can create, display, suppress, or consume notifications.
- Whether notifications are visible on public surfaces.
- Whether public sharing affects recipient/audience selection.
- Whether public sharing affects delivery channels, read/unread state, preferences, retention, or deletion.
- Which backend/RPC/RLS behavior enforces public sharing notification rules.

No public sharing notification behavior is accepted in this draft.

## 15. Relationship to Product Domains

### Personas and tiers

Known relationship:

- Notification behavior may interact with personas and tiers.

Unknown / needs verification:

- Whether personas or tiers affect triggers, recipients, delivery, visibility, preferences, read/unread state, retention, deletion, or public sharing.

### Event lifecycle

Known relationship:

- Notification behavior may interact with event lifecycle.

Unknown / needs verification:

- Whether event lifecycle state affects notification triggers, recipients, delivery, visibility, preferences, retention, deletion, ops/admin behavior, or public sharing.

### Viewer roles

Known relationship:

- Notification behavior may interact with viewer roles.

Unknown / needs verification:

- Exact viewer-role rules for notification access, display, creation, triggering, recipient selection, management, deletion, preferences, ops/admin behavior, and public sharing.

### Commerce

Known relationship:

- Notification behavior may interact with commerce.

Unknown / needs verification:

- Whether commerce events trigger notifications.
- Whether commerce state affects recipients, delivery, preferences, retention, deletion, or public sharing.

### Ticketing

Known relationship:

- Notification behavior may interact with ticketing.

Unknown / needs verification:

- Whether ticketing events trigger notifications.
- Whether ticketing state affects recipients, delivery, preferences, retention, deletion, wallet/ownership, staff scanner/check-in, or public sharing.

### Reservations

Known relationship:

- Notification behavior may interact with reservations.

Unknown / needs verification:

- Whether reservation events trigger notifications.
- Whether reservation state affects recipients, delivery, preferences, retention, deletion, ops/admin behavior, or public sharing.

### Wallet/ownership

Known relationship:

- Notification behavior may interact with wallet/ownership.

Unknown / needs verification:

- Whether wallet/ownership events trigger notifications.
- Whether ownership state affects recipients, delivery, preferences, retention, deletion, ticketing, reservations, or public sharing.

### Venue/business tools

Known relationship:

- Notification behavior may interact with venue/business tools.

Unknown / needs verification:

- Whether venue/business events trigger notifications.
- Whether venue/business state affects recipients, delivery, preferences, retention, deletion, ops/admin behavior, reservations, ticketing, media/gallery, or public sharing.

### Media/gallery

Known relationship:

- Notification behavior may interact with media/gallery.

Unknown / needs verification:

- Whether media/gallery events trigger notifications.
- Whether media/gallery state affects recipients, delivery, preferences, retention, deletion, moderation, ops/admin behavior, or public sharing.

### Staff scanner / check-in

Known relationship:

- Notification behavior may interact with staff scanner or check-in where applicable.

Unknown / needs verification:

- Whether staff scanner/check-in events trigger notifications.
- Whether staff scanner/check-in authority or state affects recipients, delivery, preferences, retention, deletion, ticketing, reservations, ops/admin behavior, or public sharing.

### Ops/admin

Known relationship:

- Notification behavior may interact with ops/admin.

Unknown / needs verification:

- Exact admin workflows.
- Exact operational override or support behavior.
- Exact auditability requirements.
- Whether admins can create, suppress, resend, delete, retain, or manage notifications.

### Host identity transfer

Known relationship:

- Notification behavior may interact with host identity transfer.

Unknown / needs verification:

- Whether host identity transfer triggers notifications.
- Whether host identity transfer affects recipients, delivery, visibility, preferences, retention, deletion, event lifecycle, viewer roles, ops/admin behavior, or public sharing.

### Public sharing

Known relationship:

- Notification behavior may interact with public sharing.

Unknown / needs verification:

- Exact public sharing notification behavior.
- Whether public sharing creates, displays, consumes, suppresses, or changes notification behavior.

## 16. Cross-Surface Consistency Requirements

### Mobile

Known facts:

- Mobile may display or consume notifications where applicable.
- Exact mobile notification behavior is not accepted yet.

Unknown / needs verification:

- Whether mobile supports notification list display, detail display, read/unread state, preferences, delivery channel controls, or public sharing notification behavior.
- Whether mobile consumes notification data from personas and tiers, event lifecycle, commerce, ticketing, reservations, wallet/ownership, venue/business tools, media/gallery, staff scanner/check-in, ops/admin, host identity transfer, or public sharing.
- Which mobile behavior must match dashboard, Web/Public, or backend behavior.

### Dashboard

Known facts:

- Dashboard may display or manage notifications where applicable.
- Exact dashboard route/component/service ownership for notifications is not accepted yet.

Unknown / needs verification:

- Exact dashboard route/component/service ownership for notifications.
- Exact dashboard notification display behavior.
- Exact dashboard notification management behavior.
- Exact dashboard trigger, recipient, read/unread, delivery, preference, retention, deletion, public sharing, ops/admin, or host identity transfer behavior related to notifications.

### Web/Public

Known facts:

- Web/Public may interact with notifications where applicable.
- Exact public sharing notification behavior is not accepted yet.

Unknown / needs verification:

- Whether Web/Public supports notification display, consumption, public sharing notification behavior, or notification-triggering behavior.
- Exact public visibility rules for notifications.
- Exact Web/Public relationship to delivery channels, recipients, read/unread state, preferences, retention, deletion, or public sharing.

### Supabase Backend

Known requirement:

- Backend/RPC/RLS must enforce security-sensitive notification behavior.

Unknown / needs verification:

- Exact schema.
- Exact RPC contracts.
- Exact RLS policies.
- Exact backend ownership boundaries.
- Exact enforcement model for triggers, recipients, visibility, read/unread state, delivery decisions, preferences, retention, deletion, and public sharing.

## 17. Security Risks

Known risks:

- Security-sensitive notifications require backend authority.
- Backend/RPC/RLS must enforce security-sensitive notification behavior.
- Frontend notification display behavior is UX only where security-sensitive.

Security risks to verify:

- Unauthorized notification access.
- Unauthorized notification creation or triggering.
- Unauthorized recipient/audience selection.
- Unauthorized read/unread state changes where security-sensitive.
- Unauthorized notification preference changes where security-sensitive.
- Unauthorized retention or deletion behavior.
- Unauthorized public exposure of notifications.
- Notification leakage across viewer roles, personas, tiers, ownership, events, commerce, ticketing, reservations, venue/business tools, media/gallery, staff scanner/check-in, ops/admin, host identity transfer, or public sharing contexts.
- Frontend-only checks being treated as enforcement.

## 18. Determinism Risks

Known determinism risks:

- Exact notification trigger model is not accepted yet.
- Exact recipient model is not accepted yet.
- Exact read/unread behavior is not accepted yet.
- Exact delivery channels are not accepted yet.
- Exact cross-surface notification behavior is not accepted yet.

Risks to verify:

- Inconsistent trigger behavior across product domains.
- Different surfaces deriving recipients differently.
- Read/unread state diverging across mobile, dashboard, Web/Public, and backend.
- Delivery channel behavior differing across surfaces.
- Preferences being applied inconsistently.
- Retention or deletion behavior producing different visible results across surfaces.
- Public sharing changing notification visibility inconsistently.

## 19. Maintainability Risks

Known maintainability risks:

- Exact notification schema is not accepted yet.
- Exact notification RPC contracts are not accepted yet.
- Exact notification RLS policies are not accepted yet.
- Exact dashboard route/component/service ownership for notifications is not accepted yet.
- Exact mobile notification behavior is not accepted yet.

Risks to verify:

- Notification behavior scattered across surfaces without clear ownership.
- Schema-like names used as implicit contracts before verification.
- Frontend components encoding security-sensitive rules.
- Trigger, recipient, read/unread, delivery, preference, retention, deletion, public sharing, and ops/admin logic duplicated without accepted ownership documentation.
- Product-domain notification behavior coupled without accepted trigger and recipient contracts.

## 20. Current Known Implementation

Current accepted implementation knowledge is limited to the facts in this draft:

- Notifications are a JoinFolk product domain.
- Security-sensitive notifications require backend authority.
- Backend/RPC/RLS must enforce security-sensitive notification behavior.
- Frontend notification display behavior is UX only where security-sensitive.
- Mobile may display or consume notifications where applicable.
- Dashboard may display or manage notifications where applicable.
- Web/Public may interact with notifications where applicable.
- Prior project context mentioned tables or concepts such as `notifications_v1`, but this is not an accepted canonical schema.

Unknown / needs verification:

- Exact accepted implementation across frontend, backend, RLS, dashboard, mobile, Web/Public, personas and tiers, event lifecycle, commerce, ticketing, reservations, wallet/ownership, venue/business tools, media/gallery, staff scanner/check-in, ops/admin, host identity transfer, and public sharing surfaces.

## 21. Unknowns / Needs Verification

The following must be verified before v1.0:

- Exact notification schema.
- Exact notification trigger model.
- Exact recipient model.
- Exact read/unread behavior.
- Exact delivery channels.
- Exact in-app/push/email behavior.
- Exact notification preferences behavior.
- Exact notification retention/deletion behavior.
- Exact notification RPC contracts.
- Exact notification RLS policies.
- Exact cross-surface notification behavior.
- Exact dashboard route/component/service ownership for notifications.
- Exact mobile notification behavior.
- Exact public sharing notification behavior.
- Exact relationship between notifications and personas/tiers.
- Exact relationship between notifications and event lifecycle.
- Exact relationship between notifications and viewer roles.
- Exact relationship between notifications and commerce.
- Exact relationship between notifications and ticketing.
- Exact relationship between notifications and reservations.
- Exact relationship between notifications and wallet/ownership.
- Exact relationship between notifications and venue/business tools.
- Exact relationship between notifications and media/gallery.
- Exact relationship between notifications and staff scanner/check-in where applicable.
- Exact relationship between notifications and ops/admin.
- Exact relationship between notifications and host identity transfer.
- Exact relationship between notifications and public sharing.

## 22. Acceptance Criteria for v1.0

Notification System v1.0 can be accepted only after verification establishes:

- Accepted notification domain vocabulary.
- Accepted notification schema.
- Accepted notification trigger model.
- Accepted recipient/audience model.
- Accepted read/unread and notification state behavior.
- Accepted delivery channels.
- Accepted in-app, push, and email behavior where applicable.
- Accepted notification preferences behavior.
- Accepted notification retention/deletion behavior.
- Accepted public sharing notification behavior.
- Accepted RPC contracts, including parameters, return shapes, errors, and authorization behavior.
- Accepted RLS policies.
- Accepted cross-surface ownership for mobile, dashboard, Web/Public, and Supabase backend.
- Accepted security-sensitive enforcement boundaries.
- Accepted maintainability ownership for routes, components, services, backend contracts, RLS, triggers, recipients, state, delivery, preferences, retention, deletion, ops/admin, host identity transfer, and public sharing.

Until these criteria are met, this document remains non-canonical.

## 23. Open Questions

- What is the accepted notification schema?
- Is `notifications_v1` an accepted backend concept, and if so, what is its accepted contract?
- What is the accepted notification trigger model?
- Which product-domain events trigger notifications?
- What is the accepted recipient/audience model?
- What read/unread or notification state behavior is accepted?
- Which delivery channels are accepted?
- What in-app, push, or email behavior is accepted?
- What notification preferences behavior is accepted?
- Can preferences suppress security-sensitive notifications?
- What retention and deletion behavior is accepted?
- Which viewer roles can view, create, trigger, manage, or delete notifications?
- How do personas and tiers affect notifications?
- How does event lifecycle affect notifications?
- How does commerce affect notifications?
- How does ticketing affect notifications?
- How do reservations affect notifications?
- How does wallet/ownership affect notifications?
- How do venue/business tools affect notifications?
- How does media/gallery affect notifications?
- Where do staff scanner or check-in workflows affect notifications, if anywhere?
- How does host identity transfer affect notifications?
- What public sharing notification behavior is accepted?
- What dashboard routes, components, and services own notifications?
- What mobile notification behavior is accepted?
- Which surfaces support notifications today: mobile, dashboard, Web/Public, and Supabase backend?

# Messaging

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is a platform-level draft specification for JoinFolk messaging concepts.

This is a handbook draft. It is not a code audit and is not an accepted implementation contract. Messaging behavior is security-sensitive where private conversation, participant-only communication, staff/host communication, or operational messages may be exposed.

Backend/RPC/RLS must enforce security-sensitive messaging behavior. Frontend messaging behavior is UX only where security-sensitive. Prior implementation notes must not be treated as canonical.

## 3. Messaging System Definition

JoinFolk has or may have messaging concepts.

Known facts:

- JoinFolk has notifications concepts.
- JoinFolk has event participation concepts.
- JoinFolk has viewer roles.
- JoinFolk has personas and tiers.
- JoinFolk has ticketing and reservations.
- JoinFolk has staff/scanner concepts.
- JoinFolk has host/event ownership concepts.

Possible messaging domains:

- Direct user-to-user communication.
- Host-to-participant communication.
- Participant-to-host communication.
- Event-level communication.
- Reservation/ticket-related communication.
- Staff/ops/admin communication.

These are possible messaging domains only and are not accepted behavior until verified.

Unknown / needs verification:

- Whether messaging exists as an accepted JoinFolk product domain.
- Exact messaging schema.
- Exact conversation/thread model.
- Exact visibility, access, realtime, delivery, read state, notification, moderation, and attachment behavior.

## 4. Authority Model

### What frontend may own

Frontend surfaces may own user experience concerns, subject to backend/RPC/RLS enforcement for security-sensitive behavior.

Frontend-owned behavior may include:

- Conversation list presentation.
- Message thread presentation.
- Composer UX.
- Attachment display UX where applicable.
- Loading, empty, and error states.
- Local draft text state.
- Client-side formatting for usability.

Unknown / needs verification:

- Exact mobile messaging behavior.
- Exact Web/Public messaging behavior.
- Exact Dashboard/Ops messaging behavior.
- Exact frontend route/component/service ownership.
- Exact realtime or delivery UX.

### What backend/RPC/RLS must enforce

Backend, RPC, and RLS must enforce security-sensitive messaging behavior.

Security-sensitive behavior includes:

- Conversation and message access.
- Participant-only visibility.
- Host/staff/admin access where applicable.
- Event-level message visibility.
- Reservation/ticket-related message visibility.
- Attachment/media access where applicable.
- Realtime subscription authorization where applicable.
- Notification integration where security-sensitive.
- Moderation/ops access where applicable.
- Any messaging behavior affecting users, profiles, personas, viewer roles, event lifecycle, event participation, event ownership, ticketing, reservations, wallet/ownership, staff/scanner, host tools, venue/business tools, public sharing, or Supabase backend authority.

Unknown / needs verification:

- Exact messaging RPC contracts.
- Exact messaging RLS policies.
- Exact realtime authorization model.
- Exact participant, host, staff, and admin access model.

### What must never be frontend-only

The following must never rely only on frontend checks:

- Private conversation access.
- Participant-only communication access.
- Host/staff/admin communication access.
- Event-level message visibility.
- Reservation/ticket-related message visibility.
- Message delivery to authorized recipients.
- Read receipt or delivery state where security-sensitive.
- Realtime channel access.
- Attachment/media access.
- Notification integration where security-sensitive.
- Moderation/ops access or actions.
- Any messaging behavior involving protected user, profile, event, ticketing, reservation, ownership, staff, host, venue/business, or public sharing data.

## 5. Known Messaging Concepts Draft

Known messaging concepts:

- Messaging.
- Conversations.
- Threads.
- Messages.
- Direct user-to-user communication.
- Host-to-participant communication.
- Participant-to-host communication.
- Event-level communication.
- Reservation/ticket-related communication.
- Staff/ops/admin communication.
- Realtime behavior.
- Delivery state.
- Read receipt behavior.
- Message visibility.
- Participant access.
- Host/staff/admin access.
- Attachments/media where applicable.
- Notification integration.
- Moderation/ops behavior.

Known related product areas:

- Users and profiles.
- Personas and tiers.
- Viewer roles.
- Event lifecycle.
- Event participation.
- Event ownership.
- Ticketing.
- Reservations.
- Wallet/ownership where applicable.
- Staff/scanner where applicable.
- Host tools.
- Venue/business tools.
- Notifications.
- Media/gallery where applicable.
- Ops/admin/moderation.
- Public sharing where applicable.
- Supabase backend/RPC/RLS where security-sensitive.

Unknown / needs verification:

- Exact messaging vocabulary.
- Which concepts exist as persisted backend entities.
- Which concepts exist only as frontend UX.
- Which concepts are authoritative in backend/RPC/RLS.

## 6. Known Schema / RPC / Realtime Concept Names Draft

No accepted schema, table, RPC, realtime channel, route, or component names are defined in this draft.

Unknown / needs verification:

- Whether messaging schema names exist.
- Whether messaging table names exist.
- Whether messaging RPC names exist.
- Whether realtime channel names exist.
- Whether dashboard, mobile, or Web/Public route/component names exist.
- Exact RPC parameters.
- Exact RPC return shapes.
- Exact realtime authorization behavior.
- Exact RLS behavior.

No schema, table, RPC, realtime channel, route, or component name is canonical in this draft.

## 7. Non-Accepted Messaging Areas

The following areas are not accepted yet:

- Exact messaging schema.
- Exact messaging RPC contracts.
- Exact messaging RLS policies.
- Exact realtime behavior.
- Exact delivery/read receipt behavior.
- Exact conversation/thread model.
- Exact message visibility behavior.
- Exact participant access behavior.
- Exact host/staff/admin access behavior.
- Exact attachment/media behavior.
- Exact notification integration behavior.
- Exact moderation/ops behavior.
- Exact mobile messaging behavior.
- Exact web/public messaging behavior.
- Exact dashboard/ops messaging behavior.

These areas must remain Unknown / Needs verification until verified through accepted source material.

## 8. Conversation / Thread Draft

Known concepts:

- Conversations.
- Threads.
- Messages.
- Direct user-to-user communication.
- Event-level communication.
- Reservation/ticket-related communication.

Unknown / needs verification:

- Exact conversation/thread model.
- Whether conversations and threads are separate concepts.
- Whether threads are direct, event-level, reservation-related, ticket-related, staff/admin, or another model.
- Whether threads are scoped to users, profiles, events, reservations, tickets, host tools, venue/business tools, or ops/admin workflows.
- Which backend/RPC/RLS behavior enforces conversation/thread access.

No exact conversation or thread behavior is accepted in this draft.

## 9. Message Visibility / Access Draft

Known facts:

- Messaging behavior is security-sensitive where private conversation, participant-only communication, staff/host communication, or operational messages may be exposed.
- Backend/RPC/RLS must enforce security-sensitive messaging behavior.

Unknown / needs verification:

- Exact message visibility behavior.
- Exact participant access behavior.
- Exact host/staff/admin access behavior.
- Whether visibility depends on viewer roles, personas and tiers, event participation, event ownership, ticketing, reservations, wallet/ownership, host tools, venue/business tools, ops/admin/moderation, or public sharing.
- Which backend/RPC/RLS behavior enforces message visibility and access.

No exact message visibility or access behavior is accepted in this draft.

## 10. Event-Level Messaging Draft

Known facts:

- JoinFolk has event participation concepts.
- JoinFolk has host/event ownership concepts.
- Messaging may exist as event-level communication.

Unknown / needs verification:

- Whether event-level messaging exists as accepted behavior.
- Exact event-level messaging behavior.
- Whether event-level messaging depends on event lifecycle, event participation, event ownership, viewer roles, personas and tiers, ticketing, reservations, staff/scanner, notifications, moderation/ops, or public sharing.
- Which backend/RPC/RLS behavior enforces event-level messaging.

No event-level messaging behavior is accepted in this draft.

## 11. Host / Participant Messaging Draft

Known facts:

- Messaging may exist as host-to-participant communication.
- Messaging may exist as participant-to-host communication.
- JoinFolk has host/event ownership concepts.
- JoinFolk has event participation concepts.

Unknown / needs verification:

- Whether host-to-participant messaging exists as accepted behavior.
- Whether participant-to-host messaging exists as accepted behavior.
- Exact host/participant access behavior.
- Whether access depends on event ownership, event participation, viewer roles, personas and tiers, ticketing, reservations, wallet/ownership, staff/scanner, notifications, or ops/admin/moderation.
- Which backend/RPC/RLS behavior enforces host/participant messaging.

No host/participant messaging behavior is accepted in this draft.

## 12. Ticketing / Reservation Messaging Draft

Known facts:

- JoinFolk has ticketing and reservations.
- Messaging may exist as reservation/ticket-related communication.

Unknown / needs verification:

- Whether ticket-related messaging exists as accepted behavior.
- Whether reservation-related messaging exists as accepted behavior.
- Exact ticketing/reservation messaging behavior.
- Whether access depends on ticket ownership, reservation ownership, wallet/ownership, event participation, event lifecycle, viewer roles, personas and tiers, host tools, venue/business tools, notifications, or ops/admin/moderation.
- Which backend/RPC/RLS behavior enforces ticketing/reservation messaging.

No ticketing or reservation messaging behavior is accepted in this draft.

## 13. Staff / Scanner / Ops Messaging Draft

Known facts:

- JoinFolk has staff/scanner concepts.
- Messaging may exist as staff/ops/admin communication.
- Messaging may interact with staff/scanner where applicable.

Unknown / needs verification:

- Whether staff/scanner messaging exists as accepted behavior.
- Whether staff/ops/admin communication exists as accepted behavior.
- Exact staff, scanner, ops, and admin access behavior.
- Whether access depends on staff assignment, scanner permissions, event ownership, viewer roles, personas and tiers, ticketing, reservations, notifications, or moderation/ops.
- Which backend/RPC/RLS behavior enforces staff/scanner/ops messaging.

No staff, scanner, or ops messaging behavior is accepted in this draft.

## 14. Attachments / Media Relationship Draft

Known facts:

- Messaging may interact with media/gallery where applicable.

Unknown / needs verification:

- Whether attachments exist as accepted messaging behavior.
- Exact attachment/media behavior.
- Whether messages can include media, gallery items, files, links, or other attachments.
- Whether attachment access depends on message visibility, participant access, media/gallery rules, public sharing, moderation/ops, or storage policies.
- Which backend/RPC/RLS behavior enforces attachment/media access.

No attachment or media behavior for messaging is accepted in this draft.

## 15. Notification Integration Draft

Known facts:

- JoinFolk has notifications concepts.
- Messaging may interact with notifications.
- Exact notification integration behavior is not accepted yet.

Unknown / needs verification:

- Whether messaging triggers notifications.
- Which messaging events may trigger notifications.
- Which recipients may receive messaging notifications.
- Whether notification behavior depends on conversation access, message visibility, viewer roles, personas and tiers, event participation, event ownership, ticketing, reservations, staff/scanner, ops/admin, or public sharing.
- Which backend/RPC/RLS behavior enforces notification-related messaging rules.

No notification integration behavior for messaging is accepted in this draft.

## 16. Moderation / Abuse / Ops Draft

Known facts:

- Messaging may interact with ops/admin/moderation.
- Exact moderation/ops behavior is not accepted yet.

Unknown / needs verification:

- Exact moderation behavior.
- Exact abuse handling behavior.
- Exact ops/admin behavior.
- Whether moderation can view, hide, remove, retain, restore, audit, or otherwise act on messages.
- Whether moderation behavior affects conversations, threads, delivery, read state, notifications, attachments, public sharing, or user/profile access.
- Which backend/RPC/RLS behavior enforces moderation/ops authority.

No moderation, abuse, or ops behavior for messaging is accepted in this draft.

## 17. Realtime / Delivery / Read State Draft

Known facts:

- Exact realtime behavior is not accepted yet.
- Exact delivery/read receipt behavior is not accepted yet.

Unknown / needs verification:

- Exact realtime behavior.
- Exact delivery behavior.
- Exact read receipt behavior.
- Exact read/unread state behavior.
- Whether realtime channels exist.
- Whether delivery or read state is per user, profile, participant, device, conversation, thread, or another model.
- Whether realtime, delivery, or read state affects notifications, moderation/ops, event participation, ticketing, reservations, or public sharing.
- Which backend/RPC/RLS behavior enforces realtime, delivery, and read state access.

No realtime, delivery, or read state behavior is accepted in this draft.

## 18. Relationship to Product Domains

### Users and profiles

Known relationship:

- Messaging may interact with users and profiles.

Unknown / needs verification:

- Exact user/profile relationship.

### Personas and tiers

Known relationship:

- Messaging may interact with personas and tiers.

Unknown / needs verification:

- Exact persona/tier relationship.

### Viewer roles

Known relationship:

- Messaging may interact with viewer roles.

Unknown / needs verification:

- Exact viewer-role relationship.

### Event lifecycle

Known relationship:

- Messaging may interact with event lifecycle.

Unknown / needs verification:

- Exact event lifecycle relationship.

### Event participation

Known relationship:

- JoinFolk has event participation concepts.
- Messaging may interact with event participation.

Unknown / needs verification:

- Exact event participation relationship.

### Event ownership

Known relationship:

- JoinFolk has host/event ownership concepts.
- Messaging may interact with event ownership.

Unknown / needs verification:

- Exact event ownership relationship.

### Ticketing

Known relationship:

- JoinFolk has ticketing.
- Messaging may interact with ticketing.

Unknown / needs verification:

- Exact ticketing relationship.

### Reservations

Known relationship:

- JoinFolk has reservations.
- Messaging may interact with reservations.

Unknown / needs verification:

- Exact reservation relationship.

### Wallet/ownership

Known relationship:

- Messaging may interact with wallet/ownership where applicable.

Unknown / needs verification:

- Exact wallet/ownership relationship.

### Staff scanner

Known relationship:

- JoinFolk has staff/scanner concepts.
- Messaging may interact with staff/scanner where applicable.

Unknown / needs verification:

- Exact staff scanner relationship.

### Host tools

Known relationship:

- Messaging may interact with host tools.

Unknown / needs verification:

- Exact host tools relationship.

### Venue/business tools

Known relationship:

- Messaging may interact with venue/business tools.

Unknown / needs verification:

- Exact venue/business tools relationship.

### Notifications

Known relationship:

- JoinFolk has notifications concepts.
- Messaging may interact with notifications.

Unknown / needs verification:

- Exact notification integration relationship.

### Media/gallery

Known relationship:

- Messaging may interact with media/gallery where applicable.

Unknown / needs verification:

- Exact media/gallery relationship.

### Ops/admin/moderation

Known relationship:

- Messaging may interact with ops/admin/moderation.

Unknown / needs verification:

- Exact ops/admin/moderation relationship.

### Public sharing

Known relationship:

- Messaging may interact with public sharing where applicable.

Unknown / needs verification:

- Exact public sharing relationship.

## 19. Cross-Surface Consistency Requirements

### Mobile

Known relationship:

- Messaging may exist on mobile, but exact mobile messaging behavior is not accepted yet.

Unknown / needs verification:

- Whether mobile supports messaging.
- Exact mobile messaging behavior.
- Which mobile behavior must match Web/Public, Dashboard/Ops, or backend behavior.

### Web/Public where applicable

Known relationship:

- Messaging may interact with public sharing where applicable.
- Exact web/public messaging behavior is not accepted yet.

Unknown / needs verification:

- Whether Web/Public supports messaging.
- Whether public surfaces expose messaging, message entry points, message notifications, or public-sharing-related communication.
- Exact public visibility rules.
- Which Web/Public behavior must match mobile, Dashboard/Ops, or backend behavior.

### Dashboard/Ops

Known relationship:

- Messaging may interact with ops/admin/moderation.
- Exact dashboard/ops messaging behavior is not accepted yet.

Unknown / needs verification:

- Whether Dashboard/Ops supports messaging, moderation, support communication, staff/admin messaging, or message review.
- Exact dashboard route/component/service ownership.
- Which Dashboard/Ops behavior must match mobile, Web/Public, or backend behavior.

### Supabase Backend

Known requirement:

- Backend/RPC/RLS must enforce security-sensitive messaging behavior.

Unknown / needs verification:

- Exact messaging schema.
- Exact messaging RPC contracts.
- Exact messaging RLS policies.
- Exact realtime behavior.
- Exact backend ownership boundaries.
- Exact enforcement model for visibility, access, participants, host/staff/admin access, attachments, notifications, moderation, and delivery/read state.

## 20. Security Risks

Known risks:

- Messaging behavior is security-sensitive where private conversation, participant-only communication, staff/host communication, or operational messages may be exposed.
- Backend/RPC/RLS must enforce security-sensitive messaging behavior.
- Frontend messaging behavior is UX only where security-sensitive.

Security risks to verify:

- Unauthorized private conversation access.
- Unauthorized participant-only message access.
- Unauthorized host, staff, or admin message access.
- Unauthorized event-level message access.
- Unauthorized ticketing/reservation message access.
- Unauthorized realtime subscription access.
- Unauthorized attachment/media access.
- Notification leakage for protected messages.
- Moderation/ops access without accepted authority.
- Frontend-only checks being treated as enforcement.

## 21. Privacy Risks

Privacy risks to verify:

- Private conversations exposed to non-participants.
- Participant-only messages exposed to hosts, staff, admins, or public surfaces without accepted authority.
- Staff/ops/admin messages exposed outside intended operational contexts.
- Message metadata exposing protected participation, ticketing, reservation, ownership, profile, or event information.
- Read receipt or delivery state revealing private behavior without accepted rules.
- Attachments/media exposing protected content.
- Notifications exposing private message content or metadata.
- Public sharing accidentally exposing messaging state.

## 22. Determinism Risks

Known determinism risks:

- Exact conversation/thread model is not accepted yet.
- Exact message visibility behavior is not accepted yet.
- Exact realtime behavior is not accepted yet.
- Exact delivery/read receipt behavior is not accepted yet.
- Exact notification integration behavior is not accepted yet.

Risks to verify:

- Conversation membership differing across surfaces.
- Message visibility differing across mobile, Web/Public, Dashboard/Ops, and backend.
- Realtime delivery diverging from persisted message access.
- Delivery/read state differing across devices or surfaces.
- Notification recipients diverging from message recipients.
- Moderation/ops actions producing inconsistent message state.

## 23. Maintainability Risks

Known maintainability risks:

- Exact messaging schema is not accepted yet.
- Exact messaging RPC contracts are not accepted yet.
- Exact messaging RLS policies are not accepted yet.
- Exact mobile, Web/Public, and Dashboard/Ops messaging behavior is not accepted yet.

Risks to verify:

- Messaging logic scattered across mobile, Web/Public, Dashboard/Ops, notifications, realtime, and backend without clear ownership.
- Frontend code encoding security-sensitive visibility or access rules.
- Conversation, access, delivery, read state, notification, attachment, and moderation logic duplicated without accepted ownership documentation.
- Schema, table, RPC, realtime channel, route, or component names being treated as canonical before verification.

## 24. Current Known Implementation

Current accepted implementation knowledge is limited to the facts in this draft:

- JoinFolk has or may have messaging concepts.
- JoinFolk has notifications concepts.
- JoinFolk has event participation concepts.
- JoinFolk has viewer roles.
- JoinFolk has personas and tiers.
- JoinFolk has ticketing and reservations.
- JoinFolk has staff/scanner concepts.
- JoinFolk has host/event ownership concepts.
- Messaging may exist as direct user-to-user, host-to-participant, participant-to-host, event-level, reservation/ticket-related, or staff/ops/admin communication, but these are possible domains only and are not accepted behavior until verified.

Unknown / needs verification:

- Exact accepted implementation across mobile, Web/Public, Dashboard/Ops, Supabase backend, users/profiles, personas/tiers, viewer roles, event lifecycle, event participation, event ownership, ticketing, reservations, wallet/ownership, staff/scanner, host tools, venue/business tools, notifications, media/gallery, ops/admin/moderation, and public sharing.

## 25. Unknowns / Needs Verification

The following must be verified before v1.0:

- Exact messaging schema.
- Exact messaging RPC contracts.
- Exact messaging RLS policies.
- Exact realtime behavior.
- Exact delivery/read receipt behavior.
- Exact conversation/thread model.
- Exact message visibility behavior.
- Exact participant access behavior.
- Exact host/staff/admin access behavior.
- Exact attachment/media behavior.
- Exact notification integration behavior.
- Exact moderation/ops behavior.
- Exact mobile messaging behavior.
- Exact web/public messaging behavior.
- Exact dashboard/ops messaging behavior.
- Exact relationship between messaging and users/profiles.
- Exact relationship between messaging and personas/tiers.
- Exact relationship between messaging and viewer roles.
- Exact relationship between messaging and event lifecycle.
- Exact relationship between messaging and event participation.
- Exact relationship between messaging and event ownership.
- Exact relationship between messaging and ticketing.
- Exact relationship between messaging and reservations.
- Exact relationship between messaging and wallet/ownership where applicable.
- Exact relationship between messaging and staff/scanner where applicable.
- Exact relationship between messaging and host tools.
- Exact relationship between messaging and venue/business tools.
- Exact relationship between messaging and notifications.
- Exact relationship between messaging and media/gallery where applicable.
- Exact relationship between messaging and ops/admin/moderation.
- Exact relationship between messaging and public sharing where applicable.

## 26. Acceptance Criteria for v1.0

Messaging v1.0 can be accepted only after verification establishes:

- Accepted messaging domain vocabulary.
- Accepted messaging schema.
- Accepted conversation/thread model.
- Accepted message visibility and access model.
- Accepted participant access behavior.
- Accepted host/staff/admin access behavior.
- Accepted event-level messaging behavior, if any.
- Accepted host/participant messaging behavior, if any.
- Accepted ticketing/reservation messaging behavior, if any.
- Accepted staff/scanner/ops messaging behavior, if any.
- Accepted attachment/media behavior.
- Accepted notification integration behavior.
- Accepted moderation/abuse/ops behavior.
- Accepted realtime behavior.
- Accepted delivery and read state behavior.
- Accepted RPC contracts, including parameters, return shapes, errors, and authorization behavior.
- Accepted RLS policies.
- Accepted cross-surface ownership for mobile, Web/Public where applicable, Dashboard/Ops, and Supabase backend.
- Accepted security-sensitive enforcement boundaries.
- Accepted privacy boundaries.
- Accepted maintainability ownership for messaging UX, backend contracts, realtime, visibility, access, notifications, attachments, moderation, delivery, and read state.

Until these criteria are met, this document remains non-canonical.

## 27. Open Questions

- Is messaging an accepted JoinFolk product domain today?
- What is the accepted messaging schema?
- What is the accepted conversation/thread model?
- What message visibility and access behavior is accepted?
- What participant access behavior is accepted?
- What host, staff, or admin access behavior is accepted?
- Does JoinFolk support direct user-to-user messaging?
- Does JoinFolk support host-to-participant or participant-to-host messaging?
- Does JoinFolk support event-level messaging?
- Does JoinFolk support ticketing or reservation-related messaging?
- Does JoinFolk support staff/scanner/ops messaging?
- What realtime behavior is accepted?
- What delivery or read receipt behavior is accepted?
- What attachment or media behavior is accepted?
- What notification integration behavior is accepted?
- What moderation, abuse, or ops behavior is accepted?
- What mobile messaging behavior is accepted?
- What Web/Public messaging behavior is accepted, if any?
- What Dashboard/Ops messaging behavior is accepted?
- What RPC contracts and RLS policies enforce messaging behavior?
- Which surfaces support messaging today: mobile, Web/Public, Dashboard/Ops, and Supabase backend?

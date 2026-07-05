# Host Identity Transfer

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## 2. Purpose

This document is a platform-level draft specification for JoinFolk host identity transfer.

This is a handbook draft. It is not a code audit and is not an accepted implementation contract. Host identity transfer is security-sensitive and may affect identity, persona, ownership, public display, event/business authority, and audit history.

All schema names, RPC names, migration names, audit event names, and field names in this document are known concepts only until verified. Prior implementation notes must not be treated as canonical.

## 3. Host Identity Transfer Definition

Host identity transfer is a controlled transfer domain or ops/admin capability in JoinFolk.

Known facts:

- JoinFolk has personas or persona-like concepts, including personal and host concepts.
- JoinFolk has profiles or profile-like concepts.
- Host identity transfer is security-sensitive.
- Backend/RPC/RLS must enforce security-sensitive host identity transfer behavior.
- Frontend/dashboard behavior is UX only where security-sensitive.
- Ops/admin approval may be involved.
- Audit logging is required or expected for security-sensitive transfer behavior.
- Dashboard/Ops may expose host transfer tooling.

Unknown / needs verification:

- Exact host identity transfer schema.
- Exact transfer request schema.
- Exact source/target user model.
- Exact approval and audit behavior.
- Exact persona-copy behavior.
- Exact ownership and public display effects.

## 4. Authority Model

### What frontend/dashboard may own

Frontend/dashboard surfaces may own user experience concerns, subject to backend/RPC/RLS enforcement for security-sensitive behavior.

Frontend/dashboard-owned behavior may include:

- Transfer tooling presentation.
- Request/application UX.
- Admin review UX.
- Risk information display UX.
- Recipient verification display UX.
- Approval workflow display UX.
- Loading, empty, and error states.
- Dashboard/Ops workflow composition.

Unknown / needs verification:

- Exact dashboard route/component/service ownership.
- Exact transfer dashboard behavior.
- Exact frontend state model.
- Exact approval workflow UX.

### What backend/RPC/RLS must enforce

Backend, RPC, and RLS must enforce security-sensitive host identity transfer behavior.

Security-sensitive behavior includes:

- Transfer creation and execution authority.
- Source/target/recipient authority.
- Recipient verification.
- Party approval where applicable.
- Risk check authority.
- JoinFolk approval authority.
- Persona-copy behavior.
- Event ownership effects.
- Venue/business ownership effects.
- Public identity/display effects.
- Audit logging.
- Rollback/reversal authority where applicable.
- Any behavior that affects personas, profiles, event ownership, venue/business tools, ticketing, reservations, media/gallery, notifications, staff/scanner, ops/admin, public sharing, or audit logs.

Unknown / needs verification:

- Exact RPC contracts.
- Exact RLS policies.
- Exact backend authority boundaries.
- Exact audit log behavior.
- Exact rollback/reversal authority.

### What must never be frontend-only

The following must never rely only on frontend/dashboard checks:

- Host identity transfer approval.
- Host identity transfer execution.
- Recipient verification.
- Risk checks.
- Party approval.
- JoinFolk approval.
- Source/target user authority.
- Persona-copy behavior.
- Event ownership transfer behavior.
- Venue/business ownership transfer behavior.
- Public identity/display mutation.
- Audit log creation or integrity.
- Rollback/reversal behavior.
- Any operation involving personas, profiles, ownership, public sharing, ops/admin, or audit logs.

## 5. Known Host Identity Transfer Concepts Draft

Known host identity transfer concepts:

- Host identity transfer.
- Controlled transfer domain.
- Ops/admin capability.
- Personas or persona-like concepts.
- Personal persona identity.
- Host persona identity.
- Profiles or profile-like concepts.
- Request/application.
- Risk check.
- Recipient verification.
- Party approval.
- JoinFolk-approved transfer.
- Audit log.
- Transfer rows.
- Recipient/to-user may be null in some prior context.
- Transfer dashboard behavior.
- Persona-copy behavior.
- Source and target user concepts.
- Source host persona fields.
- Target user profile fields.

Known related product areas:

- Personas and tiers.
- Profile identity.
- Host persona identity.
- Personal persona identity.
- Event ownership.
- Venue/business tools.
- Ticketing.
- Reservations.
- Media/gallery.
- Notifications.
- Staff/scanner where applicable.
- Ops/admin.
- Public sharing.
- Audit logs.

Unknown / needs verification:

- Exact domain vocabulary.
- Which concepts exist as persisted backend entities.
- Which concepts exist only as dashboard/Ops UX.
- Which concepts are authoritative in backend/RPC/RLS.

## 6. Known Schema / RPC / Field / Audit Concept Names Draft

Prior project context mentioned concepts or field-like names such as:

- `user_profiles`
- `profiles`
- `organizer_display_name`
- `organizer_avatar_url`
- `organizer_bio`
- `personal_avatar_url`
- `display_name`
- `avatar_url`
- `host_id`
- `created_under_persona`
- `to_user_id`

Prior project context mentioned RPC or RPC-like names such as:

- `admin_execute_host_identity_transfer_v1`

Prior project context mentioned migration-like names such as:

- `20260628_host_identity_transfer_v1_1_persona.sql`

Prior project context mentioned audit event-like names such as:

- `HOST_TRANSFER_PERSONA_IDENTITY_COPIED`

These names are known concepts only and must not be treated as accepted canonical schema, RPC, migration, field, or audit contracts until verified.

Unknown / needs verification:

- Whether these names exist in the accepted backend.
- Whether these names are current.
- Whether any of these names are exposed to frontend clients.
- Exact schema names.
- Exact field names.
- Exact RPC parameters.
- Exact RPC return shapes.
- Exact audit event names.
- Exact migration history.
- Exact RLS behavior.

## 7. Non-Accepted Host Identity Transfer Areas

The following areas are not accepted yet:

- Exact host identity transfer schema.
- Exact transfer request schema.
- Exact source/target user model.
- Exact recipient verification behavior.
- Exact party approval behavior.
- Exact risk check behavior.
- Exact JoinFolk approval behavior.
- Exact persona-copy behavior.
- Exact event ownership transfer behavior.
- Exact venue/business ownership transfer behavior.
- Exact public identity/display behavior.
- Exact audit log behavior.
- Exact notification behavior.
- Exact dashboard route/component/service ownership.
- Exact RPC contracts.
- Exact RLS policies.
- Exact rollback/reversal behavior.

These areas must remain Unknown / Needs verification until verified through accepted source material.

## 8. Controlled Transfer Flow Draft

Prior desired controlled transfer flow was:

- Request/application.
- Risk check.
- Recipient verification.
- Party approval.
- JoinFolk-approved transfer.
- Audit log.

This controlled flow is a known concept only. Exact transfer flow behavior is not accepted.

Unknown / needs verification:

- Exact request/application behavior.
- Exact risk check behavior.
- Exact recipient verification behavior.
- Exact party approval behavior.
- Exact JoinFolk approval behavior.
- Exact transfer execution behavior.
- Exact audit log behavior.
- Which viewer roles or ops/admin roles may perform each step.
- Which backend/RPC/RLS behavior enforces each step.

No exact controlled transfer flow behavior is accepted in this draft.

## 9. Source / Target / Recipient Draft

Known facts:

- Prior project context mentioned source and target user concepts.
- Prior project context mentioned transfer rows and cases where recipient/to-user may be null.
- Prior project context mentioned `to_user_id` as a field-like concept.

Unknown / needs verification:

- Exact source user model.
- Exact target user model.
- Exact recipient model.
- Whether recipient and target user are the same concept.
- What it means when recipient/to-user may be null.
- How source/target/recipient concepts relate to personas, profiles, event ownership, venue/business tools, public sharing, and audit logs.
- Which backend/RPC/RLS behavior enforces source/target/recipient authority.

No exact source, target, or recipient behavior is accepted in this draft.

## 10. Persona Copy Draft

Known facts:

- Prior project context mentioned persona-copy behavior.
- Prior project context mentioned source host persona fields and target user profile fields.
- Prior project context mentioned audit event-like name `HOST_TRANSFER_PERSONA_IDENTITY_COPIED`.

Unknown / needs verification:

- Exact persona-copy behavior.
- Whether persona-copy behavior copies host persona identity, personal persona identity, profile identity, public display fields, or other data.
- Exact source fields.
- Exact target fields.
- Exact audit behavior for persona-copy.
- Whether persona-copy affects event ownership, venue/business tools, ticketing, reservations, media/gallery, notifications, or public sharing.
- Which backend/RPC/RLS behavior enforces persona-copy behavior.

No exact persona-copy behavior is accepted in this draft.

## 11. Profile / Public Identity Draft

Known facts:

- JoinFolk has profiles or profile-like concepts.
- Host identity transfer may affect profile identity, host persona identity, personal persona identity, and public sharing.
- Prior project context mentioned source host persona fields and target user profile fields.

Unknown / needs verification:

- Exact profile identity model.
- Exact public identity/display behavior.
- Whether host identity transfer mutates public display fields.
- Whether host identity transfer affects organizer display name, avatar, bio, personal avatar, profile display name, profile avatar, or other fields.
- Whether public sharing reflects transferred identity.
- Which backend/RPC/RLS behavior enforces public identity/display changes.

No exact profile or public identity/display behavior is accepted in this draft.

## 12. Event Ownership Relationship Draft

Known facts:

- Host identity transfer may interact with event ownership.
- Host identity transfer may affect event/business authority.

Unknown / needs verification:

- Exact event ownership transfer behavior.
- Whether host identity transfer changes event ownership, event authority, event public display, event audit history, ticketing, reservations, media/gallery, notifications, or staff/scanner behavior.
- Which backend/RPC/RLS behavior enforces event ownership transfer.

No exact event ownership transfer behavior is accepted in this draft.

## 13. Venue / Business Relationship Draft

Known facts:

- Host identity transfer may interact with venue/business tools.
- Host identity transfer may affect event/business authority.

Unknown / needs verification:

- Exact venue/business ownership transfer behavior.
- Whether host identity transfer changes venue/business authority, venue/business public display, reservations, ticketing, media/gallery, notifications, ops/admin workflows, or audit logs.
- Which backend/RPC/RLS behavior enforces venue/business ownership transfer.

No exact venue/business transfer behavior is accepted in this draft.

## 14. Ticketing / Reservation / Wallet Relationship Draft

Known facts:

- Host identity transfer may interact with ticketing.
- Host identity transfer may interact with reservations.
- Host identity transfer may affect ownership.

Unknown / needs verification:

- Exact ticketing relationship.
- Exact reservation relationship.
- Exact wallet/ownership relationship.
- Whether host identity transfer affects ticketing authority, reservation authority, wallet ownership, ticket ownership, event ownership, venue/business ownership, notifications, public sharing, or audit logs.
- Which backend/RPC/RLS behavior enforces ticketing, reservation, or wallet/ownership effects.

No exact ticketing, reservation, or wallet/ownership behavior is accepted in this draft.

## 15. Media / Gallery Relationship Draft

Known facts:

- Host identity transfer may interact with media/gallery.

Unknown / needs verification:

- Exact media/gallery relationship.
- Whether host identity transfer affects media ownership, media visibility, gallery display, public sharing, event media, venue media, notifications, or audit logs.
- Which backend/RPC/RLS behavior enforces media/gallery effects.

No exact media/gallery behavior is accepted in this draft.

## 16. Notifications Relationship Draft

Known facts:

- Host identity transfer may interact with notifications.
- Exact notification behavior is not accepted yet.

Unknown / needs verification:

- Whether host identity transfer triggers notifications.
- Which transfer steps may trigger notifications.
- Which recipients may receive transfer-related notifications.
- Whether notifications are affected by risk check, recipient verification, party approval, JoinFolk approval, execution, rollback/reversal, public sharing, or audit logs.
- Which backend/RPC/RLS behavior enforces notification-related transfer rules.

No notification behavior for host identity transfer is accepted in this draft.

## 17. Audit / Ops / Approval Draft

Known facts:

- Ops/admin approval may be involved.
- Audit logging is required or expected for security-sensitive transfer behavior.
- Prior desired controlled transfer flow included JoinFolk-approved transfer and audit log.

Unknown / needs verification:

- Exact ops/admin approval behavior.
- Exact JoinFolk approval behavior.
- Exact party approval behavior.
- Exact audit log behavior.
- Exact audit event names.
- Whether audit logs record request/application, risk check, recipient verification, approvals, execution, persona-copy behavior, ownership effects, public display effects, notifications, rollback/reversal, or errors.
- Which backend/RPC/RLS behavior enforces audit/ops/approval rules.

No exact audit, ops, or approval behavior is accepted in this draft.

## 18. Rollback / Reversal Draft

Known facts:

- Exact rollback/reversal behavior is not accepted yet.

Unknown / needs verification:

- Whether rollback/reversal exists as accepted product or ops/admin behavior.
- Exact rollback behavior.
- Exact reversal behavior.
- Whether rollback/reversal affects personas, profiles, event ownership, venue/business tools, ticketing, reservations, wallet/ownership, media/gallery, notifications, public sharing, or audit logs.
- Which viewer roles or ops/admin roles may perform rollback/reversal.
- Which backend/RPC/RLS behavior enforces rollback/reversal authority.

No rollback or reversal behavior is accepted in this draft.

## 19. Relationship to Product Domains

### Personas and tiers

Known relationship:

- Host identity transfer may interact with personas and tiers.
- JoinFolk has personas or persona-like concepts, including personal and host concepts.

Unknown / needs verification:

- Exact relationship between host identity transfer, personas, personal persona identity, host persona identity, and tiers.

### Profile identity

Known relationship:

- JoinFolk has profiles or profile-like concepts.
- Host identity transfer may affect profile identity.

Unknown / needs verification:

- Exact relationship between host identity transfer and profile identity.

### Event ownership

Known relationship:

- Host identity transfer may interact with event ownership.

Unknown / needs verification:

- Exact event ownership transfer behavior and authority.

### Venue/business tools

Known relationship:

- Host identity transfer may interact with venue/business tools.
- Host identity transfer may affect event/business authority.

Unknown / needs verification:

- Exact venue/business relationship and authority effects.

### Ticketing

Known relationship:

- Host identity transfer may interact with ticketing.

Unknown / needs verification:

- Exact ticketing relationship.

### Reservations

Known relationship:

- Host identity transfer may interact with reservations.

Unknown / needs verification:

- Exact reservation relationship.

### Wallet/ownership

Known relationship:

- Host identity transfer may affect ownership.

Unknown / needs verification:

- Exact wallet/ownership relationship.

### Media/gallery

Known relationship:

- Host identity transfer may interact with media/gallery.

Unknown / needs verification:

- Exact media/gallery relationship.

### Notifications

Known relationship:

- Host identity transfer may interact with notifications.

Unknown / needs verification:

- Exact notification relationship.

### Staff scanner

Known relationship:

- Host identity transfer may interact with staff/scanner where applicable.

Unknown / needs verification:

- Whether host identity transfer affects staff/scanner authority, staff assignment, check-in, ticket queue access, notifications, or audit logs.

### Ops/admin

Known relationship:

- Host identity transfer is a controlled transfer domain or ops/admin capability.
- Ops/admin approval may be involved.

Unknown / needs verification:

- Exact ops/admin workflows.
- Exact operational approval, support, override, or audit behavior.

### Public sharing

Known relationship:

- Host identity transfer may interact with public sharing.
- Host identity transfer may affect public display.

Unknown / needs verification:

- Exact public sharing and public display behavior.

### Audit logs

Known relationship:

- Audit logging is required or expected for security-sensitive transfer behavior.

Unknown / needs verification:

- Exact audit log schema, behavior, event names, retention, and visibility.

## 20. Cross-Surface Consistency Requirements

### Dashboard/Ops

Known facts:

- Dashboard/Ops may expose host transfer tooling.
- Frontend/dashboard behavior is UX only where security-sensitive.

Unknown / needs verification:

- Exact dashboard route/component/service ownership.
- Exact transfer dashboard behavior.
- Exact ops/admin approval workflow.
- Exact relationship between dashboard state and backend transfer authority.

### Mobile

Known relationship:

- Host identity transfer may interact with personas, profiles, notifications, public sharing, and ownership concepts that mobile may consume.

Unknown / needs verification:

- Whether mobile displays or consumes host identity transfer state.
- Whether mobile displays changed persona, profile, ownership, event, venue/business, notification, media/gallery, or public sharing data after transfer.
- Which mobile behavior must match dashboard, Web/Public, or backend behavior.

### Web/Public

Known relationship:

- Host identity transfer may affect public display and public sharing.

Unknown / needs verification:

- Exact Web/Public behavior after host identity transfer.
- Exact public display and public sharing visibility rules.
- Whether Web/Public surfaces expose transfer state, transferred identity, event ownership, venue/business identity, media/gallery changes, or audit information.

### Supabase Backend

Known requirement:

- Backend/RPC/RLS must enforce security-sensitive host identity transfer behavior.

Unknown / needs verification:

- Exact schema.
- Exact RPC contracts.
- Exact RLS policies.
- Exact backend ownership boundaries.
- Exact enforcement model for controlled flow, source/target/recipient, persona-copy, ownership effects, approval, audit logging, notifications, rollback/reversal, and public sharing.

## 21. Security Risks

Known risks:

- Host identity transfer is security-sensitive.
- Host identity transfer may affect identity, persona, ownership, public display, event/business authority, and audit history.
- Backend/RPC/RLS must enforce security-sensitive host identity transfer behavior.
- Frontend/dashboard behavior is UX only where security-sensitive.

Security risks to verify:

- Unauthorized transfer request or execution.
- Unauthorized source/target/recipient selection.
- Missing or bypassed recipient verification.
- Missing or bypassed party approval.
- Missing or bypassed JoinFolk approval.
- Incomplete risk checks.
- Unauthorized persona-copy behavior.
- Unauthorized event ownership transfer.
- Unauthorized venue/business ownership transfer.
- Incorrect public identity/display changes.
- Missing or incomplete audit logging.
- Unauthorized rollback/reversal behavior.
- Frontend-only checks being treated as enforcement.

## 22. Determinism Risks

Known determinism risks:

- Host identity transfer may affect identity, persona, ownership, public display, event/business authority, and audit history.
- Exact transfer behavior is not accepted yet.
- Exact persona-copy behavior is not accepted yet.
- Exact ownership transfer behavior is not accepted yet.

Risks to verify:

- Source, target, and recipient concepts interpreted inconsistently.
- Recipient/to-user null cases handled inconsistently.
- Persona-copy behavior producing inconsistent public identity.
- Event ownership and venue/business authority diverging across surfaces.
- Notifications, public sharing, and audit logs diverging from actual transfer state.
- Rollback/reversal behavior, if accepted, producing inconsistent state.

## 23. Maintainability Risks

Known maintainability risks:

- Exact dashboard route/component/service ownership is not accepted yet.
- Exact RPC contracts are not accepted yet.
- Exact RLS policies are not accepted yet.
- Prior schema, field, migration, audit event, and RPC names are known concepts only, not accepted contracts.

Risks to verify:

- Prior implementation names being treated as canonical before verification.
- Transfer logic scattered across dashboard, backend, personas, profiles, ownership, public sharing, notifications, and audit logging without clear ownership.
- Frontend code encoding security-sensitive approval or transfer rules.
- Persona-copy, ownership transfer, notification, public display, and audit behavior duplicated without accepted ownership documentation.

## 24. Current Known Implementation

Current accepted implementation knowledge is limited to the facts in this draft:

- JoinFolk has personas or persona-like concepts, including personal and host concepts.
- JoinFolk has profiles or profile-like concepts.
- Host identity transfer is a controlled transfer domain or ops/admin capability.
- Host identity transfer is security-sensitive.
- Backend/RPC/RLS must enforce security-sensitive host identity transfer behavior.
- Frontend/dashboard behavior is UX only where security-sensitive.
- Ops/admin approval may be involved.
- Audit logging is required or expected for security-sensitive transfer behavior.
- Dashboard/Ops may expose host transfer tooling.
- Prior context mentioned transfer rows, recipient/to-user null cases, transfer dashboard behavior, persona-copy behavior, source/target user concepts, field-like names, RPC-like names, migration-like names, and audit event-like names, but none are accepted canonical contracts.

Unknown / needs verification:

- Exact accepted implementation across dashboard/Ops, mobile, Web/Public, Supabase backend, personas, profiles, event ownership, venue/business tools, ticketing, reservations, wallet/ownership, media/gallery, notifications, staff/scanner, public sharing, and audit logs.

## 25. Unknowns / Needs Verification

The following must be verified before v1.0:

- Exact host identity transfer schema.
- Exact transfer request schema.
- Exact source/target user model.
- Exact recipient verification behavior.
- Exact party approval behavior.
- Exact risk check behavior.
- Exact JoinFolk approval behavior.
- Exact persona-copy behavior.
- Exact event ownership transfer behavior.
- Exact venue/business ownership transfer behavior.
- Exact public identity/display behavior.
- Exact audit log behavior.
- Exact notification behavior.
- Exact dashboard route/component/service ownership.
- Exact RPC contracts.
- Exact RLS policies.
- Exact rollback/reversal behavior.
- Exact relationship between host identity transfer and personas/tiers.
- Exact relationship between host identity transfer and profile identity.
- Exact relationship between host identity transfer and ticketing.
- Exact relationship between host identity transfer and reservations.
- Exact relationship between host identity transfer and wallet/ownership.
- Exact relationship between host identity transfer and media/gallery.
- Exact relationship between host identity transfer and staff scanner where applicable.
- Exact relationship between host identity transfer and public sharing.

## 26. Acceptance Criteria for v1.0

Host Identity Transfer v1.0 can be accepted only after verification establishes:

- Accepted host identity transfer domain vocabulary.
- Accepted host identity transfer schema.
- Accepted transfer request schema.
- Accepted source/target/recipient model.
- Accepted controlled transfer flow.
- Accepted risk check behavior.
- Accepted recipient verification behavior.
- Accepted party approval behavior.
- Accepted JoinFolk approval behavior.
- Accepted persona-copy behavior.
- Accepted profile/public identity behavior.
- Accepted event ownership transfer behavior.
- Accepted venue/business ownership transfer behavior.
- Accepted ticketing, reservation, and wallet/ownership relationships.
- Accepted media/gallery relationship.
- Accepted notification behavior.
- Accepted audit log behavior.
- Accepted rollback/reversal behavior, if any.
- Accepted RPC contracts, including parameters, return shapes, errors, and authorization behavior.
- Accepted RLS policies.
- Accepted cross-surface ownership for Dashboard/Ops, mobile, Web/Public, and Supabase backend.
- Accepted security-sensitive enforcement boundaries.
- Accepted maintainability ownership for dashboard tooling, backend contracts, RLS, personas, profiles, ownership effects, notifications, public sharing, ops/admin, audit logs, and rollback/reversal.

Until these criteria are met, this document remains non-canonical.

## 27. Open Questions

- What is the accepted host identity transfer schema?
- What is the accepted transfer request schema?
- What is the accepted source/target/recipient model?
- What does it mean when recipient/to-user may be null?
- What is the accepted controlled transfer flow?
- What is the accepted risk check behavior?
- What is the accepted recipient verification behavior?
- What is the accepted party approval behavior?
- What is the accepted JoinFolk approval behavior?
- What is the accepted persona-copy behavior?
- Which profile or public identity fields are affected by transfer, if any?
- What event ownership transfer behavior is accepted?
- What venue/business ownership transfer behavior is accepted?
- How does host identity transfer interact with ticketing?
- How does host identity transfer interact with reservations?
- How does host identity transfer interact with wallet/ownership?
- How does host identity transfer interact with media/gallery?
- Which host identity transfer steps trigger notifications, if any?
- What audit log behavior is accepted?
- What rollback or reversal behavior is accepted, if any?
- What dashboard routes, components, and services own host transfer tooling?
- What public display or public sharing behavior is accepted after transfer?
- Which surfaces support host identity transfer today: Dashboard/Ops, mobile, Web/Public, and Supabase backend?

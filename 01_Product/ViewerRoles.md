# Viewer Roles

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines the platform-level draft specification for JoinFolk viewer roles.

Viewer roles describe what a person can see or do around an event based on context.

This document is not canonical. No viewer role behavior or permission matrix is accepted by this document alone.

## Definitions

### Viewer Role

A viewer role is a contextual role used to determine what a person can see or do around an event.

Viewer role is not the same thing as account tier.

Viewer role is not the same thing as persona.

### Viewer Role Resolver

A viewer role resolver is the logic that determines a viewer role for a person in a specific event context.

Exact resolver implementation is Unknown / Needs verification.

### Permission Matrix

A permission matrix defines which actions or views are allowed for each viewer role.

Exact permission matrix is Unknown / Needs verification.

### Contextual Authority

Contextual authority means viewer role depends on context such as authentication, event relationship, ticket ownership, participation, check-in, host ownership, staff assignment, and ops privilege.

## Known Viewer Roles

### guest

`guest` is a known viewer role.

Exact permissions are Unknown / Needs verification.

### authenticated_non_participant

`authenticated_non_participant` is a known viewer role.

Exact permissions are Unknown / Needs verification.

### ticket_holder

`ticket_holder` is a known viewer role.

Ticket holder semantics must be verified against backend and mobile behavior.

Exact permissions are Unknown / Needs verification.

### participant

`participant` is a known viewer role.

Participant semantics must be verified against backend and mobile behavior.

Exact permissions are Unknown / Needs verification.

### checked_in

`checked_in` is a known viewer role.

Exact permissions are Unknown / Needs verification.

### host

`host` is a known viewer role.

Host role is high-risk and must be backend-enforced.

Exact permissions are Unknown / Needs verification.

### staff

`staff` is a known viewer role.

Staff scanner role is high-risk and must be backend-enforced.

Exact permissions are Unknown / Needs verification.

### ops

`ops` is a known viewer role.

Ops role is high-risk and must be backend-enforced.

Exact permissions are Unknown / Needs verification.

## Canonical Rules Draft

### What Is Known

JoinFolk has viewer roles used to determine what a person can see or do around an event.

Known viewer roles are `guest`, `authenticated_non_participant`, `ticket_holder`, `participant`, `checked_in`, `host`, `staff`, and `ops`.

Viewer roles must be evaluated consistently across Mobile, Dashboard, Web/Public, and Supabase.

Viewer role depends on context such as authentication, event relationship, ticket ownership, participation, check-in, host ownership, staff assignment, and ops privilege.

Backend, RPC, and RLS must enforce security-sensitive viewer role permissions.

### What Is Not Yet Accepted

Exact resolver implementation is not accepted.

Exact permission matrix is not accepted.

Exact RLS/RPC enforcement is not accepted.

Gallery/media upload behavior is not accepted.

Staff scanner rules are not accepted.

Ticket holder and participant semantics are not accepted.

No rule in this document is canonical.

## Viewer Role Authority Model

### What Frontend May Resolve/Display

Frontend surfaces may display viewer role labels, role-dependent actions, role-dependent UI state, and UX guardrails.

Frontend surfaces may perform client-side role resolution to support user experience.

Client-side role resolution is a UX helper only.

### What Backend Must Enforce

Backend, RPC, and RLS must enforce security-sensitive viewer role permissions.

Backend enforcement is required for protected event access, event management, ticket-sensitive actions, participation-sensitive actions, check-in-sensitive actions, host authority, staff scanner authority, ops authority, and media/gallery authority where applicable.

### What Must Never Be Frontend-Only

The following must never be frontend-only:

- security-sensitive viewer role resolution
- protected event access authority
- ticket holder authority
- participant authority
- checked-in authority
- host authority
- staff scanner authority
- ops authority
- gallery/media upload authority
- event management authority

## Cross-Surface Consistency Requirements

### Mobile

Mobile must use viewer role semantics consistent with the rest of the platform.

Mobile-only viewer role authority is not accepted.

### Dashboard

Dashboard must use viewer role semantics consistent with the rest of the platform.

Dashboard role-dependent actions must be backed by backend authority.

### Web/Public

Web/public surfaces must use viewer role semantics consistent with the rest of the platform.

Public visibility must not bypass backend viewer role enforcement where protected access is required.

### Supabase Backend

Supabase database, RPC, RLS, auth, and storage behavior must enforce security-sensitive viewer role permissions.

Exact backend resolver, RLS, and RPC enforcement are Unknown / Needs verification.

## Relationship to Other Models

### Account Tiers

Viewer role is not the same thing as account tier.

Account tier may be relevant context, but exact interaction is Unknown / Needs verification.

### Personas

Viewer role is not the same thing as persona.

Persona may be relevant context for host-facing behavior, but exact interaction is Unknown / Needs verification.

### Event Lifecycle

Viewer role may interact with event lifecycle for visibility and available actions.

Exact lifecycle-dependent role behavior is Unknown / Needs verification.

### Ticketing

Viewer role depends on ticket ownership where applicable.

Ticket holder semantics must be verified against backend and mobile behavior.

### Reservations

Viewer role may interact with reservations.

Exact reservation-dependent role behavior is Unknown / Needs verification.

### Media/gallery

Gallery/media upload is expected to require checked-in/live-event style authority, but exact accepted behavior is not yet verified.

This rule is not accepted.

### Staff scanner

Staff scanner role is high-risk and must be backend-enforced.

Exact scanner permissions are Unknown / Needs verification.

### Ops/admin

Ops role is high-risk and must be backend-enforced.

Exact ops/admin permissions are Unknown / Needs verification.

## Security Risks

Frontend-only role resolution may grant unauthorized access.

Incorrect host role enforcement may allow unauthorized event management.

Incorrect staff role enforcement may allow unauthorized scanner use.

Incorrect ops enforcement may expose privileged operations.

Incorrect ticket holder or participant semantics may expose protected event capabilities.

Incorrect gallery/media authority may allow unauthorized uploads.

## Determinism Risks

Mobile, Dashboard, Web/Public, and Supabase may resolve viewer roles differently.

Ticket holder and participant may be interpreted inconsistently.

Checked-in authority may drift between scanner, mobile, and backend behavior.

Gallery/media behavior may diverge from lifecycle and check-in semantics.

Ops/admin authority may be inconsistently applied across surfaces.

## Current Known Implementation

Known implementation facts:

- JoinFolk has viewer roles used to determine what a person can see or do around an event.
- Known viewer roles include `guest`, `authenticated_non_participant`, `ticket_holder`, `participant`, `checked_in`, `host`, `staff`, and `ops`.
- Gallery/media upload is expected to require checked-in/live-event style authority.
- Staff scanner role is high-risk and must be backend-enforced.
- Host role is high-risk and must be backend-enforced.
- Ops role is high-risk and must be backend-enforced.
- Ticket holder and participant semantics must be verified against backend and mobile behavior.

These facts are not accepted as canonical rules.

## Unknowns / Needs Verification

The following are Unknown / Needs verification:

- Exact viewer role resolver implementation.
- Exact permission matrix.
- Exact RLS/RPC enforcement.
- Exact permissions for `guest`.
- Exact permissions for `authenticated_non_participant`.
- Exact permissions for `ticket_holder`.
- Exact permissions for `participant`.
- Exact permissions for `checked_in`.
- Exact permissions for `host`.
- Exact permissions for `staff`.
- Exact permissions for `ops`.
- Exact gallery/media upload authority.
- Exact staff scanner rules.
- Exact ticket holder semantics.
- Exact participant semantics.
- Cross-surface consistency across Mobile, Dashboard, Web/Public, and Supabase.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- Viewer role names are confirmed.
- Viewer role resolver behavior is specified or explicitly deferred.
- Permission matrix is specified or explicitly deferred.
- Backend enforcement expectations are verified.
- RLS/RPC enforcement is documented or explicitly deferred.
- Ticket holder and participant semantics are verified.
- Host, staff, and ops authority rules are verified.
- Gallery/media authority is verified or explicitly deferred.
- Cross-surface consistency is audited.
- Open questions are resolved, explicitly deferred, or moved to tracked status documents.
- Mustafa / JoinFolk approves the document for acceptance.

## Open Questions

- What is the accepted viewer role resolver algorithm?
- What permissions belong to each viewer role?
- What is the accepted difference between `ticket_holder` and `participant`?
- What backend rules enforce `checked_in` authority?
- What backend rules enforce host, staff, and ops authority?
- What is the accepted gallery/media upload authority rule?
- Which surface should be audited first for viewer role consistency?

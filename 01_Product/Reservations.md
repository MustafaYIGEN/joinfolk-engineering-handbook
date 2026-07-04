# Reservations

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines the platform-level draft specification for JoinFolk reservation behavior.

It records known reservation concepts, authority boundaries, consistency requirements, and verification gaps.

This document is not canonical. No reservation behavior is accepted by this document alone.

## Definitions

### Reservation

A reservation is JoinFolk behavior related to the commerce mode `reservation`.

Reservation is separate from `ticketSales`.

### Event Reservation

An event reservation is a reservation associated with an event.

Exact event reservation behavior is Unknown / Needs verification.

### Venue Reservation

A venue reservation is a reservation associated with a venue or venue/business tools.

Exact venue reservation behavior is Unknown / Needs verification.

### Reservation Request

A reservation request is a known reservation-related concept.

Exact request behavior is Unknown / Needs verification.

### Reservation Status

A reservation status is a state used by a reservation flow.

Exact reservation statuses are Unknown / Needs verification.

### Host Approval

Host approval is a known reservation-related concept.

Exact host approval behavior is Unknown / Needs verification.

### Auto-approval

Auto-approval is a known reservation-related concept.

Exact auto-approval behavior is Unknown / Needs verification.

## Known Reservation Concepts

Known reservation concepts include:

- event reservations
- venue reservations
- reservation requests
- reservation status
- host approval
- auto-approval
- reservation management surfaces

These concepts require verification before acceptance.

## Canonical Rules Draft

### What Is Known

JoinFolk has reservation behavior.

Reservations are related to the commerce mode `reservation`.

Reservation is separate from `ticketSales`.

An event should not have `ticketSales` and `reservation` active as incompatible commerce authorities at the same time unless explicitly treated as `conflict` by the commerce model.

JoinFolk has event reservations.

JoinFolk has venue reservations.

Event reservations and venue reservations may use different status flows.

Venue/business tools include reservations.

Dashboard has reservation management surfaces.

Mobile may create or interact with reservations.

Backend, RPC, and RLS must enforce security-sensitive reservation behavior.

Client-side reservation state is UX only.

Mobile, Dashboard, Web/Public, and Supabase must agree on reservation semantics.

Reservations interact with commerce model, viewer roles, event lifecycle, venue system, notifications, and public event visibility.

### What Is Not Yet Accepted

Exact reservation schema is not accepted.

Exact reservation status machine is not accepted.

Exact event reservation behavior is not accepted.

Exact venue reservation behavior is not accepted.

Exact auto-approval behavior is not accepted.

Exact host approval behavior is not accepted.

Exact RPC contracts are not accepted.

Exact RLS enforcement is not accepted.

No rule in this document is canonical.

## Reservation Authority Model

### What Frontend May Display

Frontend surfaces may display reservation state, reservation request UX, approval or auto-approval labels, venue reservation UX, event reservation UX, and reservation management UI.

Frontend surfaces may guide users through reservation-related flows.

Client-side reservation state is UX only.

### What Backend Must Enforce

Backend, RPC, and RLS must enforce security-sensitive reservation behavior.

Backend enforcement is required for reservation creation, reservation mutation, reservation status transitions, host approval, auto-approval, protected reservation access, event reservation authority, and venue reservation authority where applicable.

### What Must Never Be Frontend-Only

The following must never be frontend-only:

- reservation authority
- reservation request authority
- reservation status authority
- event reservation authority
- venue reservation authority
- host approval authority
- auto-approval authority
- protected reservation access
- commerce conflict authority

## Reservation Status / State Machine Draft

### Known States/Concepts

Known state-related concepts include reservation request, reservation status, host approval, and auto-approval.

Exact reservation statuses are not accepted.

Event reservations and venue reservations may use different status flows.

### Unknown States

The full reservation status machine is Unknown / Needs verification.

The difference between event reservation statuses and venue reservation statuses is Unknown / Needs verification.

No exact reservation statuses are defined by this document.

### Required Verification

Verification must identify accepted reservation states, allowed transitions, backend authority, RPC contracts, RLS enforcement, and cross-surface display rules.

Verification must distinguish event reservation behavior from venue reservation behavior if they use different status flows.

## Event Reservation Draft

### Known Behavior

JoinFolk has event reservations.

Mobile may create or interact with reservations.

Dashboard has reservation management surfaces.

### Unknowns

Exact event reservation behavior is Unknown / Needs verification.

Exact event reservation status flow is Unknown / Needs verification.

Exact event reservation approval behavior is Unknown / Needs verification.

## Venue Reservation Draft

### Known Behavior

JoinFolk has venue reservations.

Venue/business tools include reservations.

Dashboard has reservation management surfaces.

### Unknowns

Exact venue reservation behavior is Unknown / Needs verification.

Exact venue reservation status flow is Unknown / Needs verification.

Exact venue reservation approval behavior is Unknown / Needs verification.

## Relationship to Other Models

### Commerce model

Reservations are related to the commerce mode `reservation`.

Reservation is separate from `ticketSales`.

Commerce conflict handling must address incompatible `ticketSales` and `reservation` authority.

### Viewer roles

Reservations interact with viewer roles.

Exact viewer-role-dependent reservation behavior is Unknown / Needs verification.

### Event lifecycle

Reservations interact with event lifecycle.

Exact lifecycle-dependent reservation behavior is Unknown / Needs verification.

### Venue system

Venue/business tools include reservations.

Exact venue system reservation behavior is Unknown / Needs verification.

### Notifications

Reservations interact with notifications.

Exact notification behavior is Unknown / Needs verification.

### Public sharing

Reservations interact with public event visibility.

Exact public sharing behavior for reservations is Unknown / Needs verification.

## Cross-Surface Consistency Requirements

### Mobile

Mobile must use reservation semantics consistent with the rest of the platform.

Mobile may create or interact with reservations.

Mobile-only reservation authority is not accepted.

### Dashboard

Dashboard must use reservation semantics consistent with the rest of the platform.

Dashboard reservation management actions must be backed by backend authority.

### Web/Public

Web/public surfaces must use reservation semantics consistent with the rest of the platform.

Public reservation display must not bypass backend reservation authority.

### Supabase Backend

Supabase database, RPC, RLS, auth, and storage behavior must enforce security-sensitive reservation behavior where applicable.

Exact backend/RPC/RLS enforcement is Unknown / Needs verification.

## Security Risks

Frontend-only reservation authority may allow unauthorized reservation creation, approval, mutation, or visibility.

Incorrect host approval behavior may allow unauthorized approval or rejection.

Incorrect auto-approval behavior may create reservations without accepted authority.

Incorrect event and venue reservation separation may apply the wrong status flow.

Incorrect commerce conflict handling may allow incompatible `ticketSales` and `reservation` authority.

Incorrect backend enforcement may expose protected reservation data.

## Determinism Risks

Mobile, Dashboard, Web/Public, and Supabase may interpret reservation state differently.

Event reservations and venue reservations may be treated as the same flow when they require different status flows.

Auto-approval and host approval may be applied inconsistently.

Commerce conflict behavior may differ between reservation and ticketing surfaces.

Notification behavior may drift from actual reservation state.

## Current Known Implementation

Known implementation facts:

- JoinFolk has reservation behavior.
- Reservations are related to the commerce mode `reservation`.
- Reservation is separate from `ticketSales`.
- JoinFolk has event reservations.
- JoinFolk has venue reservations.
- Event reservations and venue reservations may use different status flows.
- Venue/business tools include reservations.
- Dashboard has reservation management surfaces.
- Mobile may create or interact with reservations.

These facts are not accepted as canonical rules.

## Unknowns / Needs Verification

The following are Unknown / Needs verification:

- Exact reservation schema.
- Exact reservation status machine.
- Exact event reservation behavior.
- Exact venue reservation behavior.
- Exact auto-approval behavior.
- Exact host approval behavior.
- Exact RPC contracts.
- Exact RLS enforcement.
- Exact commerce conflict handling for reservations.
- Exact reservation notification behavior.
- Exact public visibility behavior for reservations.
- Cross-surface consistency across Mobile, Dashboard, Web/Public, and Supabase.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- Reservation concepts are confirmed.
- Reservation schema is documented or explicitly deferred.
- Reservation status machine is specified or explicitly deferred.
- Event reservation behavior is verified or explicitly deferred.
- Venue reservation behavior is verified or explicitly deferred.
- Auto-approval behavior is verified or explicitly deferred.
- Host approval behavior is verified or explicitly deferred.
- RPC contracts are documented or explicitly deferred.
- RLS enforcement is documented or explicitly deferred.
- Commerce conflict behavior is verified.
- Cross-surface consistency is audited.
- Open questions are resolved, explicitly deferred, or moved to tracked status documents.
- Mustafa / JoinFolk approves the document for acceptance.

## Open Questions

- What is the accepted reservation status machine?
- How do event reservation and venue reservation flows differ?
- What is the accepted auto-approval behavior, if any?
- What is the accepted host approval behavior?
- What backend rules enforce reservation authority?
- What RPC contracts support reservations?
- Which surface should be audited first for reservation consistency?

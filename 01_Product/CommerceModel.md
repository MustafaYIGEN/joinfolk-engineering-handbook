# Commerce Model

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines the platform-level draft specification for JoinFolk commerce behavior.

It records known commerce modes, authority boundaries, consistency requirements, and verification gaps.

This document is not canonical. No commerce behavior is accepted by this document alone.

## Definitions

### Commerce Model

The commerce model is the platform rule set for commerce-related event behavior.

Ticketing and reservations must depend on the same commerce model.

### Commerce Mode

A commerce mode is the resolved commerce state for an event.

Known commerce modes are `none`, `ticketSales`, `reservation`, and `conflict`.

### Native Commerce

Native commerce is commerce behavior handled by JoinFolk platform capabilities.

Native commerce is expected to require pro-host style authority, but exact accepted behavior is not yet verified.

### ticketSales

`ticketSales` is a known commerce mode and event module or module-like commerce capability.

Exact schema, module behavior, and permissions are Unknown / Needs verification.

### reservation

`reservation` is a known commerce mode and event module or module-like commerce capability.

Exact schema, module behavior, and permissions are Unknown / Needs verification.

### conflict state

`conflict` means the platform detects incompatible commerce configuration.

Exact conflict detection and handling behavior are Unknown / Needs verification.

## Known Commerce Modes

### none

`none` is a known commerce mode.

Exact meaning and resolver conditions are Unknown / Needs verification.

### ticketSales

`ticketSales` is a known commerce mode.

Exact meaning and resolver conditions are Unknown / Needs verification.

### reservation

`reservation` is a known commerce mode.

Exact meaning and resolver conditions are Unknown / Needs verification.

### conflict

`conflict` is a known commerce mode.

Conflict means incompatible commerce configuration has been detected.

Exact meaning, resolver conditions, and remediation behavior are Unknown / Needs verification.

## Canonical Rules Draft

### What Is Known

JoinFolk has commerce-related event behavior.

Known commerce modes are `none`, `ticketSales`, `reservation`, and `conflict`.

`ticketSales` and `reservation` are event modules or module-like commerce capabilities.

An event should not have `ticketSales` and `reservation` active as incompatible commerce authorities at the same time unless explicitly treated as `conflict`.

`conflict` means the platform detects incompatible commerce configuration.

Frontend commerce display is UX only.

Backend, RPC, and RLS must enforce security-sensitive commerce behavior.

Mobile, Dashboard, Web/Public, and Supabase must agree on commerce semantics.

Commerce behavior interacts with event lifecycle, viewer roles, tickets, reservations, wallet/ownership, and public event visibility.

### What Is Not Yet Accepted

Native commerce authority requirements are not accepted.

Semi-pro behavior around native commerce is not accepted.

Pro behavior around native commerce is not accepted.

Exact account tier permissions for commerce are not accepted.

Exact resolver implementation is not accepted.

Exact backend/RPC/RLS enforcement is not accepted.

Exact module table or schema behavior is not accepted.

No rule in this document is canonical.

## Commerce Authority Model

### What Frontend May Display

Frontend surfaces may display commerce mode, commerce-related labels, purchase or reservation UX, conflict warnings, and disabled states based on available platform data.

Frontend surfaces may help users navigate ticketing or reservation flows.

Frontend commerce display is UX only.

### What Backend Must Enforce

Backend, RPC, and RLS must enforce security-sensitive commerce behavior.

Backend enforcement is required for commerce authority, commerce mode mutation, ticketing authority, reservation authority, wallet/ownership effects, and protected commerce data access.

### What Must Never Be Frontend-Only

The following must never be frontend-only:

- commerce authority
- native commerce eligibility
- ticket sales authority
- reservation authority
- conflict state authority
- payment or commerce mutation authority
- ticket ownership authority
- wallet ownership authority
- protected commerce data access

## Commerce Mode Resolver Draft

### Known Inputs

Known inputs include event commerce configuration and whether `ticketSales` or `reservation` capabilities are active.

Account tier may be relevant to native commerce authority, but exact behavior is Unknown / Needs verification.

### Unknown Inputs

The following resolver inputs are Unknown / Needs verification:

- exact event fields
- exact module or module-like records
- exact account tier inputs
- exact persona inputs
- exact lifecycle inputs
- exact backend authority inputs
- exact schema dependencies

### Conflict Handling

An event should not have `ticketSales` and `reservation` active as incompatible commerce authorities at the same time unless explicitly treated as `conflict`.

`conflict` means incompatible commerce configuration has been detected.

Exact conflict detection, display, enforcement, and remediation behavior are Unknown / Needs verification.

## Relationship to Other Models

### Account tiers

Native commerce is expected to require pro-host style authority, but exact account tier permissions are not accepted.

Semi-pro and pro commerce behavior are Unknown / Needs verification.

### Personas

Commerce authority may interact with host-facing persona behavior.

Exact persona-commerce rules are Unknown / Needs verification.

### Event lifecycle

Commerce behavior interacts with event lifecycle.

Exact lifecycle-dependent commerce behavior is Unknown / Needs verification.

### Viewer roles

Commerce behavior interacts with viewer roles.

Exact viewer-role-dependent commerce behavior is Unknown / Needs verification.

### Ticketing

Ticketing must depend on the same commerce model.

Exact ticketing rules are Unknown / Needs verification.

### Reservations

Reservations must depend on the same commerce model.

Exact reservation rules are Unknown / Needs verification.

### Wallet/ownership

Commerce behavior interacts with wallet and ownership.

Exact wallet/ownership effects are Unknown / Needs verification.

### Public sharing

Commerce behavior interacts with public event visibility.

Exact public sharing behavior for commerce states is Unknown / Needs verification.

## Cross-Surface Consistency Requirements

### Mobile

Mobile must use commerce semantics consistent with the rest of the platform.

Mobile-only commerce authority is not accepted.

### Dashboard

Dashboard must use commerce semantics consistent with the rest of the platform.

Dashboard commerce actions must be backed by backend authority.

### Web/Public

Web/public surfaces must use commerce semantics consistent with the rest of the platform.

Public commerce display must not bypass backend commerce authority.

### Supabase Backend

Supabase database, RPC, RLS, auth, and storage behavior must enforce security-sensitive commerce behavior where applicable.

Exact backend/RPC/RLS enforcement is Unknown / Needs verification.

## Security Risks

Frontend-only commerce authority may allow unauthorized commerce behavior.

Incorrect tier handling may grant or block native commerce incorrectly.

Incorrect conflict handling may allow incompatible ticketing and reservation authority.

Incorrect backend enforcement may affect payment, ticket, reservation, wallet, or ownership behavior.

Inconsistent public display may expose unavailable or invalid commerce options.

## Determinism Risks

Mobile, Dashboard, Web/Public, and Supabase may resolve commerce mode differently.

Ticketing and reservations may diverge from the shared commerce model.

`ticketSales` and `reservation` may be active at the same time without consistent conflict handling.

Semi-pro and pro behavior may be interpreted inconsistently before acceptance.

## Current Known Implementation

Known implementation facts:

- JoinFolk has commerce-related event behavior.
- Known commerce modes include `none`, `ticketSales`, `reservation`, and `conflict`.
- `ticketSales` and `reservation` are event modules or module-like commerce capabilities.
- Native commerce is expected to require pro-host style authority.
- Ticketing and reservations must depend on the same commerce model.
- Commerce behavior interacts with event lifecycle, viewer roles, tickets, reservations, wallet/ownership, and public event visibility.

These facts are not accepted as canonical rules.

## Unknowns / Needs Verification

The following are Unknown / Needs verification:

- Exact resolver implementation.
- Exact account tier permissions for commerce.
- Exact semi-pro behavior around native commerce.
- Exact pro behavior around native commerce.
- Exact backend/RPC/RLS enforcement.
- Exact module table or schema behavior.
- Exact conflict detection behavior.
- Exact conflict remediation behavior.
- Exact ticketing dependency on commerce mode.
- Exact reservation dependency on commerce mode.
- Exact wallet/ownership effects.
- Exact public event visibility effects.
- Cross-surface consistency across Mobile, Dashboard, Web/Public, and Supabase.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- Commerce modes are confirmed.
- Commerce mode resolver behavior is specified or explicitly deferred.
- Conflict handling is specified or explicitly deferred.
- Account tier commerce permissions are verified or explicitly deferred.
- Backend/RPC/RLS enforcement is documented or explicitly deferred.
- Ticketing and reservation dependencies are verified.
- Wallet/ownership effects are verified or explicitly deferred.
- Public visibility effects are verified or explicitly deferred.
- Cross-surface consistency is audited.
- Open questions are resolved, explicitly deferred, or moved to tracked status documents.
- Mustafa / JoinFolk approves the document for acceptance.

## Open Questions

- What is the accepted commerce mode resolver algorithm?
- What exact authority is required for native commerce?
- What are the accepted commerce permissions for `semi_pro` and `pro`?
- What backend rules enforce commerce authority?
- How should `conflict` be displayed, blocked, and remediated?
- Which surface should be audited first for commerce consistency?

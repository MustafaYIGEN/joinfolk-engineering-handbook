# Mobile Architecture

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior project context
- canonical: false

## Purpose

This document defines the platform-level mobile architecture specification draft for JoinFolk.

It records known mobile responsibilities, authority boundaries, high-risk areas, and verification gaps.

This document is not canonical. No mobile architecture rule is accepted by this document alone.

## Mobile Architecture Definition

Mobile app is one implementation surface of JoinFolk.

Mobile must use the same business rules as Dashboard, Web/Public, and Supabase.

Mobile frontend guards are UX helpers only.

Security-sensitive mobile behavior must be enforced by backend, RPC, and RLS.

Exact mobile route architecture, navigation architecture, component boundaries, service/data-access ownership, guard implementation, RPC contracts, direct Supabase call policy, state management strategy, testing strategy, offline/retry behavior, and media upload pipeline are Unknown / Needs verification.

## Mobile Responsibilities Draft

Mobile participates in product domains including:

- event discovery
- event detail
- event participation
- event lifecycle display
- viewer roles
- commerce model
- ticketing
- reservations
- wallet/ownership
- media/gallery
- notifications
- staff scanner or scanner-related flows where applicable
- public sharing where applicable

Mobile may create or interact with reservations.

Mobile may interact with ticketing and wallet/ownership behavior.

Mobile may interact with media/gallery behavior.

These responsibilities require verification before acceptance.

## Mobile Authority Model

### What Mobile Frontend May Own

Mobile frontend may own presentation, screen navigation, local interaction state, form state, client-side validation for UX, display formatting, and non-authoritative guardrails.

Mobile frontend may guide users through event discovery, event detail, event participation, lifecycle display, viewer role display, commerce, ticketing, reservations, wallet/ownership, media/gallery, notifications, scanner-related flows where applicable, and public sharing where applicable.

Mobile frontend ownership is not security authority.

### What Backend/RPC/RLS Must Enforce

Backend, RPC, and RLS must enforce security-sensitive mobile behavior.

Backend authority is required for check-in, ticket ownership, wallet ownership, reservation authority, media/gallery upload authority, staff scanner authority, protected visibility, protected data access, event lifecycle-sensitive actions, commerce authority, and viewer-role-sensitive actions.

### What Must Never Be Mobile-Only

The following must never be mobile-only:

- authentication-sensitive access authority
- authorization authority
- viewer role authority
- ticket ownership authority
- wallet ownership authority
- reservation authority
- check-in authority
- staff scanner authority
- media/gallery upload authority
- commerce authority
- protected public/private visibility authority
- protected storage/media access authority

## Mobile Layering Draft

### Screens/routes

Screens and routes define mobile navigation and screen-level composition.

Exact mobile route architecture is Unknown / Needs verification.

### Components

Components define reusable mobile UI behavior where applicable.

Exact component boundaries are Unknown / Needs verification.

### Hooks/services

Hooks and services may coordinate data access, business logic, and frontend behavior.

Exact mobile service/data-access ownership is Unknown / Needs verification.

### Supabase/RPC access

Mobile may use backend, RPC, or Supabase-backed access where applicable.

Exact mobile RPC contracts and direct Supabase call policy are Unknown / Needs verification.

### Shared domain rules

Mobile must use shared domain rules consistently with Dashboard, Web/Public, and Supabase.

Exact shared domain rule integration is Unknown / Needs verification.

### Media/upload layer

Mobile may interact with media/gallery behavior.

Exact media upload pipeline is Unknown / Needs verification.

## Service and Data Access Draft

Mobile service and data-access ownership is not accepted.

Mobile data access must be audited for consistency with shared business rules and backend authority.

No exact service ownership, RPC usage, or direct Supabase call behavior is accepted by this document.

## Guard and Permission Draft

Mobile frontend guards are UX helpers only.

Client-side mobile state is UX only where security-sensitive.

Exact mobile guard implementation is Unknown / Needs verification.

Guard verification must identify which backend authority enforces the same security-sensitive rule.

## Media / Gallery Draft

Mobile may interact with media/gallery behavior.

Media/gallery upload authority is security-sensitive.

Exact media upload pipeline is Unknown / Needs verification.

Media/gallery behavior must be consistent with backend authority and shared platform rules.

## Offline / Retry / Local State Draft

Mobile may have local state.

Client-side mobile state is UX only where security-sensitive.

Exact offline/retry behavior is Unknown / Needs verification.

Offline, retry, and local state behavior must not create security-sensitive authority without backend enforcement.

## High-Risk Mobile Areas

### Viewer role display vs authority

Viewer role display may guide UX.

Viewer role authority must be backend-enforced where security-sensitive.

### Ticket/wallet ownership

Ticket ownership and wallet ownership are security-sensitive.

Exact mobile ticket/wallet ownership behavior is Unknown / Needs verification.

### Reservations

Mobile may create or interact with reservations.

Reservation authority is security-sensitive.

Exact mobile reservation behavior is Unknown / Needs verification.

### Check-in and staff/scanner flows

Check-in and staff scanner authority are security-sensitive.

Exact mobile check-in and staff/scanner behavior is Unknown / Needs verification.

### Media/gallery upload

Media/gallery upload authority is security-sensitive.

Exact mobile upload behavior is Unknown / Needs verification.

### Protected public/private visibility

Protected visibility is security-sensitive.

Exact mobile visibility behavior is Unknown / Needs verification.

## Relationship to Product Domains

### Event lifecycle

Mobile participates in event lifecycle display.

Exact mobile lifecycle behavior is Unknown / Needs verification.

### Viewer roles

Mobile participates in viewer role behavior.

Exact mobile viewer role behavior is Unknown / Needs verification.

### Commerce

Mobile participates in commerce model behavior.

Exact mobile commerce behavior is Unknown / Needs verification.

### Ticketing

Mobile may interact with ticketing behavior.

Exact mobile ticketing behavior is Unknown / Needs verification.

### Reservations

Mobile may create or interact with reservations.

Exact mobile reservation behavior is Unknown / Needs verification.

### Wallet/ownership

Mobile may interact with wallet/ownership behavior.

Exact mobile wallet/ownership behavior is Unknown / Needs verification.

### Media/gallery

Mobile may interact with media/gallery behavior.

Exact mobile media/gallery behavior is Unknown / Needs verification.

### Notifications

Mobile participates in notification behavior.

Exact mobile notification behavior is Unknown / Needs verification.

### Staff scanner

Mobile may participate in staff scanner or scanner-related flows where applicable.

Exact mobile scanner behavior is Unknown / Needs verification.

### Public sharing

Mobile may participate in public sharing where applicable.

Exact mobile public sharing behavior is Unknown / Needs verification.

## Cross-Surface Consistency Requirements

### Mobile

Mobile must use shared business rules and backend authority for security-sensitive behavior.

Mobile-only authority is not accepted.

### Dashboard

Mobile behavior must remain consistent with Dashboard shared business rules where applicable.

Differences between Mobile and Dashboard behavior must be audited and recorded.

### Web/Public

Mobile behavior must remain consistent with Web/Public shared business rules where applicable.

Public sharing behavior must not bypass backend authority.

### Supabase Backend

Mobile security-sensitive behavior must be enforced by Supabase backend, RPC, and RLS.

Exact backend/RPC/RLS enforcement for mobile behavior is Unknown / Needs verification.

## Security Risks

Mobile frontend guards may be mistaken for security authority.

Client-side mobile state may be mistaken for authoritative ownership, reservation, ticketing, check-in, scanner, media, or visibility state.

Unverified mobile data access may expose protected data.

Unverified media upload behavior may allow unauthorized uploads.

Unverified offline/retry behavior may replay or mutate security-sensitive actions incorrectly.

Unverified scanner or check-in behavior may allow unauthorized check-ins.

## Maintainability Risks

Unaccepted route and navigation architecture may make mobile flows hard to audit.

Unaccepted component boundaries may increase duplicated behavior.

Unaccepted service/data-access ownership may create inconsistent backend usage.

Unaccepted state management may duplicate shared rule interpretation.

Unaccepted testing strategy may leave high-risk mobile behavior unverified.

## Determinism Risks

Mobile may interpret shared business rules differently from Dashboard, Web/Public, or Supabase.

Mobile local state may diverge from backend authority.

Mobile offline/retry behavior may produce inconsistent results if not verified.

Mobile media/upload behavior may diverge from storage and viewer-role authority.

Mobile ticketing, reservation, wallet, and check-in flows may diverge from backend enforcement.

## Current Known Implementation

Known implementation facts:

- Mobile app is one implementation surface of JoinFolk.
- Mobile must use the same business rules as Dashboard, Web/Public, and Supabase.
- Mobile frontend guards are UX helpers only.
- Security-sensitive mobile behavior must be enforced by backend/RPC/RLS.
- Mobile may create or interact with reservations.
- Mobile may interact with ticketing and wallet/ownership behavior.
- Mobile may interact with media/gallery behavior.

These facts are not accepted as canonical mobile architecture rules.

## Unknowns / Needs Verification

The following are Unknown / Needs verification:

- Exact mobile route architecture.
- Exact navigation architecture.
- Exact component boundaries.
- Exact mobile service/data-access ownership.
- Exact mobile guard implementation.
- Exact mobile RPC contracts.
- Exact mobile direct Supabase call policy.
- Exact mobile state management strategy.
- Exact mobile testing strategy.
- Exact offline/retry behavior.
- Exact media upload pipeline.
- Backend/RPC/RLS enforcement for security-sensitive mobile behavior.
- Cross-surface consistency with Dashboard, Web/Public, and Supabase.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- Mobile responsibilities are confirmed.
- Route architecture is documented or explicitly deferred.
- Navigation architecture is documented or explicitly deferred.
- Component boundaries are documented or explicitly deferred.
- Service/data-access ownership is documented or explicitly deferred.
- Guard behavior is verified against backend enforcement.
- RPC contracts are documented or explicitly deferred.
- Direct Supabase call policy is documented or explicitly deferred.
- State management strategy is documented or explicitly deferred.
- Testing strategy is documented or explicitly deferred.
- Offline/retry behavior is verified or explicitly deferred.
- Media upload pipeline is verified or explicitly deferred.
- High-risk mobile areas are audited.
- Cross-surface consistency risks are recorded.
- Open questions are resolved, explicitly deferred, or moved to tracked status documents.
- Mustafa / JoinFolk approves the document for acceptance.

## Open Questions

- What is the accepted mobile route architecture?
- What is the accepted mobile navigation architecture?
- What component boundaries should be treated as stable?
- What service/data-access ownership model should mobile use?
- Which mobile guards are backed by backend/RPC/RLS enforcement?
- What are the accepted mobile RPC contracts?
- What offline/retry behavior is allowed for security-sensitive flows?
- What is the accepted mobile media upload pipeline?

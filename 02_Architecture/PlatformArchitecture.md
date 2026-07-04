# Platform Architecture

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines the platform-level architecture specification draft for JoinFolk.

It records known implementation surfaces, architecture principles, authority boundaries, risks, and verification gaps.

This document is not canonical. No architecture rule is accepted by this document alone.

## Platform Architecture Definition

JoinFolk is a social event platform with multiple implementation surfaces.

The platform architecture must support shared business rules across Mobile, Dashboard, Web/Public pages, and Supabase backend behavior.

The platform must separate frontend user experience from backend security authority for security-sensitive behavior.

Exact repository boundaries, shared package strategy, backend schema/RPC/RLS behavior, and CI/CD/build architecture are Unknown / Needs verification.

## Platform Surfaces

### Mobile App

The Mobile app is an implementation surface of the JoinFolk platform.

It must use the same business rules as Dashboard, Web/Public, and Supabase where applicable.

### Dashboard

The Dashboard is an implementation surface of the JoinFolk platform.

It must use the same business rules as Mobile, Web/Public, and Supabase where applicable.

### Web/Public

Web/Public pages are implementation surfaces of the JoinFolk platform.

They must use the same business rules as Mobile, Dashboard, and Supabase where applicable.

### Supabase Backend

Supabase provides database, RPC, storage, and auth responsibilities.

Supabase backend behavior must enforce security-sensitive rules through backend, RPC, and RLS where applicable.

## Core Architectural Principle

### Shared Business Rules

Mobile, Dashboard, Web/Public, and Supabase must share the same business rules.

Product domains that require shared rules include event discovery, event participation, event lifecycle, host/persona system, viewer roles, commerce model, ticketing, reservations, wallet/ownership, notifications, venue/business tools, media/gallery, staff scanner, ops/admin, host identity transfer, and public sharing.

### Backend Authority

Security-sensitive behavior must be enforced by backend, RPC, and RLS.

Backend authority is required for protected access, permissions, ownership, lifecycle transitions, commerce authority, ticketing, reservations, staff scanner, ops/admin, storage access, and other security-sensitive behavior.

### Frontend UX Only for Security-Sensitive Display

Client-side guards are UX helpers only.

Frontend surfaces may display state, guide flows, and block obvious invalid actions for user experience.

Frontend-only guards must not be treated as security authority.

## Layering Model Draft

### UI Layer

The UI layer owns presentation, layout, navigation, form state, and user interaction.

The UI layer may show client-side guards as UX helpers.

### Domain/Business Rules Layer

The domain/business rules layer should express shared platform rules consistently across surfaces.

Exact shared package architecture is Unknown / Needs verification.

### Data Access Layer

The data access layer mediates reads and writes from implementation surfaces to backend services where applicable.

Direct Supabase calls in pages or components are not automatically wrong, but they are an architecture risk requiring audit.

### Backend Authority Layer

The backend authority layer includes Supabase database, RPC, RLS, auth, and related backend enforcement.

Exact backend schema, RPC, and RLS behavior are Unknown / Needs verification.

### Storage/Media Layer

The storage/media layer includes storage and media responsibilities.

Exact storage architecture and media authority rules are Unknown / Needs verification.

## Authority Boundaries

### What Frontend May Own

Frontend surfaces may own presentation, routing, local interaction state, client-side validation for UX, display formatting, and non-authoritative flow guidance.

Frontend surfaces may use guards to improve user experience.

### What Backend Must Own

Backend, RPC, and RLS must own security-sensitive enforcement.

This includes authentication-sensitive access, authorization, ownership, protected data access, lifecycle transitions, commerce authority, ticketing authority, reservation authority, wallet ownership, staff scanner authorization, ops/admin authority, and storage/media access where security-sensitive.

### What Must Never Be Frontend-Only

The following must never be frontend-only:

- authentication authority
- authorization authority
- ownership authority
- event lifecycle transition authority
- viewer role authority
- commerce authority
- ticket ownership authority
- wallet ownership authority
- reservation authority
- staff scanner authority
- ops/admin authority
- host identity transfer authority
- protected storage/media access
- public/private visibility authority for protected data

## Cross-Surface Consistency Model

### Mobile

Mobile must use shared platform rules and backend authority for security-sensitive behavior.

Mobile-only business rule authority is not accepted.

### Dashboard

Dashboard must use shared platform rules and backend authority for security-sensitive behavior.

Dashboard client guards for auth, tier, ops, host, and staff surfaces are UX helpers only unless backed by backend enforcement.

### Web/Public

Web/Public pages must use shared platform rules and backend authority for security-sensitive behavior.

Public display must not bypass backend visibility or access authority.

### Supabase

Supabase database, RPC, storage, and auth responsibilities must support shared platform rules and backend authority.

Exact schema/RPC/RLS behavior is Unknown / Needs verification.

## Data Flow Draft

### User/auth

User and auth flows must rely on backend authority for security-sensitive behavior.

Exact auth data flow is Unknown / Needs verification.

### Event lifecycle

Event lifecycle transitions must be backend-enforced.

Exact lifecycle data flow is Unknown / Needs verification.

### Viewer role

Viewer role resolution must be consistent across surfaces and backend-enforced where security-sensitive.

Exact viewer role resolver behavior is Unknown / Needs verification.

### Commerce

Commerce behavior must be consistent across surfaces and backend-enforced where security-sensitive.

Exact commerce resolver and authority behavior are Unknown / Needs verification.

### Ticketing/reservations

Ticketing and reservations must rely on shared commerce semantics and backend authority.

Exact ticketing and reservation data flows are Unknown / Needs verification.

### Media/storage

Media and storage behavior must rely on backend authority where access or mutation is security-sensitive.

Exact storage/media data flow is Unknown / Needs verification.

### Notifications

Notification behavior must be consistent across surfaces where applicable and backend-enforced where security-sensitive.

Exact notification data flow is Unknown / Needs verification.

## Known Architecture Risks

Known architecture risks include:

- broad service layer
- direct Supabase calls in some pages/components
- many RPC-backed flows
- high-risk venue editor and ticket/product-section mapping
- client guards for auth/tier/ops/host/staff surfaces
- no complete backend/RLS verification yet

Direct Supabase calls require audit to determine whether they are appropriate, duplicated, security-sensitive, or inconsistent with shared platform rules.

## Determinism Risks

Mobile, Dashboard, Web/Public, and Supabase may implement business rules differently.

Client guards may drift from backend enforcement.

RPC-backed flows may have unclear contracts without accepted documentation.

Direct data access from UI surfaces may duplicate or bypass shared rule interpretation.

Venue editor and ticket/product-section mapping may create inconsistent state if not governed by backend authority.

## Simplification Principles

Shared business rules should be explicit, documented, and reused where possible.

Security-sensitive decisions should be centralized in backend, RPC, and RLS authority.

Frontend surfaces should avoid becoming independent authorities for the same product rule.

Architecture changes should reduce ambiguity, duplication, and inconsistent behavior across surfaces.

Large rewrites should not be used as a default fix. Changes must be scoped, auditable, and verifiable.

## Current Known Implementation

Known implementation facts:

- JoinFolk has Mobile app, Dashboard, Web/Public pages, and Supabase backend surfaces.
- Supabase provides database, RPC, storage, and auth responsibilities.
- Current dashboard audit found a broad service layer.
- Current dashboard audit found direct Supabase calls in some pages/components.
- Current dashboard audit found many RPC-backed flows.
- Current dashboard audit found high-risk venue editor and ticket/product-section mapping.
- Current dashboard audit found client guards for auth/tier/ops/host/staff surfaces.
- There is no complete backend/RLS verification yet.

These facts are not accepted as canonical architecture rules.

## Unknowns / Needs Verification

The following are Unknown / Needs verification:

- Exact repository boundaries.
- Exact shared package strategy.
- Exact backend schema behavior.
- Exact RPC behavior.
- Exact RLS behavior.
- Exact CI/CD/build architecture.
- Exact data access architecture per surface.
- Exact storage/media architecture.
- Which direct Supabase calls are appropriate.
- Which direct Supabase calls duplicate or bypass shared rules.
- Which client guards are backed by backend enforcement.
- Cross-surface consistency across Mobile, Dashboard, Web/Public, and Supabase.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- Platform surfaces are confirmed.
- Repository boundaries are documented or explicitly deferred.
- Shared business rule strategy is documented or explicitly deferred.
- Backend authority boundaries are verified.
- Backend schema/RPC/RLS behavior is documented or explicitly deferred.
- Data access architecture is audited.
- Direct Supabase call risks are classified.
- Client guard risks are classified.
- CI/CD/build architecture is documented or explicitly deferred.
- Cross-surface consistency risks are recorded.
- Open questions are resolved, explicitly deferred, or moved to tracked status documents.
- Mustafa / JoinFolk approves the document for acceptance.

## Open Questions

- What are the accepted repository boundaries?
- What is the accepted shared package or shared rules strategy?
- Which business rules are duplicated across Mobile, Dashboard, Web/Public, and Supabase?
- Which direct Supabase calls are acceptable, and which require refactoring?
- Which client guards are backed by backend enforcement?
- What is the accepted CI/CD/build architecture?
- Which architecture area should be audited first?

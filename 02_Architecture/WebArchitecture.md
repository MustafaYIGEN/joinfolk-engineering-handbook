# Web Architecture

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines the platform-level web/public architecture specification draft for JoinFolk.

It records known Web/Public responsibilities, authority boundaries, visibility requirements, and verification gaps.

This document is not canonical. No Web/Public architecture rule is accepted by this document alone.

## Web/Public Architecture Definition

Web/Public pages are one implementation surface of JoinFolk.

Web/Public must use the same business rules as Mobile, Dashboard, and Supabase.

Web/Public frontend guards are UX helpers only where applicable.

Security-sensitive Web/Public behavior must be enforced by backend, RPC, and RLS.

Exact Web/Public route architecture, public sharing implementation, event sharing data contract, backend/RPC/RLS enforcement, visibility rules, SEO/social preview behavior, deployment/build architecture, and testing strategy are Unknown / Needs verification.

## Web/Public Responsibilities Draft

Web/Public may support public event visibility and public sharing.

Web/Public behavior may interact with:

- event discovery
- event detail
- event lifecycle display
- viewer roles
- commerce model
- ticketing
- reservations
- media/gallery
- notifications where applicable
- public sharing

These responsibilities require verification before acceptance.

## Web/Public Authority Model

### What Web/Public Frontend May Own

Web/Public frontend may own presentation, routing, public-facing layout, local interaction state, display formatting, SEO/social preview presentation where applicable, and non-authoritative guardrails.

Web/Public frontend may display public event visibility and public sharing UX based on available platform data.

Web/Public frontend ownership is not security authority.

### What Backend/RPC/RLS Must Enforce

Backend, RPC, and RLS must enforce security-sensitive Web/Public behavior.

Backend authority is required for protected public/private visibility, protected event access, viewer-role-sensitive access, commerce-sensitive access, ticketing-sensitive access, reservation-sensitive access, media/gallery access, and notification access where applicable.

### What Must Never Be Web/Public-Only

The following must never be Web/Public-only:

- protected public/private visibility authority
- protected event access authority
- viewer role authority
- commerce authority
- ticketing authority
- reservation authority
- protected media/gallery access authority
- protected notification access authority
- public sharing access authority

## Web/Public Layering Draft

### Routes/pages

Routes and pages define Web/Public navigation and page-level composition.

Exact Web/Public route architecture is Unknown / Needs verification.

### Components

Components define reusable Web/Public UI behavior where applicable.

Exact component boundaries are Unknown / Needs verification.

### Data access

Data access must be consistent with shared domain rules and backend authority.

Exact Web/Public data access architecture is Unknown / Needs verification.

### Supabase/RPC access

Web/Public may use Supabase or RPC-backed access where applicable.

Exact backend/RPC/RLS behavior is Unknown / Needs verification.

### Shared domain rules

Web/Public must use shared domain rules consistently with Mobile, Dashboard, and Supabase.

Exact shared domain rule integration is Unknown / Needs verification.

### Public sharing layer

Public sharing is a JoinFolk product domain.

Exact public sharing implementation is Unknown / Needs verification.

## Public Sharing Draft

Public sharing may support public event visibility and event sharing.

Public display must not bypass backend visibility or access authority.

Exact public sharing implementation is Unknown / Needs verification.

Exact event sharing data contract is Unknown / Needs verification.

Exact SEO/social preview behavior is Unknown / Needs verification.

## Visibility and Access Draft

Protected public/private visibility is security-sensitive.

Public display must not bypass backend visibility or access authority.

Exact visibility rules are Unknown / Needs verification.

Visibility behavior must be consistent with event lifecycle, viewer roles, commerce, ticketing, reservations, media/gallery, notifications where applicable, and public sharing.

## Relationship to Product Domains

### Event discovery

Web/Public may interact with event discovery.

Exact Web/Public event discovery behavior is Unknown / Needs verification.

### Event detail

Web/Public may interact with event detail.

Exact Web/Public event detail behavior is Unknown / Needs verification.

### Event lifecycle

Web/Public may display event lifecycle.

Exact Web/Public lifecycle display behavior is Unknown / Needs verification.

### Viewer roles

Web/Public may interact with viewer role behavior.

Exact Web/Public viewer role behavior is Unknown / Needs verification.

### Commerce

Web/Public may interact with commerce model behavior.

Exact Web/Public commerce behavior is Unknown / Needs verification.

### Ticketing

Web/Public may interact with ticketing behavior.

Exact Web/Public ticketing behavior is Unknown / Needs verification.

### Reservations

Web/Public may interact with reservations behavior.

Exact Web/Public reservation behavior is Unknown / Needs verification.

### Media/gallery

Web/Public may interact with media/gallery behavior.

Exact Web/Public media/gallery behavior is Unknown / Needs verification.

### Notifications

Web/Public may interact with notifications where applicable.

Exact Web/Public notification behavior is Unknown / Needs verification.

### Public sharing

Public sharing is a JoinFolk product domain.

Exact public sharing behavior is Unknown / Needs verification.

## Cross-Surface Consistency Requirements

### Mobile

Web/Public behavior must remain consistent with Mobile shared business rules where applicable.

Differences between Web/Public and Mobile behavior must be audited and recorded.

### Dashboard

Web/Public behavior must remain consistent with Dashboard shared business rules where applicable.

Public sharing support must remain consistent with dashboard-managed event behavior where applicable.

### Web/Public

Web/Public must use shared business rules and backend authority for security-sensitive behavior.

Web/Public-only authority is not accepted.

### Supabase Backend

Web/Public security-sensitive behavior must be enforced by Supabase backend, RPC, and RLS.

Exact backend/RPC/RLS enforcement for Web/Public behavior is Unknown / Needs verification.

## Security Risks

Web/Public frontend guards may be mistaken for security authority.

Public display may expose protected private event data if backend visibility is not enforced.

Public sharing may expose incorrect event information if the sharing data contract is unverified.

Commerce, ticketing, reservation, media/gallery, or notification data may be exposed without backend authority.

SEO/social previews may expose data if visibility rules are not enforced.

## Maintainability Risks

Unaccepted route architecture may make public flows hard to audit.

Unaccepted public sharing implementation may duplicate domain rules.

Unaccepted event sharing data contracts may create inconsistent public display.

Unaccepted deployment/build architecture may hide production differences.

Unaccepted testing strategy may leave visibility behavior unverified.

## Determinism Risks

Web/Public may interpret shared business rules differently from Mobile, Dashboard, or Supabase.

Public sharing may display different event data than other surfaces.

Visibility behavior may diverge from event lifecycle, viewer role, commerce, ticketing, or reservation rules.

SEO/social preview behavior may diverge from protected visibility rules.

Backend/RPC/RLS behavior may differ from Web/Public frontend assumptions.

## Current Known Implementation

Known implementation facts:

- Web/Public pages are one implementation surface of JoinFolk.
- Web/Public must use the same business rules as Mobile, Dashboard, and Supabase.
- Web/Public frontend guards are UX helpers only where applicable.
- Security-sensitive Web/Public behavior must be enforced by backend/RPC/RLS.
- Public sharing is a JoinFolk product domain.
- Web/Public may support public event visibility and public sharing.

These facts are not accepted as canonical Web/Public architecture rules.

## Unknowns / Needs Verification

The following are Unknown / Needs verification:

- Exact Web/Public route architecture.
- Exact public sharing implementation.
- Exact event sharing data contract.
- Exact backend/RPC/RLS enforcement.
- Exact visibility rules.
- Exact SEO/social preview behavior.
- Exact deployment/build architecture.
- Exact testing strategy.
- Exact Web/Public data access architecture.
- Cross-surface consistency with Mobile, Dashboard, and Supabase.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- Web/Public responsibilities are confirmed.
- Route architecture is documented or explicitly deferred.
- Public sharing implementation is documented or explicitly deferred.
- Event sharing data contract is documented or explicitly deferred.
- Visibility rules are verified.
- Backend/RPC/RLS enforcement is documented or explicitly deferred.
- SEO/social preview behavior is verified or explicitly deferred.
- Deployment/build architecture is documented or explicitly deferred.
- Testing strategy is documented or explicitly deferred.
- Cross-surface consistency risks are recorded.
- Open questions are resolved, explicitly deferred, or moved to tracked status documents.
- Mustafa / JoinFolk approves the document for acceptance.

## Open Questions

- What is the accepted Web/Public route architecture?
- What is the accepted public sharing implementation?
- What is the accepted event sharing data contract?
- What backend rules enforce public/private visibility?
- What SEO/social preview behavior is allowed for protected or non-public events?
- What deployment/build architecture supports Web/Public pages?
- Which Web/Public visibility flows should be audited first?

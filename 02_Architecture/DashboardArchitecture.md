# Dashboard Architecture

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior dashboard audit summary
- canonical: false

## Purpose

This document defines the platform-level dashboard architecture specification draft for JoinFolk.

It records known dashboard architecture facts, authority boundaries, high-risk areas, and verification gaps.

This document is not canonical. No dashboard architecture rule is accepted by this document alone.

## Dashboard Architecture Definition

Dashboard is one implementation surface of JoinFolk.

Dashboard must use the same business rules as Mobile, Web/Public, and Supabase.

Dashboard frontend guards are UX helpers only.

Security-sensitive dashboard behavior must be enforced by backend, RPC, and RLS.

Exact dashboard route architecture, component boundaries, service/data-access ownership, guard implementation, RPC call contracts, direct Supabase call policy per file, state management strategy, and testing strategy are Unknown / Needs verification.

## Dashboard Responsibilities Draft

Dashboard product areas may include:

- event management
- host/persona behavior
- event lifecycle management
- commerce module management
- ticketing/product management
- reservations management
- venue/business tools
- media/gallery management
- staff/scanner-related management
- ops/admin surfaces
- host identity transfer surfaces
- public sharing support

These responsibilities require verification before acceptance.

## Dashboard Authority Model

### What Dashboard Frontend May Own

Dashboard frontend may own presentation, route navigation, local interaction state, form state, client-side validation for UX, display formatting, and non-authoritative guardrails.

Dashboard frontend may guide event management, host/persona flows, lifecycle management, commerce management, ticketing, reservations, venue tools, media/gallery, staff/scanner-related workflows, ops/admin surfaces, host identity transfer surfaces, and public sharing support.

Dashboard frontend ownership is not security authority.

### What Dashboard Backend/RPC/RLS Must Enforce

Backend, RPC, and RLS must enforce security-sensitive dashboard behavior.

Backend authority is required for authentication-sensitive access, authorization, tier-sensitive behavior, host authority, staff authority, ops/admin authority, lifecycle transitions, commerce authority, ticketing authority, reservation authority, venue editor changes, product-section mapping, media/storage access, host identity transfer, and protected public/private visibility.

### What Must Never Be Dashboard-Only

The following must never be dashboard-only:

- authentication authority
- authorization authority
- tier permission authority
- host authority
- staff authority
- ops/admin authority
- event lifecycle transition authority
- commerce authority
- ticketing/product authority
- product-section mapping authority
- reservation authority
- venue editor authority
- media/storage authority
- host identity transfer authority
- protected public/private visibility authority

## Dashboard Layering Draft

### Routes/pages

Routes and pages define dashboard navigation and screen-level composition.

Exact dashboard route architecture is Unknown / Needs verification.

### Components

Components define reusable dashboard UI behavior where applicable.

Exact component boundaries are Unknown / Needs verification.

### Hooks/services

Hooks and services may coordinate data access, business logic, and frontend behavior.

Current dashboard audit found a broad service layer.

Exact service ownership is Unknown / Needs verification.

### Supabase/RPC access

Dashboard uses Supabase and RPC-backed flows where applicable.

Current dashboard audit found many RPC-backed flows and direct Supabase calls in some pages/components.

Exact RPC behavior and direct Supabase call policy per file are Unknown / Needs verification.

### Shared domain rules

Dashboard must use shared domain rules consistently with Mobile, Web/Public, and Supabase.

Exact shared domain rule integration is Unknown / Needs verification.

## Service and Data Access Draft

Current dashboard audit found a broad service layer.

Current dashboard audit found direct Supabase calls in some pages/components.

Current dashboard audit found many RPC-backed flows.

Service and data-access ownership must be audited before acceptance.

No exact service ownership model is accepted by this document.

## Direct Supabase Call Policy Draft

Direct Supabase calls in dashboard pages/components are not automatically wrong.

They are an audit risk because they may duplicate domain logic, obscure backend authority, bypass shared rules, or make consistency harder to verify.

Each direct Supabase call requires classification as acceptable, needs wrapping, needs refactoring, or needs backend enforcement review.

This document does not require all direct Supabase calls to be removed.

## Guard and Permission Draft

Current dashboard audit found client guards for auth/tier/ops/host/staff surfaces.

Dashboard guards are UX helpers only unless backed by backend, RPC, or RLS enforcement.

Exact guard implementation is Unknown / Needs verification.

Guard verification must identify which backend authority enforces the same security-sensitive rule.

## High-Risk Dashboard Areas

### Venue editor

Current dashboard audit found a high-risk venue editor.

Exact venue editor architecture and backend enforcement are Unknown / Needs verification.

### Ticket/product-section mapping

Current dashboard audit found high-risk ticket/product-section mapping.

Exact product-section mapping behavior and backend enforcement are Unknown / Needs verification.

### Ops/admin

Ops/admin surfaces are security-sensitive.

Exact dashboard ops/admin behavior and backend enforcement are Unknown / Needs verification.

### Host identity transfer

Host identity transfer surfaces are security-sensitive.

Exact dashboard host identity transfer behavior and backend enforcement are Unknown / Needs verification.

### Staff/scanner

Staff/scanner-related management is security-sensitive.

Exact dashboard staff/scanner behavior and backend enforcement are Unknown / Needs verification.

## Relationship to Product Domains

### Event lifecycle

Dashboard may support event lifecycle management.

Exact lifecycle management behavior is Unknown / Needs verification.

### Viewer roles

Dashboard may use viewer role semantics.

Exact dashboard viewer role behavior is Unknown / Needs verification.

### Commerce

Dashboard may support commerce module management.

Exact commerce management behavior is Unknown / Needs verification.

### Ticketing

Dashboard may support ticketing/product management.

Exact ticketing/product behavior is Unknown / Needs verification.

### Reservations

Dashboard may support reservations management.

Exact reservations behavior is Unknown / Needs verification.

### Wallet/ownership

Dashboard may interact with wallet/ownership behavior through ticketing or related flows.

Exact wallet/ownership dashboard behavior is Unknown / Needs verification.

### Venue/business tools

Dashboard may support venue/business tools.

Exact venue/business behavior is Unknown / Needs verification.

### Media/gallery

Dashboard may support media/gallery management.

Exact media/gallery dashboard behavior is Unknown / Needs verification.

### Ops/admin

Dashboard may include ops/admin surfaces.

Exact ops/admin dashboard behavior is Unknown / Needs verification.

### Host identity transfer

Dashboard may include host identity transfer surfaces.

Exact host identity transfer dashboard behavior is Unknown / Needs verification.

### Public sharing

Dashboard may support public sharing.

Exact public sharing dashboard behavior is Unknown / Needs verification.

## Cross-Surface Consistency Requirements

### Mobile

Dashboard behavior must remain consistent with Mobile shared business rules.

Differences between Dashboard and Mobile behavior must be audited and recorded.

### Dashboard

Dashboard must use shared business rules and backend authority for security-sensitive behavior.

Dashboard-only authority is not accepted.

### Web/Public

Dashboard behavior must remain consistent with Web/Public shared business rules where applicable.

Public sharing support must not bypass backend authority.

### Supabase Backend

Dashboard security-sensitive behavior must be enforced by Supabase backend, RPC, and RLS.

Exact backend/RPC/RLS enforcement is Unknown / Needs verification.

## Security Risks

Dashboard client guards may be mistaken for security authority.

Direct Supabase calls may bypass shared rule interpretation or obscure backend enforcement.

Unverified RPC contracts may allow unauthorized or inconsistent operations.

Venue editor changes may affect high-risk venue or layout state.

Ticket/product-section mapping may affect ticketing, capacity, seating, or venue state incorrectly.

Ops/admin, staff/scanner, and host identity transfer surfaces may expose privileged actions without verified backend enforcement.

## Maintainability Risks

Large complex files require careful scoped changes.

A broad service layer may make ownership unclear.

Direct Supabase calls in pages/components may make data access patterns inconsistent.

Unaccepted component boundaries may increase duplication.

Unaccepted route architecture may make flows harder to verify.

No complete test/lint coverage is accepted yet.

## Determinism Risks

Dashboard may interpret shared business rules differently from Mobile, Web/Public, or Supabase.

Client guards may drift from backend enforcement.

RPC-backed flows may behave differently than frontend assumptions.

Direct Supabase calls may create inconsistent data access behavior.

High-risk venue editor and ticket/product-section mapping behavior may diverge from backend authority.

## Current Known Implementation

Known implementation facts from current dashboard audit:

- Vite React dashboard app.
- Broad service layer.
- Direct Supabase calls in some pages/components.
- Many RPC-backed flows.
- Client guards for auth/tier/ops/host/staff surfaces.
- High-risk venue editor.
- High-risk ticket/product-section mapping.
- No complete backend/RLS verification yet.
- No complete test/lint coverage accepted yet.
- Large complex files that require careful scoped changes.

These facts are not accepted as canonical dashboard architecture rules.

## Unknowns / Needs Verification

The following are Unknown / Needs verification:

- Exact dashboard route architecture.
- Exact component boundaries.
- Exact service/data-access ownership.
- Exact guard implementation.
- Exact RPC call contracts.
- Exact direct Supabase call policy per file.
- Exact state management strategy.
- Exact testing strategy.
- Backend/RPC/RLS enforcement for dashboard security-sensitive behavior.
- Cross-surface consistency with Mobile, Web/Public, and Supabase.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- Dashboard responsibilities are confirmed.
- Route architecture is documented or explicitly deferred.
- Component boundaries are documented or explicitly deferred.
- Service/data-access ownership is documented or explicitly deferred.
- Direct Supabase calls are audited and classified.
- Guard behavior is verified against backend enforcement.
- RPC call contracts are documented or explicitly deferred.
- State management strategy is documented or explicitly deferred.
- Testing strategy is documented or explicitly deferred.
- High-risk areas are audited.
- Cross-surface consistency risks are recorded.
- Open questions are resolved, explicitly deferred, or moved to tracked status documents.
- Mustafa / JoinFolk approves the document for acceptance.

## Open Questions

- What is the accepted dashboard route architecture?
- What component boundaries should be treated as stable?
- What service/data-access ownership model should dashboard use?
- Which direct Supabase calls are acceptable as-is?
- Which guards are backed by backend/RPC/RLS enforcement?
- What are the accepted RPC contracts used by dashboard?
- What testing strategy is required before accepting dashboard architecture?

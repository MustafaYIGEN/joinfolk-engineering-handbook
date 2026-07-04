# Platform Overview

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines the top-level product constitution for JoinFolk during the Handbook Bootstrap / Product Constitution phase.

It records the current platform-level product facts that may guide future specifications, audits, patch plans, patches, verification, and accepted changes.

This document is not yet authoritative. No product specification is accepted yet.

## Product Definition

JoinFolk is a social event platform.

The platform includes event discovery, participation, lifecycle, identity, ticketing, reservations, venue/business tools, media, wallet/ticket ownership, staff operations, admin operations, host identity transfer, and public sharing.

The platform must become deterministic, secure, maintainable, and production-ready.

## Platform Surfaces

### Mobile

The mobile app is one implementation surface of the JoinFolk platform.

Mobile behavior must align with shared platform business rules.

### Dashboard

The dashboard is one implementation surface of the JoinFolk platform.

Dashboard behavior must align with shared platform business rules.

### Web/Public

Web and public pages are implementation surfaces of the JoinFolk platform.

Web/public behavior must align with shared platform business rules.

### Supabase Backend

The Supabase backend includes database, RPC, storage, and auth responsibilities.

Backend, RPC, and RLS behavior must be the authority for security-sensitive operations.

## Core Domains

The core platform domains are:

- event discovery
- event participation
- event lifecycle
- host/persona system
- ticketing
- reservations
- venue/business tools
- media/gallery
- wallet/ticket ownership
- staff scanner
- ops/admin tools
- host identity transfer
- public event sharing

Each domain requires future specification before it can be treated as accepted platform behavior.

## Authority Model

Business rules must be shared across Mobile, Dashboard, Web, and Supabase.

Frontend surfaces may decide presentation, navigation, local form state, non-authoritative client validation, and UX guardrails.

The backend must decide security-sensitive operations, permission checks, ownership authority, durable state transitions, and protected data access.

Client-side guards are UX helpers. They are not security authority.

The following must never be frontend-only:

- authentication authority
- authorization authority
- ownership authority
- ticket ownership authority
- wallet ownership authority
- payment or commerce authority
- staff scanner authorization
- event lifecycle authority
- protected data access
- storage access authority

## Determinism Principles

Shared business rules must produce consistent outcomes across Mobile, Dashboard, Web, and Supabase.

Platform state transitions must be explicit, auditable, and reproducible.

Security-sensitive outcomes must not depend on client-only checks.

When implementation behavior differs across surfaces, the inconsistency must be recorded and resolved through the handbook workflow.

## Security Principles

Backend, RPC, and RLS must enforce security-sensitive rules.

Frontend checks may improve user experience, but they must not be the only enforcement layer for protected actions.

Permission, ownership, storage, scanner, commerce, and lifecycle decisions require backend authority.

Security-sensitive changes must follow governance, patch policy, and do-not-touch rules.

## Maintainability Principles

Business rules must not be allowed to drift independently across Mobile, Dashboard, Web, and Supabase.

Shared platform behavior must be specified before it is treated as accepted.

Audits must distinguish between intended behavior, observed implementation, and accepted platform rules.

Patches must be scoped, reviewed, verified, and recorded.

## Current Known Implementation

Known platform implementations include:

- Mobile app
- Dashboard
- Web/public pages
- Supabase database/RPC/storage/auth

No implementation behavior is accepted by this document alone.

Existing implementation may be used as evidence during audits, but code is not automatically truth.

## Unknowns / Needs Verification

The following require verification before acceptance:

- Exact business rules for each core domain.
- Current behavior of Mobile, Dashboard, Web/public pages, and Supabase backend.
- Current gaps between frontend behavior and backend authority.
- Current RLS, RPC, storage, auth, and database enforcement boundaries.
- Current cross-surface inconsistencies.
- Current production readiness risks.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- The product definition is reviewed and approved by Mustafa / JoinFolk.
- Platform surfaces are confirmed.
- Core domains are confirmed.
- Authority boundaries are reviewed.
- Unknowns are resolved, explicitly deferred, or moved to tracked status documents.
- Related product specs are created or linked.
- Security-sensitive authority rules are verified against backend expectations.
- Any known platform inconsistencies are recorded.

## Open Questions

- Which product domain should be specified first?
- Which platform surface should be audited first?
- What facts from the prior audit summary require direct verification?
- What is the approval process for accepting this document as v1.0?

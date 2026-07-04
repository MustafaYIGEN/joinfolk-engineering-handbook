# Backend Architecture

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines the platform-level backend architecture specification draft for JoinFolk.

It records known Supabase responsibilities, backend authority boundaries, verification requirements, and unresolved backend architecture questions.

This document is not canonical. No backend architecture behavior is accepted by this document alone.

## Backend Architecture Definition

JoinFolk uses Supabase as backend infrastructure.

Supabase provides database, RPC, storage, and auth responsibilities.

Backend, RPC, and RLS must enforce security-sensitive behavior.

Frontend guards are UX helpers only.

Mobile, Dashboard, Web/Public, and Supabase must share the same business rules.

## Supabase Responsibilities

### Database

Supabase database is responsible for persistent data storage.

Exact backend schema is Unknown / Needs verification.

### RPC

Supabase RPC is responsible for backend-executed operations where applicable.

Current dashboard audit found many RPC-backed flows.

Exact RPC contracts are Unknown / Needs verification.

### RLS

RLS is responsible for database-level access control where applicable.

Backend/RPC/RLS must enforce security-sensitive behavior.

Exact RLS policies are Unknown / Needs verification.

### Auth

Supabase auth is responsible for authentication-related backend behavior where applicable.

Exact auth model is Unknown / Needs verification.

### Storage

Supabase storage is responsible for storage and media-related backend behavior where applicable.

Exact storage policies are Unknown / Needs verification.

## Backend Authority Model

### What Backend Must Enforce

Backend authority is required for:

- authentication-sensitive access
- authorization
- event lifecycle transitions
- viewer role security-sensitive behavior
- commerce authority
- ticketing authority
- reservation authority
- wallet/ownership authority
- check-in authority
- staff scanner authority
- ops/admin authority
- host identity transfer authority
- protected public/private visibility
- protected storage/media access
- security-sensitive notifications

### What Frontend Must Not Enforce Alone

Frontend guards must not enforce security-sensitive behavior alone.

Client-side guards may display warnings, disable actions, guide flows, or provide UX feedback.

Frontend-only authority is not accepted for protected access, ownership, lifecycle transitions, commerce, tickets, reservations, scanner actions, ops/admin actions, host identity transfer, storage/media access, or security-sensitive notifications.

### What Requires Verification

The following require backend verification before acceptance:

- database schema
- RPC contracts
- RLS policies
- storage policies
- auth model
- database trigger/function behavior
- migration status
- backend enforcement for each security-sensitive product domain

## RPC Architecture Draft

### Known Role

RPC-backed flows exist in the current dashboard audit.

RPCs may provide backend-executed operations for security-sensitive or consistency-sensitive behavior.

### Unknown Contracts

Exact RPC contracts are Unknown / Needs verification.

No RPC name, parameter, return value, permission rule, or side effect is accepted by this document.

### Verification Requirements

RPC verification must identify:

- purpose
- inputs
- outputs
- permissions
- side effects
- related tables or storage objects
- RLS interaction
- security-sensitive behavior
- callers across Mobile, Dashboard, Web/Public, and Supabase where applicable

## RLS / Permission Architecture Draft

### Known Role

RLS is part of the backend authority model where applicable.

Backend/RPC/RLS must enforce security-sensitive behavior.

### Unknown Policies

Exact RLS policies are Unknown / Needs verification.

No RLS policy behavior is accepted by this document.

### Verification Requirements

RLS and permission verification must identify:

- protected tables or records
- allowed read paths
- allowed write paths
- role or identity inputs
- ownership checks
- viewer role or persona dependencies where applicable
- RPC bypass or SECURITY DEFINER behavior where applicable
- storage and media access relationships where applicable

## Storage / Media Authority Draft

Supabase storage is part of backend infrastructure.

Protected storage/media access must be backend-enforced where security-sensitive.

Exact storage policies and media authority behavior are Unknown / Needs verification.

Storage and media behavior must be consistent with product domains such as media/gallery, public sharing, event lifecycle, viewer roles, staff scanner, and ops/admin where applicable.

## Auth and Identity Draft

Supabase auth is part of backend infrastructure.

Authentication-sensitive access must be backend-enforced.

Exact auth model and identity behavior are Unknown / Needs verification.

Auth behavior may interact with personas, tiers, viewer roles, staff scanner, ops/admin, and host identity transfer, but exact relationships are Unknown / Needs verification.

## Migration / Schema Governance Draft

Exact backend schema is not accepted.

Exact database trigger/function behavior is not accepted.

Exact migration status is not accepted.

Schema and migration changes must follow governance, patch policy, and do-not-touch rules.

Migration and schema verification must occur before accepting backend behavior.

## Relationship to Product Domains

### Event lifecycle

Backend authority is required for event lifecycle transitions.

Exact backend lifecycle enforcement is Unknown / Needs verification.

### Viewer roles

Backend authority is required for viewer role security-sensitive behavior.

Exact backend viewer role enforcement is Unknown / Needs verification.

### Commerce

Backend authority is required for commerce authority.

Exact backend commerce enforcement is Unknown / Needs verification.

### Ticketing

Backend authority is required for ticketing authority.

Exact backend ticketing enforcement is Unknown / Needs verification.

### Reservations

Backend authority is required for reservation authority.

Exact backend reservation enforcement is Unknown / Needs verification.

### Wallet/ownership

Backend authority is required for wallet/ownership authority.

Exact backend ownership enforcement is Unknown / Needs verification.

### Notifications

Backend authority is required for security-sensitive notifications.

Exact backend notification enforcement is Unknown / Needs verification.

### Venue/business tools

Venue/business tools may require backend authority where security-sensitive.

Exact backend venue/business enforcement is Unknown / Needs verification.

### Media/gallery

Protected storage/media access must be backend-enforced where security-sensitive.

Exact backend media/gallery enforcement is Unknown / Needs verification.

### Staff scanner

Backend authority is required for staff scanner authority and check-in authority.

Exact backend scanner enforcement is Unknown / Needs verification.

### Ops/admin

Backend authority is required for ops/admin authority.

Exact backend ops/admin enforcement is Unknown / Needs verification.

### Host identity transfer

Backend authority is required for host identity transfer authority.

Exact backend host identity transfer enforcement is Unknown / Needs verification.

### Public sharing

Backend authority is required for protected public/private visibility.

Exact backend public sharing enforcement is Unknown / Needs verification.

## Cross-Surface Backend Contract Requirements

### Mobile

Mobile must rely on backend authority for security-sensitive behavior.

Mobile backend contracts must match shared platform rules.

### Dashboard

Dashboard must rely on backend authority for security-sensitive behavior.

Current dashboard audit found many RPC-backed flows and client guards for auth/tier/ops/host/staff surfaces.

Dashboard client guards must be verified against backend enforcement.

### Web/Public

Web/Public surfaces must rely on backend authority for protected visibility and security-sensitive behavior.

Public display must not bypass backend enforcement.

### Supabase

Supabase database, RPC, RLS, storage, and auth must provide the backend authority required for security-sensitive behavior.

Exact Supabase backend contracts are Unknown / Needs verification.

## Security Risks

Frontend guards may be mistaken for security authority.

Unverified RPC contracts may allow unauthorized or inconsistent operations.

Unverified RLS policies may expose protected data or block legitimate access.

Unverified storage policies may expose protected media or files.

Unverified auth behavior may create incorrect identity or permission assumptions.

Unverified migrations, triggers, or functions may alter backend authority without accepted documentation.

## Determinism Risks

Mobile, Dashboard, Web/Public, and Supabase may rely on different business rule interpretations.

RPC-backed flows may behave differently from frontend assumptions.

RLS behavior may differ from client guards.

Storage/media access may diverge from event lifecycle, viewer role, or public visibility expectations.

Migration status may differ from documented schema assumptions.

## Current Known Implementation

Known implementation facts:

- JoinFolk uses Supabase as backend infrastructure.
- Supabase provides database, RPC, storage, and auth responsibilities.
- Current dashboard audit found many RPC-backed flows.
- Current dashboard audit found no complete backend/RLS verification yet.
- Current dashboard audit found client guards for auth/tier/ops/host/staff surfaces.

These facts are not accepted as canonical backend architecture rules.

## Unknowns / Needs Verification

The following are Unknown / Needs verification:

- Exact backend schema.
- Exact RPC contracts.
- Exact RLS policies.
- Exact storage policies.
- Exact auth model.
- Exact database trigger/function behavior.
- Exact migration status.
- Backend enforcement for authentication-sensitive access.
- Backend enforcement for authorization.
- Backend enforcement for event lifecycle transitions.
- Backend enforcement for viewer roles.
- Backend enforcement for commerce.
- Backend enforcement for ticketing.
- Backend enforcement for reservations.
- Backend enforcement for wallet/ownership.
- Backend enforcement for check-in and staff scanner.
- Backend enforcement for ops/admin.
- Backend enforcement for host identity transfer.
- Backend enforcement for protected public/private visibility.
- Backend enforcement for storage/media access.
- Backend enforcement for security-sensitive notifications.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- Supabase responsibilities are confirmed.
- Backend schema is documented or explicitly deferred.
- RPC contracts are documented or explicitly deferred.
- RLS policies are documented or explicitly deferred.
- Storage policies are documented or explicitly deferred.
- Auth model is documented or explicitly deferred.
- Database trigger/function behavior is documented or explicitly deferred.
- Migration status is documented or explicitly deferred.
- Backend enforcement is verified for security-sensitive product domains or explicitly deferred.
- Dashboard client guards are verified against backend enforcement.
- Open questions are resolved, explicitly deferred, or moved to tracked status documents.
- Mustafa / JoinFolk approves the document for acceptance.

## Open Questions

- What is the accepted backend schema?
- What RPC contracts are currently authoritative in implementation?
- What RLS policies enforce each security-sensitive domain?
- What storage policies protect media and files?
- What is the accepted auth and identity model?
- What migrations, triggers, and functions currently define backend behavior?
- Which backend area should be verified first?

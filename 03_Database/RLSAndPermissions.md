# RLS And Permissions

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines the platform-level RLS and permission specification draft for JoinFolk.

It records known permission authority requirements, verification requirements, and unresolved backend permission questions.

This document is not canonical. No RLS policy, permission rule, role input, identity input, ownership check, RPC interaction, storage relationship, or auth behavior is accepted by this document alone.

## RLS / Permission Definition

RLS is part of backend authority where applicable.

Permissions define who may read, write, mutate, access, or operate on protected platform data and actions.

JoinFolk uses Supabase/Postgres. Supabase provides database, RPC, storage, and auth responsibilities.

Exact RLS policies and exact permission rules are not accepted yet.

## Permission Authority Model

### What RLS May Enforce

RLS may enforce database-level read and write access where applicable.

RLS may enforce permission rules tied to auth inputs, ownership checks, personas, tiers, viewer roles, and other backend-verifiable context.

Exact RLS policies are Unknown / Needs verification.

### What RPC May Enforce

RPC may enforce backend-executed permission checks and security-sensitive operations where applicable.

RPC may interact with RLS or SECURITY DEFINER behavior where applicable.

Exact RPC permission behavior and SECURITY DEFINER behavior are Unknown / Needs verification.

### What Frontend Must Not Enforce Alone

Frontend guards are UX helpers only.

Frontend must not enforce security-sensitive behavior alone.

Frontend-only authority is not accepted for authentication-sensitive access, authorization, personas and tiers, lifecycle transitions, viewer roles, commerce, ticketing, reservations, wallet/ownership, check-in, staff scanner, ops/admin, host identity transfer, protected visibility, protected storage/media access, or security-sensitive notifications.

## Known Permission Domains

Security-sensitive domains requiring backend permission verification include:

- authentication-sensitive access
- authorization
- personas and tiers
- event lifecycle transitions
- viewer roles
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

Mobile, Dashboard, Web/Public, and Supabase must share the same permission meaning.

## Non-Accepted Permission Areas

The following are not accepted:

- exact RLS policies
- exact permission rules
- exact role/identity inputs
- exact ownership checks
- exact viewer role dependencies
- exact persona dependencies
- exact tier dependencies
- exact RPC bypass behavior
- exact SECURITY DEFINER behavior
- exact storage/media access relationships
- exact table-level read/write rules
- exact auth model

No permission behavior is canonical in this draft.

## RLS Verification Requirements

### Tables

Verify which tables are protected by RLS and what data each table controls.

### Read policies

Verify exact read policies before acceptance.

No read policy behavior is accepted by this document.

### Write policies

Verify exact write, update, insert, and delete policies before acceptance.

No write policy behavior is accepted by this document.

### Auth inputs

Verify which auth inputs are used by each policy or RPC permission check.

Exact auth behavior is Unknown / Needs verification.

### Ownership checks

Verify ownership checks for protected data and actions.

Exact ownership checks are Unknown / Needs verification.

### Persona/tier dependencies

Verify any persona or tier dependencies used by RLS, RPC, or permissions.

Exact persona and tier dependencies are Unknown / Needs verification.

### Viewer role dependencies

Verify any viewer role dependencies used by RLS, RPC, or permissions.

Exact viewer role dependencies are Unknown / Needs verification.

### RPC interaction

Verify how RPC behavior depends on, bypasses, or complements RLS.

Exact RPC interaction is Unknown / Needs verification.

### SECURITY DEFINER behavior

Verify any SECURITY DEFINER behavior and the authority it grants.

Exact SECURITY DEFINER behavior is Unknown / Needs verification.

### Storage/media relationship

Verify how storage/media permissions relate to database, RPC, RLS, and auth.

Exact storage/media permission behavior is Unknown / Needs verification.

## Relationship to Product Domains

### Personas and tiers

Personas and tiers require backend permission verification.

Exact permission behavior is Unknown / Needs verification.

### Event lifecycle

Event lifecycle transitions require backend permission verification.

Exact lifecycle permission behavior is Unknown / Needs verification.

### Viewer roles

Viewer roles require backend permission verification where security-sensitive.

Exact viewer role permission behavior is Unknown / Needs verification.

### Commerce

Commerce authority requires backend permission verification.

Exact commerce permission behavior is Unknown / Needs verification.

### Ticketing

Ticketing authority requires backend permission verification.

Exact ticketing permission behavior is Unknown / Needs verification.

### Reservations

Reservation authority requires backend permission verification.

Exact reservation permission behavior is Unknown / Needs verification.

### Wallet/ownership

Wallet/ownership authority requires backend permission verification.

Exact ownership permission behavior is Unknown / Needs verification.

### Notifications

Security-sensitive notifications require backend permission verification.

Exact notification permission behavior is Unknown / Needs verification.

### Venue/business tools

Venue/business tools require backend permission verification where security-sensitive.

Exact venue/business permission behavior is Unknown / Needs verification.

### Media/gallery

Protected storage/media access requires backend permission verification.

Exact media/gallery permission behavior is Unknown / Needs verification.

### Staff scanner

Staff scanner and check-in authority require backend permission verification.

Exact scanner permission behavior is Unknown / Needs verification.

### Ops/admin

Ops/admin authority requires backend permission verification.

Exact ops/admin permission behavior is Unknown / Needs verification.

### Host identity transfer

Host identity transfer authority requires backend permission verification.

Exact transfer permission behavior is Unknown / Needs verification.

### Public sharing

Protected public/private visibility requires backend permission verification.

Exact public sharing permission behavior is Unknown / Needs verification.

## Frontend Guard Relationship

Current dashboard audit found client guards for auth/tier/ops/host/staff surfaces.

Frontend guards are UX helpers only.

Every security-sensitive frontend guard must be verified against backend, RPC, or RLS enforcement before the related permission rule is accepted.

Frontend guard behavior must not be treated as proof of permission behavior.

## RPC / SECURITY DEFINER Relationship

Exact RPC bypass or SECURITY DEFINER behavior is not accepted.

RPC and SECURITY DEFINER behavior must be verified before acceptance because they may affect backend authority.

Verification must identify whether RPCs depend on RLS, bypass RLS, complement RLS, or apply separate permission checks.

## Storage / Media Permission Relationship

Exact storage/media access relationships are not accepted.

Protected storage/media access requires backend permission verification.

Verification must identify how storage policies, database records, RPC behavior, auth, and RLS interact where media/gallery behavior is security-sensitive.

## Security Risks

Unverified RLS policies may expose protected data or block legitimate access.

Unverified permission rules may grant unauthorized authority.

Frontend guards may be mistaken for backend permission enforcement.

Unverified RPC or SECURITY DEFINER behavior may bypass expected RLS protections.

Unverified storage/media permission behavior may expose protected media or files.

Unverified ownership, viewer role, persona, tier, or auth dependencies may create inconsistent authorization behavior.

## Determinism Risks

Mobile, Dashboard, Web/Public, and Supabase may interpret permission meaning differently.

Frontend guards may diverge from backend permission enforcement.

RPC behavior may diverge from RLS behavior.

Storage/media permissions may diverge from database permissions.

Auth, persona, tier, viewer role, and ownership inputs may be interpreted inconsistently.

## Current Known Implementation

Known implementation facts:

- JoinFolk uses Supabase/Postgres.
- Supabase provides database, RPC, storage, and auth responsibilities.
- RLS is part of backend authority where applicable.
- Backend, RPC, and RLS must enforce security-sensitive behavior.
- Frontend guards are UX helpers only.
- Current dashboard audit found client guards for auth/tier/ops/host/staff surfaces.
- Current dashboard audit did not complete backend/RLS verification.

These facts are not accepted as canonical RLS or permission rules.

## Unknowns / Needs Verification

The following are Unknown / Needs verification:

- Exact RLS policies.
- Exact permission rules.
- Exact role/identity inputs.
- Exact ownership checks.
- Exact viewer role dependencies.
- Exact persona dependencies.
- Exact tier dependencies.
- Exact RPC bypass behavior.
- Exact SECURITY DEFINER behavior.
- Exact storage/media access relationships.
- Exact table-level read/write rules.
- Exact auth model.
- Cross-surface permission consistency across Mobile, Dashboard, Web/Public, and Supabase.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- RLS-protected tables are documented or explicitly deferred.
- Read policies are documented or explicitly deferred.
- Write policies are documented or explicitly deferred.
- Auth inputs are documented or explicitly deferred.
- Ownership checks are documented or explicitly deferred.
- Persona/tier dependencies are documented or explicitly deferred.
- Viewer role dependencies are documented or explicitly deferred.
- RPC interaction is documented or explicitly deferred.
- SECURITY DEFINER behavior is documented or explicitly deferred.
- Storage/media permission relationships are documented or explicitly deferred.
- Frontend guards are verified against backend enforcement.
- Cross-surface permission consistency is audited.
- Open questions are resolved, explicitly deferred, or moved to tracked status documents.
- Mustafa / JoinFolk approves the document for acceptance.

## Open Questions

- Which tables have RLS enabled?
- What read and write policies exist for each protected table?
- What auth inputs are used by current permissions?
- What ownership checks exist?
- What persona, tier, or viewer role dependencies exist?
- Which RPCs bypass, depend on, or complement RLS?
- Which SECURITY DEFINER functions exist?
- How are storage/media permissions enforced?

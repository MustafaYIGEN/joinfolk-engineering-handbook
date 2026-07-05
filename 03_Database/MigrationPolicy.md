# Migration Policy

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines the platform-level database migration policy draft for JoinFolk.

It records migration governance, approval expectations, verification requirements, safety rules, and unresolved migration questions.

This document is not canonical. No migration history, production database state, rollback strategy, tooling, CI/CD behavior, schema state, RPC state, RLS state, storage state, or auth state is accepted by this document alone.

## Migration Policy Definition

A migration is any change that alters Supabase/Postgres database, RPC, storage, auth-related behavior, or seed/reference data where applicable.

Migration policy must support auditability, rollback thinking, verification, and cross-surface consistency.

Exact migration status is not accepted yet.

## Migration Authority Model

### What Migrations May Change

Migration changes can affect:

- tables
- columns
- constraints
- indexes
- triggers/functions
- RPCs
- RLS policies
- storage policies
- auth-related behavior
- seed/reference data where applicable

### What Requires Explicit Approval

No production SQL or migration should be applied without explicit approval.

Explicit approval is required before applying migrations that affect schema, RPCs, functions, RLS policies, storage policies, auth-related behavior, seed/reference data, production data, or security-sensitive product domains.

### What Must Not Be Changed Silently

The following must not be changed silently:

- production database state
- applied migrations
- pending migrations
- schema objects
- RPCs or functions
- RLS policies
- storage policies
- auth-related behavior
- seed/reference data where applicable
- security-sensitive product domain behavior

## Migration Lifecycle Draft

### Proposal

A migration proposal must state what will change, why it will change, affected domains, risk level, required approval, verification plan, and rollback thinking.

### Review

Migration review must examine affected schema, RPCs/functions, RLS policies, storage policies, auth-related behavior, and product domains where applicable.

### Verification

Verification must confirm the expected pre-change and post-change state.

Exact verification method is Unknown / Needs verification.

### Application

No production SQL or migration should be applied without explicit approval.

Exact migration application process is Unknown / Needs verification.

### Post-application validation

Post-application validation must confirm that the intended database, RPC, RLS, storage, auth, and product-domain effects occurred where applicable.

Exact post-application validation process is Unknown / Needs verification.

### Rollback planning

Rollback thinking is required before applying a migration.

Exact rollback strategy is Unknown / Needs verification.

Rollback may require reversal, forward fix, operational mitigation, or other approved recovery path, but no exact strategy is accepted by this document.

## High-Risk Migration Areas

### Schema

Schema changes may affect tables, columns, constraints, indexes, and relationships.

Exact schema state is Unknown / Needs verification.

### RPCs/functions

RPC and function changes may affect backend authority and side effects.

Exact RPC/function state is Unknown / Needs verification.

### RLS policies

RLS policy changes may affect protected data access.

Exact RLS state is Unknown / Needs verification.

### Storage policies

Storage policy changes may affect protected media or files.

Exact storage policy state is Unknown / Needs verification.

### Auth/identity behavior

Auth or identity-related changes may affect authentication-sensitive access, authorization, personas, tiers, viewer roles, ops/admin, or host identity transfer.

Exact auth/identity behavior is Unknown / Needs verification.

### Security-sensitive product domains

Migration changes are high-risk when they affect security-sensitive product domains.

Security-sensitive product domains include personas and tiers, event lifecycle, viewer roles, commerce, ticketing, reservations, wallet/ownership, notifications, venue/business tools, media/gallery, staff scanner, ops/admin, host identity transfer, and public sharing.

## Production Safety Rules

No production SQL or migration should be applied without explicit approval.

Production migration work must not rely on frontend guards as security authority.

Backend, RPC, and RLS must enforce security-sensitive behavior.

Production-impacting migration work must identify risk, verification, and rollback thinking before application.

Exact production database state is Unknown / Needs verification.

## Verification Requirements

Migration verification must identify:

- intended change
- affected environments
- affected schema objects
- affected RPCs/functions
- affected RLS policies
- affected storage policies
- affected auth-related behavior
- affected seed/reference data where applicable
- affected product domains
- security-sensitive impact
- cross-surface consistency impact
- pre-application validation
- post-application validation
- rollback or recovery thinking

Exact verification process is Unknown / Needs verification.

## Relationship to Database Specs

### Canonical schema

Migration policy supports future canonical schema acceptance.

Exact schema state is not accepted.

### RPC contracts

Migration changes may create, alter, or remove RPCs/functions.

Exact RPC contracts are not accepted.

### RLS and permissions

Migration changes may create, alter, or remove RLS policies and permission behavior.

Exact RLS and permission behavior is not accepted.

### Storage model

Migration changes may affect storage policies or storage-related database relationships.

Exact storage model is not accepted.

## Relationship to Product Domains

Migration changes may affect:

- personas and tiers
- event lifecycle
- viewer roles
- commerce
- ticketing
- reservations
- wallet/ownership
- notifications
- venue/business tools
- media/gallery
- staff scanner
- ops/admin
- host identity transfer
- public sharing

Any migration affecting these domains must be reviewed for security, determinism, and cross-surface consistency impact.

## Cross-Surface Consistency Requirements

### Mobile

Migration changes must not create untracked divergence between Mobile behavior and backend state.

### Dashboard

Migration changes must not create untracked divergence between Dashboard behavior and backend state.

Current dashboard audit did not complete backend/RLS verification.

### Web/Public

Migration changes must not create untracked divergence between Web/Public behavior and backend state.

Protected public/private visibility must remain backend-enforced.

### Supabase Backend

Migration changes must preserve or intentionally modify Supabase backend behavior with explicit approval and verification.

Exact Supabase backend state is Unknown / Needs verification.

## Security Risks

Unapproved migrations may alter security-sensitive behavior.

Unverified RLS changes may expose protected data.

Unverified RPC/function changes may bypass permissions or create unintended side effects.

Unverified storage policy changes may expose protected media or files.

Unverified auth-related changes may alter identity or permission behavior.

Unverified migration status may make handbook or implementation assumptions inaccurate.

## Determinism Risks

Mobile, Dashboard, Web/Public, and Supabase may assume different backend state.

Migration state may differ across environments.

Schema, RPC, RLS, storage, or auth state may diverge from documented product rules.

Frontend guards may continue to assume old backend behavior after migration changes.

## Operational Risks

Production migrations may be difficult to reverse.

Missing rollback thinking may increase incident impact.

Unverified production state may cause a migration to apply incorrectly.

Seed/reference data changes may affect product behavior where applicable.

Unaccepted CI/CD migration behavior may create deployment uncertainty.

## Current Known Implementation

Known implementation facts:

- JoinFolk uses Supabase/Postgres.
- Supabase provides database, RPC, storage, and auth responsibilities.
- Backend, RPC, and RLS must enforce security-sensitive behavior.
- Frontend guards are UX helpers only.
- Current dashboard audit did not complete backend/RLS verification.

These facts are not accepted as canonical migration policy or migration state.

## Unknowns / Needs Verification

The following are Unknown / Needs verification:

- Exact migration status.
- Exact applied migration list.
- Exact pending migration list.
- Exact production database state.
- Exact schema state.
- Exact RPC state.
- Exact RLS state.
- Exact storage state.
- Exact auth state.
- Exact rollback strategy.
- Exact migration tooling.
- Exact CI/CD migration behavior.
- Exact verification process.
- Exact post-application validation process.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- Migration status is verified or explicitly deferred.
- Applied migration list is documented or explicitly deferred.
- Pending migration list is documented or explicitly deferred.
- Production database state is documented or explicitly deferred.
- Schema/RPC/RLS/storage/auth state is documented or explicitly deferred.
- Migration approval process is accepted.
- Verification requirements are accepted.
- Rollback planning requirements are accepted.
- Production safety rules are accepted.
- Cross-surface consistency requirements are reviewed.
- Open questions are resolved, explicitly deferred, or moved to tracked status documents.
- Mustafa / JoinFolk approves the document for acceptance.

## Open Questions

- What migration status is current?
- Which migrations are applied?
- Which migrations are pending?
- What is the verified production database state?
- What migration tooling and process are used?
- What approval record is required before production SQL or migration application?
- What rollback or recovery standard should be required for each migration type?

# Canonical Schema

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines the platform-level database schema specification draft for JoinFolk.

It records known schema domains, known table or concept names, verification requirements, and unresolved database questions.

This document is not canonical. No table, column, relationship, constraint, index, trigger, function, RLS policy, or migration status is accepted by this document alone.

## Canonical Schema Definition

A canonical schema is the accepted database model for JoinFolk platform data.

The canonical schema must define verified tables, columns, relationships, constraints, indexes, triggers/functions, RLS policies, and migration status when accepted.

The current schema is not accepted yet.

## Schema Authority Model

### What This Document May Define

This document may define database schema expectations after verification.

It may record draft schema domains, known table or concept names, unresolved questions, and verification requirements.

### What Requires Supabase Verification

The following require Supabase/Postgres verification before acceptance:

- exact table list
- exact column definitions
- exact constraints
- exact indexes
- exact triggers/functions
- exact relationships
- exact RLS policies
- exact migration status
- exact RPC relationships to schema objects
- exact storage relationships where applicable

### What Must Not Be Inferred From Frontend Code

Frontend code must not be used alone to infer canonical schema.

Frontend references may provide evidence, but they do not prove accepted table names, columns, constraints, indexes, triggers/functions, RLS policies, or migration status.

No schema behavior should be accepted without Supabase/Postgres verification.

## Known Schema Domains

Database schema supports product domains including:

- profiles / user profiles
- personas and tiers
- events
- event lifecycle
- viewer roles or role-related data
- commerce model
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

These domains require verification before acceptance.

## Known Table / Concept Names Draft

Prior project context mentions tables or concepts such as:

- `events`
- `profiles`
- `user_profiles`
- `tickets`
- `reservations`
- `share_groups`
- `event_modules`
- `event_ticket_products_v1`
- staff assignments
- `notifications_v1`

These names are known concepts only.

They must not be treated as accepted canonical schema until verified.

## Non-Accepted Schema Areas

The following are not accepted:

- exact backend schema
- exact table list
- exact column definitions
- exact constraints
- exact indexes
- exact triggers/functions
- exact relationships
- exact RLS policies
- exact migration status

No table name is canonical in this draft.

## Relationship to Product Domains

### Personas and tiers

Schema may support profiles, personas, tiers, and ops/admin flags.

Exact schema is Unknown / Needs verification.

### Event lifecycle

Schema may support events and lifecycle status.

Exact schema is Unknown / Needs verification.

### Viewer roles

Schema may support viewer roles or role-related data.

Exact schema is Unknown / Needs verification.

### Commerce

Schema may support commerce model behavior.

Exact schema is Unknown / Needs verification.

### Ticketing

Schema may support ticketing, ticket products, ticket claims, requests, ownership, and check-in behavior.

Exact schema is Unknown / Needs verification.

### Reservations

Schema may support event reservations and venue reservations.

Exact schema is Unknown / Needs verification.

### Wallet/ownership

Schema may support wallet ownership and ticket ownership.

Exact schema is Unknown / Needs verification.

### Notifications

Schema may support notifications or notification-related concepts.

Exact schema is Unknown / Needs verification.

### Venue/business tools

Schema may support venue/business tools.

Exact schema is Unknown / Needs verification.

### Media/gallery

Schema may support media/gallery behavior and storage relationships.

Exact schema is Unknown / Needs verification.

### Staff scanner

Schema may support staff scanner and check-in behavior.

Exact schema is Unknown / Needs verification.

### Ops/admin

Schema may support ops/admin authority.

Exact schema is Unknown / Needs verification.

### Host identity transfer

Schema may support host identity transfer.

Exact schema is Unknown / Needs verification.

### Public sharing

Schema may support public sharing.

Exact schema is Unknown / Needs verification.

## Schema Verification Requirements

Schema verification must identify:

- current table list
- current columns
- current relationships
- current constraints
- current indexes
- current triggers/functions
- current RLS policies
- current RPC dependencies
- current storage dependencies where applicable
- current product domain ownership
- current gaps between schema and draft product specs

Verification must distinguish observed schema from accepted schema.

## Migration Verification Requirements

Migration verification must identify:

- current migration status
- applied migrations
- unapplied or pending migrations where applicable
- schema objects created or modified by migrations
- functions and triggers created or modified by migrations
- RLS policies created or modified by migrations
- migration risks for security-sensitive domains

Exact migration status is Unknown / Needs verification.

## RLS / Constraint Relationship

Backend, RPC, and RLS must enforce security-sensitive behavior.

Constraints may enforce structural integrity where applicable.

RLS may enforce access control where applicable.

Exact RLS policies and exact constraints are Unknown / Needs verification.

RLS and constraints must be verified before any schema area is accepted as canonical.

## Security Risks

Unverified schema assumptions may hide missing access controls.

Unverified RLS policies may expose protected data or block legitimate access.

Unverified relationships may allow incorrect ownership, event, ticket, reservation, wallet, scanner, ops/admin, or public sharing behavior.

Unverified triggers/functions may mutate security-sensitive state without accepted documentation.

Unverified migration status may make accepted schema claims inaccurate.

## Determinism Risks

Mobile, Dashboard, Web/Public, and Supabase may assume different schema meanings.

Frontend references may drift from actual database schema.

RPC behavior may depend on tables or functions not documented as accepted schema.

RLS behavior may differ from frontend guard assumptions.

Migrations may create schema differences across environments.

## Current Known Implementation

Known implementation facts:

- JoinFolk uses Supabase/Postgres.
- Supabase provides database, RPC, storage, and auth responsibilities.
- Backend, RPC, and RLS must enforce security-sensitive behavior.
- Prior project context mentions `events`, `profiles`, `user_profiles`, `tickets`, `reservations`, `share_groups`, `event_modules`, `event_ticket_products_v1`, staff assignments, and `notifications_v1`.
- Current dashboard audit did not complete backend/RLS verification.

These facts are not accepted as canonical schema rules.

## Unknowns / Needs Verification

The following are Unknown / Needs verification:

- Exact backend schema.
- Exact table list.
- Exact column definitions.
- Exact constraints.
- Exact indexes.
- Exact triggers/functions.
- Exact relationships.
- Exact RLS policies.
- Exact migration status.
- Exact schema relationship to RPC contracts.
- Exact schema relationship to storage policies.
- Exact schema ownership for each product domain.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- Supabase/Postgres schema is verified.
- Table list is documented or explicitly deferred.
- Column definitions are documented or explicitly deferred.
- Relationships are documented or explicitly deferred.
- Constraints are documented or explicitly deferred.
- Indexes are documented or explicitly deferred.
- Triggers/functions are documented or explicitly deferred.
- RLS policies are documented or explicitly deferred.
- Migration status is documented or explicitly deferred.
- Known table/concept names are either verified, rejected, or explicitly deferred.
- Security-sensitive schema areas are reviewed.
- Open questions are resolved, explicitly deferred, or moved to tracked status documents.
- Mustafa / JoinFolk approves the document for acceptance.

## Open Questions

- What is the verified current table list?
- Which mentioned table or concept names are real schema objects?
- What columns and relationships exist for each product domain?
- What constraints and indexes enforce data integrity?
- What triggers/functions mutate security-sensitive state?
- What RLS policies protect each table?
- What migration status should be treated as current?

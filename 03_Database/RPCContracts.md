# RPC Contracts

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines the platform-level RPC contract specification draft for JoinFolk.

It records known RPC-backed flow facts, known RPC or RPC-like names, verification requirements, and unresolved contract questions.

This document is not canonical. No RPC name, parameter, return shape, permission rule, side effect, RLS interaction, SECURITY DEFINER behavior, table relationship, storage relationship, or caller list is accepted by this document alone.

## RPC Contract Definition

An RPC contract is the accepted definition of a backend-executed operation.

An accepted RPC contract must define the RPC name, purpose, inputs, outputs, permissions, side effects, RLS interaction, SECURITY DEFINER behavior, tables/storage touched, and callers.

Exact RPC contracts are not accepted yet.

## RPC Authority Model

### What RPCs May Enforce

RPCs may enforce backend behavior for security-sensitive or consistency-sensitive platform operations.

Backend, RPC, and RLS must enforce security-sensitive behavior.

RPC-backed enforcement may apply to event lifecycle, viewer roles, commerce, ticketing, reservations, wallet/ownership, notifications, venue/business tools, media/gallery, staff scanner, ops/admin, host identity transfer, and public sharing.

### What Frontend Must Not Enforce Alone

Frontend guards are UX helpers only.

Frontend must not enforce security-sensitive behavior alone.

Frontend-only authority is not accepted for RPC-backed operations, protected access, permissions, ownership, lifecycle transitions, commerce, ticketing, reservations, scanner actions, ops/admin actions, host identity transfer, storage/media access, notifications, or public/private visibility.

### What Requires Verification

Every RPC requires verification before acceptance.

Verification must include name, purpose, inputs, outputs, permissions, side effects, RLS interaction, SECURITY DEFINER behavior, tables/storage touched, and callers.

## Known RPC / RPC-like Names Draft

Prior audit/project context mentioned RPC or RPC-like names such as:

- `transition_event_status_v2`
- `publish_event_with_groups_and_snapshot_v2`
- `get_event_stats_v1`
- `get_event_ticket_queue_v2`
- `checkin_ticket_by_id_v2`
- `get_event_modules_v1`
- `set_event_modules_v1`
- `clear_event_module_v1`
- `get_event_ticket_products_v1`
- `upsert_event_ticket_product_v2`
- `get_event_product_section_usage_v1`
- `get_venue_layout_v1`
- `create_visual_venue_layout_v1`
- `save_venue_layout_v1`
- `link_event_venue_layout_v1`
- `list_my_venues_v1`
- `get_venue_v1`
- `create_venue_v1`
- `update_venue_v1`
- `archive_venue_v1`
- `get_venue_reservations_v1`
- `update_venue_reservation_status_v1`
- `list_venue_media_v1`
- `add_venue_media_v1`
- `remove_venue_media_v1`
- `update_venue_media_v1`
- `get_event_reservations_v1`
- `update_reservation_status_v1`
- `dm_get_conversations_v1`
- `dm_get_messages_v1`
- `dm_send_message_v1`
- `dm_mark_read_v1`
- `dm_get_unread_count_v1`
- `ops_resolve_transfer_recipient_v1`
- `ops_approve_transfer_v1`
- `ops_reject_transfer_v1`
- `admin_execute_host_identity_transfer_v1`
- `get_event_share`

These names are known concepts only.

They must not be treated as accepted canonical RPC contracts until verified.

## Non-Accepted RPC Areas

The following are not accepted:

- exact RPC contracts
- exact RPC names as canonical
- exact RPC parameters
- exact RPC return shapes
- exact RPC permissions
- exact RPC side effects
- exact RPC relationship to RLS
- exact RPC relationship to database tables
- exact SECURITY DEFINER behavior
- exact callers across Mobile, Dashboard, Web/Public, and Supabase

No RPC name is canonical in this draft.

## RPC Verification Requirements

### Name

Verify the exact RPC name in Supabase/Postgres before acceptance.

### Purpose

Verify the operation the RPC is intended to perform.

### Inputs

Verify all parameters, types, required values, optional values, and validation expectations.

### Outputs

Verify return shape, result semantics, error behavior, and status behavior.

### Permissions

Verify who may call the RPC and under what conditions.

### Side effects

Verify all data, storage, notification, ownership, lifecycle, or permission side effects.

### RLS interaction

Verify whether and how the RPC depends on, bypasses, or complements RLS.

### SECURITY DEFINER behavior

Verify whether the RPC uses SECURITY DEFINER behavior and what authority it grants.

### Tables/storage touched

Verify all tables, views, functions, triggers, and storage objects read or mutated by the RPC.

### Callers

Verify callers across Mobile, Dashboard, Web/Public, and Supabase where applicable.

## Relationship to Product Domains

### Event lifecycle

RPCs may support event lifecycle behavior.

Exact lifecycle RPC contracts are Unknown / Needs verification.

### Viewer roles

RPCs may support viewer role behavior.

Exact viewer role RPC contracts are Unknown / Needs verification.

### Commerce

RPCs may support commerce behavior.

Exact commerce RPC contracts are Unknown / Needs verification.

### Ticketing

RPCs may support ticketing behavior.

Exact ticketing RPC contracts are Unknown / Needs verification.

### Reservations

RPCs may support reservation behavior.

Exact reservation RPC contracts are Unknown / Needs verification.

### Wallet/ownership

RPCs may support wallet/ownership behavior.

Exact ownership RPC contracts are Unknown / Needs verification.

### Notifications

RPCs may support notification behavior.

Exact notification RPC contracts are Unknown / Needs verification.

### Venue/business tools

RPCs may support venue/business tool behavior.

Exact venue/business RPC contracts are Unknown / Needs verification.

### Media/gallery

RPCs may support media/gallery behavior.

Exact media/gallery RPC contracts are Unknown / Needs verification.

### Staff scanner

RPCs may support staff scanner and check-in behavior.

Exact scanner RPC contracts are Unknown / Needs verification.

### Ops/admin

RPCs may support ops/admin behavior.

Exact ops/admin RPC contracts are Unknown / Needs verification.

### Host identity transfer

RPCs may support host identity transfer behavior.

Exact host identity transfer RPC contracts are Unknown / Needs verification.

### Public sharing

RPCs may support public sharing behavior.

Exact public sharing RPC contracts are Unknown / Needs verification.

## RPC / RLS Relationship Draft

Backend, RPC, and RLS must enforce security-sensitive behavior.

Exact RPC relationship to RLS is Unknown / Needs verification.

No RPC should be accepted until its RLS interaction is documented or explicitly deferred.

SECURITY DEFINER behavior is high-risk and must be verified before acceptance.

## RPC / Schema Relationship Draft

Exact RPC relationship to database tables is Unknown / Needs verification.

RPC verification must identify tables, views, functions, triggers, and storage objects read or mutated by each RPC.

Frontend references are not sufficient to accept schema relationships.

## RPC / Cross-Surface Caller Draft

Exact callers across Mobile, Dashboard, Web/Public, and Supabase are Unknown / Needs verification.

RPC caller verification must identify which surfaces call each RPC and whether each caller uses the RPC consistently with shared business rules.

No caller list is accepted by this document alone.

## Security Risks

Unverified RPC permissions may allow unauthorized operations.

Unverified side effects may mutate security-sensitive state.

Unverified RLS interaction may bypass expected protections.

Unverified SECURITY DEFINER behavior may grant excessive authority.

Unverified caller lists may leave exposed or unintended invocation paths.

Unverified RPC relationships to schema and storage may hide ownership, ticketing, reservation, scanner, ops/admin, or host identity transfer risks.

## Determinism Risks

Mobile, Dashboard, Web/Public, and Supabase may call or interpret RPCs differently.

Frontend guards may drift from RPC enforcement.

RPC return shapes may be assumed differently across surfaces.

RPC side effects may conflict with accepted product specs if not documented.

RPC and RLS behavior may diverge from frontend expectations.

## Current Known Implementation

Known implementation facts:

- JoinFolk uses Supabase RPC-backed flows.
- Current dashboard audit found many RPC-backed flows.
- Backend, RPC, and RLS must enforce security-sensitive behavior.
- Frontend guards are UX helpers only.
- Current dashboard audit did not complete backend/RLS verification.
- Prior audit/project context mentioned the RPC or RPC-like names listed in this document.

These facts are not accepted as canonical RPC contracts.

## Unknowns / Needs Verification

The following are Unknown / Needs verification:

- Exact RPC contracts.
- Exact RPC names as canonical.
- Exact RPC parameters.
- Exact RPC return shapes.
- Exact RPC permissions.
- Exact RPC side effects.
- Exact RPC relationship to RLS.
- Exact RPC relationship to database tables.
- Exact SECURITY DEFINER behavior.
- Exact callers across Mobile, Dashboard, Web/Public, and Supabase.
- Exact RPC relationship to storage.
- Exact RPC relationship to product domains.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- RPC list is verified.
- RPC names are documented or explicitly deferred.
- RPC purposes are documented or explicitly deferred.
- RPC inputs are documented or explicitly deferred.
- RPC outputs are documented or explicitly deferred.
- RPC permissions are documented or explicitly deferred.
- RPC side effects are documented or explicitly deferred.
- RPC/RLS interactions are documented or explicitly deferred.
- SECURITY DEFINER behavior is documented or explicitly deferred.
- Tables/storage touched are documented or explicitly deferred.
- Cross-surface callers are documented or explicitly deferred.
- Known RPC-like names are verified, rejected, or explicitly deferred.
- Open questions are resolved, explicitly deferred, or moved to tracked status documents.
- Mustafa / JoinFolk approves the document for acceptance.

## Open Questions

- Which mentioned RPC-like names exist in Supabase/Postgres?
- What are the exact contracts for each verified RPC?
- Which RPCs are SECURITY DEFINER?
- Which RPCs read or mutate security-sensitive data?
- Which RPCs bypass, depend on, or complement RLS?
- Which surfaces call each RPC?
- Which RPC contract should be verified first?

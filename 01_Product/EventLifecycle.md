# Event Lifecycle

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines the platform-level draft specification for the JoinFolk event lifecycle.

It records known lifecycle facts, current implementation signals, and unresolved verification requirements.

This document is not canonical. No event lifecycle behavior is accepted by this document alone.

## Definitions

### Event Lifecycle

The event lifecycle is the set of statuses and transitions that describe an event from creation through public availability, live operation, ending, and archive state.

### Event Status

An event status is the lifecycle value assigned to an event.

Known statuses are `draft`, `published`, `live`, `ended`, and `archived`.

### Lifecycle Transition

A lifecycle transition is a change from one event status to another.

Exact transition rules are Unknown / Needs verification.

### Feed-Visible Event

A feed-visible event is an event expected to appear in discovery or feed surfaces.

Feed-visible events are expected to include `published` and `live` events.

### Public Event

A public event is an event visible through public or shared surfaces.

Exact public visibility rules are Unknown / Needs verification.

### Managed Event

A managed event is an event available to authorized dashboard users for creation, editing, publishing, lifecycle transitions, or event management.

Exact management permission rules are Unknown / Needs verification.

## Known Statuses

### draft

`draft` is a known event status.

Draft events are not public feed-visible.

### published

`published` is a known event status.

Published events are expected to be feed-visible.

### live

`live` is a known event status.

Live events are expected to be feed-visible.

### ended

`ended` is a known event status.

Exact visibility and management behavior for ended events is Unknown / Needs verification.

### archived

`archived` is a known event status.

Archived events are not active public feed-visible.

## Canonical Rules Draft

### What Is Known

JoinFolk has an event lifecycle.

Known event statuses are `draft`, `published`, `live`, `ended`, and `archived`.

Event visibility depends on lifecycle status.

Feed-visible events are expected to include `published` and `live` events.

Draft events are not public feed-visible.

Archived events are not active public feed-visible.

Dashboard supports event creation, editing, publishing, lifecycle transitions, and event management.

Mobile consumes event lifecycle through discovery, detail, participation, media, and event interaction flows.

Lifecycle rules must be shared consistently across Mobile, Dashboard, Web/Public, and Supabase.

### What Is Not Yet Accepted

The exact transition graph is not accepted.

Exact backend schema, trigger, RLS, and RPC behavior are not accepted.

Exact public share visibility rules are not accepted.

Exact management permission rules are not accepted.

Exact revert behavior is not accepted.

No rule in this document is canonical.

## Lifecycle Authority Model

### What Frontend May Display

Frontend surfaces may display lifecycle status, lifecycle-dependent labels, lifecycle-dependent actions, and lifecycle-dependent UX state.

Frontend surfaces may guide users through event creation, editing, publishing, management, discovery, detail, participation, media, and event interaction flows.

Client-side lifecycle display is UX only.

### What Backend Must Enforce

Supabase, RPC, and backend behavior must be the authority for lifecycle transitions.

Backend enforcement is required for status changes, security-sensitive visibility, public access, management permissions, and lifecycle-dependent protected operations.

### What Must Never Be Frontend-Only

The following must never be frontend-only:

- lifecycle transition authority
- event publication authority
- live-state authority
- end-state authority
- archive authority
- public feed visibility authority
- protected event management authority
- security-sensitive event visibility authority

## Known Transition Concepts

### publish

Publishing is a known lifecycle concept.

There is an RPC named `publish_event_with_groups_and_snapshot_v2` in dashboard audit output.

Exact publish transition behavior is Unknown / Needs verification.

### go live

Going live is a known lifecycle concept.

Exact go-live transition behavior is Unknown / Needs verification.

### end

Ending an event is a known lifecycle concept.

Exact end transition behavior is Unknown / Needs verification.

### archive

Archiving an event is a known lifecycle concept.

Exact archive transition behavior is Unknown / Needs verification.

### possible revert behavior

Some prior implementation mentions a live-to-published revert behavior with a limited window.

The exact revert window and accepted revert rules are Unknown / Needs verification.

This behavior must not be treated as accepted until verified and approved.

## Visibility Rules Draft

### Feed Visibility

Feed-visible events are expected to include `published` and `live` events.

Draft events are not public feed-visible.

Archived events are not active public feed-visible.

Exact feed visibility rules for `ended` events are Unknown / Needs verification.

### Public Share Visibility

Public event sharing exists as a platform domain.

Exact public share visibility rules by lifecycle status are Unknown / Needs verification.

### Dashboard Visibility

Dashboard supports event creation, editing, publishing, lifecycle transitions, and event management.

Exact dashboard visibility rules by lifecycle status and permission are Unknown / Needs verification.

### Mobile Visibility

Mobile consumes event lifecycle through discovery, detail, participation, media, and event interaction flows.

Exact mobile visibility rules by lifecycle status are Unknown / Needs verification.

## Cross-Surface Consistency Requirements

### Mobile

Mobile must use the same lifecycle statuses and visibility semantics as the rest of the platform.

Mobile-only lifecycle authority is not accepted.

### Dashboard

Dashboard must use the same lifecycle statuses and transition semantics as the rest of the platform.

Dashboard lifecycle actions must be backed by backend authority.

### Web/Public

Web/public surfaces must use the same lifecycle visibility semantics as the rest of the platform.

Public event display must not bypass backend visibility rules.

### Supabase Backend

Supabase database, RPC, RLS, triggers, storage, and auth-related behavior must enforce lifecycle-sensitive authority where applicable.

Exact backend schema, trigger, RLS, and RPC behavior are Unknown / Needs verification.

## Security Risks

Frontend-only lifecycle authority may allow unauthorized event publication, live transitions, ending, archiving, or visibility changes.

Incorrect visibility rules may expose draft, archived, or otherwise non-public events.

Incorrect dashboard permissions may allow unauthorized event management.

Unverified RPC or RLS behavior may create gaps between displayed lifecycle state and enforced lifecycle state.

## Determinism Risks

Mobile, Dashboard, Web/Public, and Supabase may interpret lifecycle status differently.

Feed visibility may drift from backend lifecycle rules.

Public share behavior may diverge from dashboard or mobile behavior.

Possible revert behavior may be implemented inconsistently if the accepted transition graph is not defined.

## Current Known Implementation

Known implementation facts:

- Dashboard supports event creation, editing, publishing, lifecycle transitions, and event management.
- Mobile consumes event lifecycle through discovery, detail, participation, media, and event interaction flows.
- There is an RPC named `transition_event_status_v2` in dashboard audit output.
- There is an RPC named `publish_event_with_groups_and_snapshot_v2` in dashboard audit output.
- Some prior implementation mentions a live-to-published revert behavior with a limited window.

These facts are not accepted as canonical rules.

## Unknowns / Needs Verification

The following are Unknown / Needs verification:

- Exact transition graph.
- Exact transition rules for `draft`.
- Exact transition rules for `published`.
- Exact transition rules for `live`.
- Exact transition rules for `ended`.
- Exact transition rules for `archived`.
- Exact revert behavior and any revert window.
- Exact backend schema for lifecycle status.
- Exact trigger behavior.
- Exact RLS behavior.
- Exact RPC behavior for `transition_event_status_v2`.
- Exact RPC behavior for `publish_event_with_groups_and_snapshot_v2`.
- Exact feed visibility rules.
- Exact public share visibility rules.
- Exact dashboard management visibility and permission rules.
- Exact mobile lifecycle behavior.
- Cross-surface consistency between Mobile, Dashboard, Web/Public, and Supabase.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- Known statuses are confirmed.
- The transition graph is specified or explicitly deferred.
- Feed visibility rules are verified.
- Public share visibility rules are verified or explicitly deferred.
- Dashboard lifecycle management rules are verified.
- Mobile lifecycle consumption rules are verified.
- Backend authority for transitions is verified.
- Relevant schema, trigger, RLS, and RPC behavior is documented or explicitly deferred.
- Revert behavior is verified, rejected, or explicitly deferred.
- Cross-surface inconsistencies are recorded.
- Mustafa / JoinFolk approves the document for acceptance.

## Open Questions

- What is the accepted transition graph between `draft`, `published`, `live`, `ended`, and `archived`?
- What backend rules enforce lifecycle transitions?
- What are the exact feed visibility rules for each status?
- What are the exact public share visibility rules for each status?
- Is live-to-published revert behavior intended, and if so, what is the accepted window?
- Which surface should be audited first for lifecycle consistency?

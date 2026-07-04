# Notifications

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines the platform-level draft specification for JoinFolk notification behavior.

It records known notification concepts, authority boundaries, cross-surface requirements, and verification gaps.

This document is not canonical. No notification behavior is accepted by this document alone.

## Definitions

### Notification

A notification is a platform communication or notification-related record, signal, or delivery behavior.

Exact notification schema and behavior are Unknown / Needs verification.

### Notification Recipient

A notification recipient is the intended receiver of a notification.

Exact recipient rules are Unknown / Needs verification.

### Notification Type

A notification type classifies notification behavior.

Exact notification types are Unknown / Needs verification.

### Notification Delivery

Notification delivery is the process or mechanism by which a notification reaches a recipient.

Exact delivery behavior and delivery channels are Unknown / Needs verification.

### In-app Notification

An in-app notification is a notification displayed inside a JoinFolk platform surface.

Exact in-app notification behavior is Unknown / Needs verification.

### Read/Unread State

Read/unread state is a notification state indicating whether a recipient has read or not read a notification.

Exact read/unread behavior is Unknown / Needs verification.

## Known Notification Concepts

Known notification concepts include:

- notification behavior
- notification-related concepts
- notification recipient
- notification type
- notification delivery
- in-app notification
- read/unread state
- possible push, email, or in-app delivery channels

These concepts require verification before acceptance.

## Canonical Rules Draft

### What Is Known

JoinFolk has notification behavior or notification-related concepts.

Notifications are a platform domain.

Notifications may interact with event lifecycle, ticketing, reservations, wallet/ownership, media/gallery, staff scanner, ops/admin, and host identity transfer.

Reservation behavior may interact with notifications.

Wallet/ownership behavior may interact with notifications.

Notification behavior must be consistent across Mobile, Dashboard, Web/Public, and Supabase where applicable.

Backend, RPC, and RLS must enforce security-sensitive notification behavior.

Client-side notification display is UX only.

### What Is Not Yet Accepted

Exact notification schema is not accepted.

Exact notification types are not accepted.

Exact notification status machine is not accepted.

Exact recipient rules are not accepted.

Exact delivery behavior is not accepted.

Exact read/unread behavior is not accepted.

Exact RPC contracts are not accepted.

Exact RLS enforcement is not accepted.

Exact push, email, and in-app delivery channels are not accepted.

Exact notification behavior around tickets, reservations, transfers, media, staff scanner, and ops/admin is not accepted.

No rule in this document is canonical.

## Notification Authority Model

### What Frontend May Display

Frontend surfaces may display notification lists, notification labels, unread indicators, read state UI, delivery-related UI, and notification-related navigation based on available platform data.

Frontend surfaces may help users interact with notifications.

Client-side notification display is UX only.

### What Backend Must Enforce

Backend, RPC, and RLS must enforce security-sensitive notification behavior.

Backend enforcement is required for recipient authority, protected notification access, notification creation where applicable, notification mutation, read/unread mutation, delivery eligibility, and notification behavior tied to security-sensitive domains.

### What Must Never Be Frontend-Only

The following must never be frontend-only:

- notification recipient authority
- protected notification access
- notification creation authority
- notification mutation authority
- read/unread mutation authority
- delivery eligibility
- notification behavior tied to tickets, reservations, transfers, media, staff scanner, ops/admin, or host identity transfer

## Notification State / Status Draft

### Known Concepts

Known state-related concepts include notification type, notification delivery, in-app notification, and read/unread state.

Exact notification states are not accepted.

### Unknown States

The full notification status machine is Unknown / Needs verification.

No exact notification states are defined by this document.

### Required Verification

Verification must identify accepted notification states, allowed mutations, recipient rules, delivery behavior, backend authority, RPC contracts, RLS enforcement, and cross-surface display rules.

## Recipient and Delivery Draft

### Known Behavior

Notifications may have recipients.

Notifications may have delivery behavior.

Possible delivery channels include push, email, or in-app delivery, but exact accepted channels are Unknown / Needs verification.

### Unknowns

The following are Unknown / Needs verification:

- exact recipient rules
- exact delivery behavior
- exact delivery channels
- exact in-app behavior
- exact read/unread behavior
- exact backend authority
- exact RPC contracts
- exact RLS enforcement

### Dangerous Assumptions

Do not assume notification recipients can be calculated by frontend-only logic.

Do not assume a displayed notification is authorized without backend enforcement.

Do not assume push, email, or in-app delivery channels are accepted.

Do not assume read/unread behavior is only local client state.

Do not assume notification behavior around tickets, reservations, transfers, media, scanner, ops/admin, or host identity transfer is accepted.

## Relationship to Other Models

### Event lifecycle

Notifications may interact with event lifecycle.

Exact lifecycle-related notification behavior is Unknown / Needs verification.

### Ticketing

Notifications may interact with ticketing.

Exact ticket-related notification behavior is Unknown / Needs verification.

### Reservations

Reservation behavior may interact with notifications.

Exact reservation-related notification behavior is Unknown / Needs verification.

### Wallet/ownership

Wallet/ownership behavior may interact with notifications.

Exact ownership-related notification behavior is Unknown / Needs verification.

### Media/gallery

Notifications may interact with media/gallery.

Exact media/gallery-related notification behavior is Unknown / Needs verification.

### Staff scanner

Notifications may interact with staff scanner behavior.

Exact scanner-related notification behavior is Unknown / Needs verification.

### Ops/admin

Notifications may interact with ops/admin behavior.

Exact ops/admin notification behavior is Unknown / Needs verification.

### Host identity transfer

Notifications may interact with host identity transfer.

Exact host identity transfer notification behavior is Unknown / Needs verification.

## Cross-Surface Consistency Requirements

### Mobile

Mobile must use notification semantics consistent with the rest of the platform where applicable.

Mobile-only notification authority is not accepted.

### Dashboard

Dashboard must use notification semantics consistent with the rest of the platform where applicable.

Dashboard notification behavior must be backed by backend authority where security-sensitive.

### Web/Public

Web/public surfaces must use notification semantics consistent with the rest of the platform where applicable.

Public notification-related display must not bypass backend notification authority.

### Supabase Backend

Supabase database, RPC, RLS, auth, and storage behavior must enforce security-sensitive notification behavior where applicable.

Exact backend/RPC/RLS enforcement is Unknown / Needs verification.

## Security Risks

Frontend-only notification authority may expose private notifications or allow unauthorized notification mutation.

Incorrect recipient rules may send or display notifications to the wrong person.

Incorrect delivery behavior may leak sensitive event, ticket, reservation, ownership, scanner, ops/admin, or transfer information.

Incorrect read/unread authority may allow unauthorized state mutation.

Incorrect RLS/RPC enforcement may expose protected notification data.

## Determinism Risks

Mobile, Dashboard, Web/Public, and Supabase may interpret notification state differently.

Recipient rules may differ between creation, display, and delivery.

Read/unread behavior may diverge across surfaces.

Delivery behavior may diverge from backend notification state.

Notifications tied to tickets, reservations, transfers, media, staff scanner, ops/admin, or host identity transfer may drift from the source domain state.

## Current Known Implementation

Known implementation facts:

- JoinFolk has notification behavior or notification-related concepts.
- Notifications are a platform domain.
- Notifications may interact with event lifecycle, ticketing, reservations, wallet/ownership, media/gallery, staff scanner, ops/admin, and host identity transfer.
- Reservation behavior may interact with notifications.
- Wallet/ownership behavior may interact with notifications.

These facts are not accepted as canonical rules.

## Unknowns / Needs Verification

The following are Unknown / Needs verification:

- Exact notification schema.
- Exact notification types.
- Exact notification status machine.
- Exact recipient rules.
- Exact delivery behavior.
- Exact read/unread behavior.
- Exact RPC contracts.
- Exact RLS enforcement.
- Exact push, email, and in-app delivery channels.
- Exact notification behavior around tickets.
- Exact notification behavior around reservations.
- Exact notification behavior around transfers.
- Exact notification behavior around media/gallery.
- Exact notification behavior around staff scanner.
- Exact notification behavior around ops/admin.
- Exact notification behavior around host identity transfer.
- Cross-surface consistency across Mobile, Dashboard, Web/Public, and Supabase.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- Notification concepts are confirmed.
- Notification schema is documented or explicitly deferred.
- Notification types are specified or explicitly deferred.
- Notification status machine is specified or explicitly deferred.
- Recipient rules are verified or explicitly deferred.
- Delivery behavior and channels are verified or explicitly deferred.
- Read/unread behavior is verified or explicitly deferred.
- RPC contracts are documented or explicitly deferred.
- RLS enforcement is documented or explicitly deferred.
- Notification behavior around related domains is verified or explicitly deferred.
- Cross-surface consistency is audited.
- Open questions are resolved, explicitly deferred, or moved to tracked status documents.
- Mustafa / JoinFolk approves the document for acceptance.

## Open Questions

- What notification types are accepted?
- What is the accepted notification schema?
- What are the accepted recipient rules?
- What delivery channels are accepted?
- What is the accepted read/unread behavior?
- What backend rules enforce notification access and mutation?
- Which related domain should be audited first for notification behavior?

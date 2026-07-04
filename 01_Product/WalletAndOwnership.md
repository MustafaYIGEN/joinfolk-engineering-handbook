# Wallet And Ownership

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines the platform-level draft specification for JoinFolk wallet and ownership behavior.

It records known wallet and ownership concepts, authority boundaries, high-risk areas, and verification gaps.

This document is not canonical. No wallet or ownership behavior is accepted by this document alone.

## Definitions

### Wallet

A wallet is a known platform concept related to ownership.

Exact wallet schema and behavior are Unknown / Needs verification.

### Ownership

Ownership is the authority relationship between a user, wallet, ticket, or related asset.

Ownership behavior is security-sensitive.

### Ticket Ownership

Ticket ownership is the authority relationship between a ticket and an owner.

Ticket ownership is security-sensitive.

Exact ticket ownership schema and behavior are Unknown / Needs verification.

### Wallet Ownership

Wallet ownership is the authority relationship between a wallet and an owner.

Wallet ownership is security-sensitive.

Exact wallet ownership schema and behavior are Unknown / Needs verification.

### Ticket Holder

A ticket holder is a known viewer-role-related concept.

Ticket holder semantics exist but are not accepted yet.

### Participant

A participant is a known viewer-role-related concept.

Participant semantics exist but are not accepted yet.

### Gift

Gift behavior exists as a ticket-related concept.

Exact gift behavior is Unknown / Needs verification.

### Transfer

Transfer behavior exists as a ticket-related concept.

Exact transfer behavior is Unknown / Needs verification.

### Check-in Relationship

Check-in interacts with ticket ownership and viewer role behavior.

Staff scanner interacts with check-in.

Exact check-in and ownership relationship is Unknown / Needs verification.

## Known Wallet / Ownership Concepts

Known wallet and ownership concepts include:

- wallet ownership
- ticket ownership
- ticket holder semantics
- participant semantics
- gift behavior
- transfer behavior
- check-in relationship
- staff scanner relationship

These concepts require verification before acceptance.

## Canonical Rules Draft

### What Is Known

JoinFolk has wallet/ticket ownership concepts.

Ticket ownership is security-sensitive.

Wallet ownership is security-sensitive.

Ticketing involves ticket ownership or wallet ownership concepts.

Ticket holder and participant semantics exist but are not accepted yet.

Gift/transfer behavior exists as a ticket-related concept, but exact behavior is not accepted yet.

Host identity transfer exists and is separate from ticket/wallet ownership unless explicitly verified otherwise.

Check-in interacts with ticket ownership and viewer role behavior.

Staff scanner interacts with check-in.

Backend, RPC, and RLS must enforce security-sensitive ownership behavior.

Client-side ownership display is UX only.

Mobile, Dashboard, Web/Public, and Supabase must agree on wallet and ownership semantics.

Wallet/ownership behavior interacts with ticketing, viewer roles, event lifecycle, commerce model, staff scanner, notifications, and host identity transfer.

### What Is Not Yet Accepted

Exact wallet schema is not accepted.

Exact ticket ownership schema is not accepted.

Exact ownership status machine is not accepted.

Exact transfer/gift behavior is not accepted.

Exact ticket holder vs participant semantics are not accepted.

Exact RPC contracts are not accepted.

Exact RLS enforcement is not accepted.

Host identity transfer relationship to wallet/ticket ownership is not accepted beyond the known separation rule.

No rule in this document is canonical.

## Ownership Authority Model

### What Frontend May Display

Frontend surfaces may display wallet state, ticket ownership state, ticket holder labels, participant labels, gift or transfer UX, check-in-related ownership UX, and ownership warnings based on available platform data.

Frontend surfaces may guide users through wallet, ownership, gift, transfer, and check-in-related flows.

Client-side ownership display is UX only.

### What Backend Must Enforce

Backend, RPC, and RLS must enforce security-sensitive ownership behavior.

Backend enforcement is required for wallet ownership, ticket ownership, ticket holder authority, participant authority, gift behavior, transfer behavior, check-in-sensitive ownership effects, protected ownership access, and ownership mutations.

### What Must Never Be Frontend-Only

The following must never be frontend-only:

- wallet ownership authority
- ticket ownership authority
- ticket holder authority
- participant authority
- gift authority
- transfer authority
- ownership mutation authority
- protected ownership access
- check-in-sensitive ownership authority

## Ownership State / Status Draft

### Known Concepts

Known state-related concepts include wallet ownership, ticket ownership, ticket holder semantics, participant semantics, gift behavior, transfer behavior, and check-in relationship.

Exact ownership statuses are not accepted.

### Unknown States

The full ownership status machine is Unknown / Needs verification.

No exact ownership states are defined by this document.

### Required Verification

Verification must identify accepted ownership states, allowed transitions, backend authority, RPC contracts, RLS enforcement, and cross-surface display rules.

Verification must distinguish wallet ownership from ticket ownership where applicable.

## Gift / Transfer Draft

### Known Behavior

Gift/transfer behavior exists as a ticket-related concept.

Exact gift behavior is not accepted.

Exact transfer behavior is not accepted.

### Unknowns

The following are Unknown / Needs verification:

- whether gift and transfer are separate flows
- exact source and destination ownership rules
- exact approval requirements
- exact status changes
- exact wallet effects
- exact ticket ownership effects
- exact viewer role effects
- exact notification effects
- exact RPC contracts
- exact RLS enforcement

### Dangerous Assumptions

Do not assume gift or transfer can be handled by frontend-only logic.

Do not assume ticket holder and participant mean the same thing.

Do not assume wallet ownership and ticket ownership are identical.

Do not merge host identity transfer with wallet/ticket ownership unless explicitly verified.

Do not assume current implementation behavior is intended behavior.

## Relationship to Other Models

### Ticketing

Ticketing involves ticket ownership or wallet ownership concepts.

Exact ticketing ownership behavior is Unknown / Needs verification.

### Viewer roles

Ticket holder and participant semantics exist but are not accepted yet.

Ownership may affect viewer roles.

Exact viewer role effects are Unknown / Needs verification.

### Event lifecycle

Wallet/ownership behavior interacts with event lifecycle.

Exact lifecycle-dependent ownership behavior is Unknown / Needs verification.

### Commerce model

Wallet/ownership behavior interacts with commerce model.

Exact commerce-related ownership behavior is Unknown / Needs verification.

### Staff scanner

Staff scanner interacts with check-in.

Check-in interacts with ticket ownership and viewer role behavior.

Exact scanner ownership behavior is Unknown / Needs verification.

### Notifications

Wallet/ownership behavior interacts with notifications.

Exact notification behavior is Unknown / Needs verification.

### Host identity transfer

Host identity transfer exists and is separate from ticket/wallet ownership unless explicitly verified otherwise.

Any relationship between host identity transfer and wallet/ticket ownership is Unknown / Needs verification.

## Cross-Surface Consistency Requirements

### Mobile

Mobile must use wallet and ownership semantics consistent with the rest of the platform.

Mobile-only ownership authority is not accepted.

### Dashboard

Dashboard must use wallet and ownership semantics consistent with the rest of the platform.

Dashboard ownership actions must be backed by backend authority.

### Web/Public

Web/public surfaces must use wallet and ownership semantics consistent with the rest of the platform.

Public ownership display must not bypass backend ownership authority.

### Supabase Backend

Supabase database, RPC, RLS, auth, and storage behavior must enforce security-sensitive ownership behavior where applicable.

Exact backend/RPC/RLS enforcement is Unknown / Needs verification.

## Security Risks

Frontend-only ownership authority may allow unauthorized ownership display, mutation, gift, transfer, or access.

Incorrect ticket ownership behavior may assign or expose tickets incorrectly.

Incorrect wallet ownership behavior may expose or mutate wallet assets incorrectly.

Incorrect ticket holder or participant semantics may grant incorrect event access.

Incorrect check-in ownership behavior may allow unauthorized entry or duplicate entry.

Incorrect gift/transfer behavior may transfer ownership incorrectly.

Incorrect assumptions about host identity transfer may merge unrelated authority models.

## Determinism Risks

Mobile, Dashboard, Web/Public, and Supabase may interpret ownership state differently.

Wallet ownership and ticket ownership may be conflated.

Ticket holder and participant semantics may diverge.

Gift and transfer behavior may update ownership inconsistently.

Check-in behavior may diverge from ownership and viewer role behavior.

Host identity transfer may be incorrectly treated as wallet/ticket ownership behavior.

## Current Known Implementation

Known implementation facts:

- JoinFolk has wallet/ticket ownership concepts.
- Ticketing involves ticket ownership or wallet ownership concepts.
- Gift/transfer behavior exists as a ticket-related concept.
- Check-in interacts with ticket ownership and viewer role behavior.
- Staff scanner interacts with check-in.
- Host identity transfer exists and is separate from ticket/wallet ownership unless explicitly verified otherwise.

These facts are not accepted as canonical rules.

## Unknowns / Needs Verification

The following are Unknown / Needs verification:

- Exact wallet schema.
- Exact ticket ownership schema.
- Exact ownership status machine.
- Exact transfer behavior.
- Exact gift behavior.
- Exact ticket holder semantics.
- Exact participant semantics.
- Exact check-in relationship to ownership.
- Exact staff scanner relationship to ownership.
- Exact RPC contracts.
- Exact RLS enforcement.
- Exact relationship, if any, between host identity transfer and wallet/ticket ownership.
- Cross-surface consistency across Mobile, Dashboard, Web/Public, and Supabase.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- Wallet and ownership concepts are confirmed.
- Wallet schema is documented or explicitly deferred.
- Ticket ownership schema is documented or explicitly deferred.
- Ownership status machine is specified or explicitly deferred.
- Gift behavior is verified or explicitly deferred.
- Transfer behavior is verified or explicitly deferred.
- Ticket holder and participant semantics are verified.
- Check-in relationship to ownership is verified.
- Staff scanner relationship to ownership is verified.
- RPC contracts are documented or explicitly deferred.
- RLS enforcement is documented or explicitly deferred.
- Relationship to host identity transfer is verified or explicitly deferred.
- Cross-surface consistency is audited.
- Open questions are resolved, explicitly deferred, or moved to tracked status documents.
- Mustafa / JoinFolk approves the document for acceptance.

## Open Questions

- What is the accepted wallet schema?
- What is the accepted ticket ownership schema?
- What is the accepted ownership status machine?
- What is the accepted difference between ticket holder and participant?
- What are the accepted gift and transfer behaviors?
- What backend rules enforce wallet and ticket ownership?
- How does check-in affect ownership, if at all?
- Is there any verified relationship between host identity transfer and wallet/ticket ownership?

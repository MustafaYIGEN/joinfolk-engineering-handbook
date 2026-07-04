# Ticketing

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines the platform-level draft specification for JoinFolk ticketing behavior.

It records known ticketing concepts, authority boundaries, high-risk areas, and verification gaps.

This document is not canonical. No ticketing behavior is accepted by this document alone.

## Definitions

### Ticketing

Ticketing is JoinFolk event behavior related to the commerce mode `ticketSales`.

### Ticket Product

A ticket product is a ticket-related product associated with an event.

Exact schema and product behavior are Unknown / Needs verification.

### Ticket Claim

A ticket claim is a known ticket-related concept.

Exact claim behavior is Unknown / Needs verification.

### Ticket Request

A ticket request is a known ticket-related concept.

Exact request behavior is Unknown / Needs verification.

### Ticket Ownership

Ticket ownership is the authority relationship between a ticket and an owner.

Ticket ownership is security-sensitive.

Exact ownership behavior is Unknown / Needs verification.

### Ticket Holder

A ticket holder is a known viewer-role-related concept.

Ticket holder semantics exist but are not accepted yet.

### Participant

A participant is a known viewer-role-related concept.

Participant semantics exist but are not accepted yet.

### Check-in

Check-in is a known ticket-related concept.

Check-in is security-sensitive and must be backend-enforced.

### Reserved Seating

Reserved seating is a known ticketing concept.

Reserved seating is high-risk.

Exact reserved seating behavior is Unknown / Needs verification.

### Product-section Mapping

Product-section mapping is a known ticketing concept.

Product-section mapping is high-risk.

Exact product-section mapping behavior is Unknown / Needs verification.

## Known Ticketing Concepts

Known ticket-related concepts include:

- ticket products
- ticket claims
- ticket requests
- purchase/request flows
- approval
- rejection
- revocation
- check-in
- capacity
- reserved seating
- product-section mapping
- gift/transfer behavior

These concepts require verification before acceptance.

## Canonical Rules Draft

### What Is Known

JoinFolk has ticketing behavior.

Ticketing is related to the commerce mode `ticketSales`.

Ticketing involves ticket products.

Ticketing involves ticket ownership or wallet ownership concepts.

Ticket holder and participant semantics exist but are not accepted yet.

Ticket ownership and wallet ownership are security-sensitive.

Check-in is security-sensitive and must be backend-enforced.

Staff scanner behavior interacts with ticket check-in.

Reserved seating and product-section mapping are high-risk.

Backend, RPC, and RLS must enforce security-sensitive ticketing behavior.

Client-side ticket state is UX only.

Mobile, Dashboard, Web/Public, and Supabase must agree on ticketing semantics.

### What Is Not Yet Accepted

Exact ticket schema is not accepted.

Exact ticket status machine is not accepted.

Exact ticket RPC contracts are not accepted.

Exact capacity enforcement behavior is not accepted.

Exact reserved seating behavior is not accepted.

Exact product-section mapping behavior is not accepted.

Exact gift/transfer behavior is not accepted.

Exact ticket holder vs participant semantics are not accepted.

Exact wallet ownership behavior is not accepted.

Exact RLS/RPC behavior is not accepted.

No rule in this document is canonical.

## Ticketing Authority Model

### What Frontend May Display

Frontend surfaces may display ticket products, ticket state, ticket request state, purchase/request flows, approval or rejection labels, revocation labels, check-in labels, capacity indicators, seating information, and wallet/ticket ownership UX.

Frontend surfaces may guide users through ticket-related flows.

Client-side ticket state is UX only.

### What Backend Must Enforce

Backend, RPC, and RLS must enforce security-sensitive ticketing behavior.

Backend enforcement is required for ticket ownership, wallet ownership effects, ticket purchase/request authority, approval, rejection, revocation, check-in, capacity enforcement, reserved seating, product-section mapping, and gift/transfer behavior where applicable.

### What Must Never Be Frontend-Only

The following must never be frontend-only:

- ticket ownership authority
- wallet ownership authority
- ticket holder authority
- participant authority
- purchase/request authority
- approval authority
- rejection authority
- revocation authority
- check-in authority
- capacity enforcement
- reserved seating authority
- product-section mapping authority
- gift/transfer authority

## Ticket Status / State Machine Draft

### Known States/Concepts

Known state-related concepts include purchase/request flows, approval, rejection, revocation, and check-in.

Exact ticket statuses are not accepted.

### Unknown States

The full ticket status machine is Unknown / Needs verification.

No exact ticket statuses are defined by this document.

### Required Verification

Verification must identify the accepted ticket states, allowed transitions, backend authority, RLS/RPC enforcement, and cross-surface display rules.

## Capacity and Reserved Seating Draft

### Known Behavior

Capacity is a known ticketing concept.

Reserved seating is a known ticketing concept.

Product-section mapping is a known ticketing concept.

### High-risk Areas

Capacity enforcement is high-risk because incorrect behavior can oversell, block valid users, or corrupt event availability.

Reserved seating is high-risk because incorrect behavior can assign, expose, or duplicate seats incorrectly.

Product-section mapping is high-risk because incorrect behavior can connect ticket products to the wrong event sections.

### Unknowns

Exact capacity enforcement behavior is Unknown / Needs verification.

Exact reserved seating behavior is Unknown / Needs verification.

Exact product-section mapping behavior is Unknown / Needs verification.

## Wallet / Ownership Relationship

Ticketing involves ticket ownership or wallet ownership concepts.

Ticket ownership and wallet ownership are security-sensitive.

Exact wallet ownership behavior is Unknown / Needs verification.

Ticketing changes that affect wallet or ownership authority must be treated as high-risk.

## Staff Scanner / Check-in Relationship

Staff scanner behavior interacts with ticket check-in.

Check-in is security-sensitive and must be backend-enforced.

Exact staff scanner check-in behavior is Unknown / Needs verification.

## Relationship to Other Models

### Commerce model

Ticketing is related to the commerce mode `ticketSales`.

Ticketing must depend on the shared commerce model.

### Viewer roles

Ticket holder and participant semantics exist but are not accepted yet.

Ticketing may affect viewer roles.

Exact viewer role effects are Unknown / Needs verification.

### Event lifecycle

Ticketing may interact with event lifecycle.

Exact lifecycle-dependent ticketing behavior is Unknown / Needs verification.

### Reservations

Ticketing and reservations are separate commerce-related concepts.

Exact interaction between ticketing and reservations is Unknown / Needs verification.

### Wallet/ownership

Ticketing involves ticket ownership or wallet ownership concepts.

Exact ownership behavior is Unknown / Needs verification.

### Staff scanner

Staff scanner behavior interacts with ticket check-in.

Exact scanner authority and check-in behavior are Unknown / Needs verification.

## Cross-Surface Consistency Requirements

### Mobile

Mobile must use ticketing semantics consistent with the rest of the platform.

Mobile-only ticketing authority is not accepted.

### Dashboard

Dashboard must use ticketing semantics consistent with the rest of the platform.

Dashboard ticketing actions must be backed by backend authority.

### Web/Public

Web/public surfaces must use ticketing semantics consistent with the rest of the platform.

Public ticketing display must not bypass backend ticketing authority.

### Supabase Backend

Supabase database, RPC, RLS, auth, and storage behavior must enforce security-sensitive ticketing behavior where applicable.

Exact backend/RPC/RLS enforcement is Unknown / Needs verification.

## Security Risks

Frontend-only ticketing authority may allow unauthorized purchases, requests, approvals, revocations, check-ins, transfers, or ownership changes.

Incorrect ticket ownership or wallet ownership behavior may transfer or expose assets incorrectly.

Incorrect check-in behavior may allow unauthorized event entry or duplicate entry.

Incorrect capacity enforcement may oversell or block legitimate access.

Incorrect reserved seating or product-section mapping may assign incorrect inventory or event sections.

Incorrect staff scanner enforcement may allow unauthorized scanning.

## Determinism Risks

Mobile, Dashboard, Web/Public, and Supabase may interpret ticketing state differently.

Ticket holder and participant semantics may diverge.

Capacity may be calculated inconsistently.

Reserved seating may be assigned or displayed inconsistently.

Product-section mapping may differ between dashboard management, backend enforcement, and public display.

Gift/transfer behavior may change ownership inconsistently if not backend-enforced.

## Current Known Implementation

Known implementation facts:

- JoinFolk has ticketing behavior.
- Ticketing is related to the commerce mode `ticketSales`.
- Ticketing involves ticket products.
- Ticketing involves ticket ownership or wallet ownership concepts.
- Ticket-related concepts include ticket claims, ticket requests, purchase/request flows, approval, rejection, revocation, check-in, capacity, reserved seating, product-section mapping, and gift/transfer behavior.
- Staff scanner behavior interacts with ticket check-in.

These facts are not accepted as canonical rules.

## Unknowns / Needs Verification

The following are Unknown / Needs verification:

- Exact ticket schema.
- Exact ticket status machine.
- Exact ticket RPC contracts.
- Exact capacity enforcement behavior.
- Exact reserved seating behavior.
- Exact product-section mapping behavior.
- Exact gift/transfer behavior.
- Exact ticket ownership behavior.
- Exact wallet ownership behavior.
- Exact ticket holder semantics.
- Exact participant semantics.
- Exact staff scanner check-in behavior.
- Exact backend/RPC/RLS enforcement.
- Cross-surface consistency across Mobile, Dashboard, Web/Public, and Supabase.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- Ticketing concepts are confirmed.
- Ticket schema is documented or explicitly deferred.
- Ticket status machine is specified or explicitly deferred.
- Ticket RPC contracts are documented or explicitly deferred.
- Capacity enforcement behavior is verified or explicitly deferred.
- Reserved seating behavior is verified or explicitly deferred.
- Product-section mapping behavior is verified or explicitly deferred.
- Gift/transfer behavior is verified or explicitly deferred.
- Ticket holder and participant semantics are verified.
- Wallet/ownership relationship is verified.
- Staff scanner check-in behavior is verified.
- Backend/RPC/RLS enforcement is documented or explicitly deferred.
- Cross-surface consistency is audited.
- Open questions are resolved, explicitly deferred, or moved to tracked status documents.
- Mustafa / JoinFolk approves the document for acceptance.

## Open Questions

- What is the accepted ticket status machine?
- What are the accepted ticket RPC contracts?
- What is the accepted difference between ticket holder and participant?
- What backend rules enforce ticket and wallet ownership?
- What backend rules enforce check-in?
- What are the accepted capacity and reserved seating rules?
- What is the accepted gift/transfer behavior?
- Which surface should be audited first for ticketing consistency?

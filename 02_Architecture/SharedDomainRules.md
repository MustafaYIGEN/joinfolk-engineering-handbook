# Shared Domain Rules

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines the platform-level draft specification for shared business and domain rules in JoinFolk.

It records which rules must be consistent across implementation surfaces, where authority must live, and which areas require verification.

This document is not canonical. No shared domain rule architecture is accepted by this document alone.

## Shared Domain Rule Definition

A shared domain rule is a business or platform rule that must produce the same meaning across Mobile, Dashboard, Web/Public, and Supabase backend surfaces.

Shared domain rules may be expressed in frontend display logic, shared helper or resolver logic, backend RPC logic, database policies, or other backend enforcement.

Security-sensitive shared rules must be enforced by backend, RPC, and RLS.

Exact shared package strategy and exact domain-rule ownership model are Unknown / Needs verification.

## Domains Requiring Shared Rules

Product domains requiring shared rules include:

- personas and tiers
- event lifecycle
- viewer roles
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

Each domain requires verification before its shared rule model can be accepted.

## Rule Authority Model

### Frontend Display Rules

Frontend display rules may determine labels, disabled states, warnings, local validation, navigation, and other user experience behavior.

Client-side guards and client-side resolvers are UX helpers only.

Frontend display rules must not become the only authority for security-sensitive behavior.

### Backend Enforcement Rules

Backend, RPC, and RLS must enforce security-sensitive rules.

Backend enforcement is required for protected access, authorization, ownership, lifecycle transitions, commerce authority, ticketing authority, reservation authority, wallet ownership, staff scanner authorization, ops/admin authority, host identity transfer, and protected public/private visibility.

Exact backend schema, RPC, and RLS behavior are Unknown / Needs verification.

### Shared Pure Resolver/Helper Rules

Shared pure resolver or helper rules may be used to keep non-authoritative calculations consistent across surfaces.

Exact resolver algorithms are Unknown / Needs verification.

Shared resolver output must not replace backend enforcement for security-sensitive operations.

## What Must Be Shared Across Surfaces

The meaning of platform domains must be shared across Mobile, Dashboard, Web/Public, and Supabase.

Shared meanings include:

- tier and persona semantics
- lifecycle status semantics
- viewer role semantics
- commerce mode semantics
- ticketing semantics
- reservation semantics
- wallet and ownership semantics
- notification semantics where applicable
- venue/business tool semantics
- media/gallery authority semantics
- staff scanner semantics
- ops/admin semantics
- host identity transfer semantics
- public sharing and visibility semantics

Exact implementation strategy is Unknown / Needs verification.

## What May Be Surface-Specific

Surfaces may differ in presentation, navigation, layout, copy, local interaction state, loading states, and UX affordances.

Surfaces may expose different workflows when the underlying business rule and backend authority are consistent.

Surface-specific behavior must not redefine a shared domain rule.

## What Must Never Diverge

The following must never diverge across surfaces:

- security-sensitive authorization meaning
- ownership meaning
- event lifecycle meaning
- viewer role meaning
- commerce mode meaning
- ticket holder and participant meaning after acceptance
- reservation authority meaning
- staff scanner authority meaning
- ops/admin authority meaning
- host identity transfer authority meaning
- protected public/private visibility meaning

If divergence is observed, it must be recorded as a platform inconsistency.

## Frontend Resolver Rules Draft

Frontend resolvers may support display and UX decisions.

Frontend resolvers must be treated as non-authoritative when they affect security-sensitive operations.

Frontend resolver behavior must be consistent with accepted specs and backend authority.

Exact frontend resolver algorithms are Unknown / Needs verification.

## Backend Enforcement Rules Draft

Backend, RPC, and RLS must enforce security-sensitive shared rules.

Backend enforcement must be verified before a security-sensitive rule is accepted.

Exact backend schema, RPC contracts, and RLS policies are Unknown / Needs verification.

No backend enforcement behavior is accepted by this document alone.

## Direct Supabase Call Policy Draft

Direct Supabase calls in pages or components are not automatically wrong.

They are an audit risk because they may duplicate domain logic, bypass shared helpers, obscure authority boundaries, or make cross-surface consistency harder to verify.

Direct Supabase calls require audit to classify whether they are acceptable, need wrapping, need refactoring, or require backend enforcement review.

This document does not require all direct Supabase calls to be removed.

## Cross-Surface Consistency Requirements

### Mobile

Mobile must use shared business rules consistently with Dashboard, Web/Public, and Supabase.

Mobile client-side guards and resolvers are UX helpers only.

### Dashboard

Dashboard must use shared business rules consistently with Mobile, Web/Public, and Supabase.

Dashboard client-side guards and resolvers are UX helpers only.

### Web/Public

Web/Public surfaces must use shared business rules consistently with Mobile, Dashboard, and Supabase.

Public visibility must not bypass backend authority.

### Supabase Backend

Supabase backend behavior must enforce security-sensitive shared rules through database, RPC, RLS, auth, and storage responsibilities where applicable.

Exact Supabase enforcement behavior is Unknown / Needs verification.

## Determinism Risks

Mobile, Dashboard, Web/Public, and Supabase may implement the same rule differently.

Frontend guards may drift from backend enforcement.

Client-side resolvers may be treated as security authority.

Direct Supabase calls may duplicate or bypass shared rule interpretation.

Broad service layers may hide inconsistent domain ownership.

Unaccepted backend schema, RPC, or RLS behavior may conflict with frontend assumptions.

## Simplification Principles

Shared rules should have one accepted meaning.

Security-sensitive authority should be enforced in backend, RPC, and RLS.

Frontend logic should present and guide, not replace backend authority.

Direct data access should be auditable and clearly scoped.

Shared helpers should reduce duplicate interpretation without claiming security authority.

Changes should be scoped, verifiable, and tied to the handbook workflow.

## Current Known Implementation

Known implementation facts:

- JoinFolk has Mobile, Dashboard, Web/Public, and Supabase backend surfaces.
- These surfaces must share the same business rules.
- Backend, RPC, and RLS must enforce security-sensitive rules.
- Client-side guards and client-side resolvers are UX helpers only.
- Direct Supabase calls in pages/components are not automatically wrong, but they are an audit risk.
- Current dashboard audit found a broad service layer.
- Current dashboard audit found direct Supabase calls in some pages/components.

These facts are not accepted as canonical architecture rules.

## Unknowns / Needs Verification

The following are Unknown / Needs verification:

- Exact shared package strategy.
- Exact domain-rule ownership model.
- Exact backend schema behavior.
- Exact RPC behavior.
- Exact RLS behavior.
- Exact cross-surface consistency status.
- Exact resolver algorithms.
- Which direct Supabase calls are acceptable.
- Which direct Supabase calls duplicate or bypass shared rules.
- Which client-side guards are backed by backend enforcement.
- Which domains currently diverge across surfaces.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- Domains requiring shared rules are confirmed.
- Shared rule ownership model is documented or explicitly deferred.
- Shared package strategy is documented or explicitly deferred.
- Frontend resolver expectations are documented or explicitly deferred.
- Backend schema/RPC/RLS enforcement is verified or explicitly deferred.
- Direct Supabase call policy is reviewed against implementation evidence.
- Cross-surface consistency is audited.
- Platform inconsistencies are recorded.
- Open questions are resolved, explicitly deferred, or moved to tracked status documents.
- Mustafa / JoinFolk approves the document for acceptance.

## Open Questions

- What is the accepted shared package or shared helper strategy?
- Which surface currently owns each domain rule?
- Which domain rules are duplicated across surfaces?
- Which direct Supabase calls are acceptable as-is?
- Which security-sensitive rules lack backend enforcement verification?
- Which product domain should be audited first for cross-surface consistency?

# Personas And Tiers

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines the platform-level draft specification for JoinFolk account tiers and personas.

It records known facts and unresolved questions for future specification, audit, patch planning, verification, and acceptance.

This document is not canonical. No tier or persona behavior is accepted by this document alone.

## Definitions

### Account Tier

An account tier is a classification of a JoinFolk account.

Known account tiers include `user`, `semi_pro`, `pro`, and ops/admin access through `is_ops` or an equivalent privileged flag.

Exact tier permissions are not accepted in this document.

### Persona

A persona is a user-facing identity mode used by the platform.

JoinFolk has at least two user-facing personas: personal persona and host persona.

### Personal Identity

Personal identity is the normal personal identity for a user.

Known personal identity fields include `display_name`, `avatar_url`, `bio`, or personal avatar/profile equivalents.

Exact schema and field ownership are not yet accepted.

### Host/Organizer Identity

Host or organizer identity is the identity used for organizer-facing or host-facing behavior.

Known organizer identity fields include `organizer_display_name`, `organizer_avatar_url`, and `organizer_bio`.

Exact schema and field ownership are not yet accepted.

### Ops/Admin Privilege

Ops/admin privilege is privileged access represented by `is_ops` or an equivalent privileged flag.

Exact permissions and enforcement rules are not yet accepted.

## Known Account Tiers

### user

`user` is a known account tier.

Permissions for this tier are Unknown / Needs verification.

### semi_pro

`semi_pro` is a known account tier.

Permissions for this tier are Unknown / Needs verification.

### pro

`pro` is a known account tier.

Permissions for this tier are Unknown / Needs verification.

### ops/admin

Ops/admin access exists through `is_ops` or an equivalent privileged flag.

Exact privilege boundaries are Unknown / Needs verification.

## Known Personas

### personal

The personal persona is used for normal personal identity.

### host

The host persona is used for organizer/host-facing identity.

Dashboard host events are expected to be host-persona events.

Some dashboard code filters events by `created_under_persona = "host"`.

## Canonical Rules Draft

### What Is Known

JoinFolk has account tiers: `user`, `semi_pro`, `pro`, and ops/admin via `is_ops` or an equivalent privileged flag.

JoinFolk has at least two user-facing personas: personal and host.

Host persona is used for organizer/host-facing identity.

Personal persona is used for normal personal identity.

Mobile, Dashboard, Web, and Supabase must agree on the same tier/persona semantics.

### What Is Not Yet Accepted

Exact tier permissions are not accepted.

Exact persona transition rules are not accepted.

Exact database schema is not accepted.

Exact RLS enforcement is not accepted.

Exact host identity transfer behavior is not accepted.

No rule in this document is canonical.

## Authority Model

### What Frontend May Display

Frontend surfaces may display tier labels, persona labels, profile fields, organizer fields, and UX guards based on available platform data.

Frontend surfaces may help users navigate between personal and host-facing flows.

Frontend behavior must not be treated as security authority.

### What Backend Must Enforce

Backend, RPC, and RLS must enforce security-sensitive tier and persona permissions.

Backend enforcement is required for privileged access, protected data access, ownership-sensitive operations, and host identity transfer.

### What Must Never Be Frontend-Only

The following must never be frontend-only:

- tier permission authority
- persona permission authority
- ops/admin privilege authority
- host identity transfer authority
- ticket ownership authority
- wallet ownership authority
- protected event access authority
- security-sensitive profile or organizer identity mutation authority

## Cross-Surface Consistency Requirements

### Mobile

Mobile must use the same tier and persona semantics as the rest of the platform.

Mobile-only interpretation of tier or persona behavior is not accepted.

### Dashboard

Dashboard must use the same tier and persona semantics as the rest of the platform.

Dashboard host events are expected to be host-persona events.

### Web/Public

Web/public surfaces must use the same tier and persona semantics as the rest of the platform.

Public display of personal or organizer identity requires verification of intended authority and exposure rules.

### Supabase Backend

Supabase database, RPC, auth, storage, and RLS must enforce security-sensitive tier and persona permissions.

Exact backend schema and RLS enforcement are Unknown / Needs verification.

## Host Identity Transfer Rules Draft

### Known Behavior

Host identity transfer exists and is high-risk.

Transfer may involve copying organizer identity and preserving personal identity.

Exact transfer behavior is not accepted.

### Dangerous Assumptions

Do not assume transfer can be handled by frontend-only logic.

Do not assume organizer identity and personal identity can be merged, overwritten, copied, or preserved without backend verification.

Do not assume current implementation behavior is intended behavior.

Do not assume tier permissions imply transfer authority.

### Required Backend Verification

Host identity transfer requires backend verification before any behavior is accepted or changed.

Verification must include schema, RPC, RLS, ownership, and data mutation review as applicable.

Manual verification is required before accepting transfer behavior.

## Security Risks

Tier and persona errors may grant unauthorized access.

Ops/admin privilege errors may expose privileged operations.

Host identity transfer errors may copy, overwrite, or expose identity data incorrectly.

Frontend-only guards may create false security assumptions.

Inconsistent Mobile, Dashboard, Web, and Supabase semantics may create authorization gaps.

## Determinism Risks

Different surfaces may interpret tier or persona differently.

Dashboard host events may diverge from backend persona semantics.

Identity fields may be copied or displayed inconsistently.

Unaccepted schema assumptions may cause future patches to encode incorrect behavior.

## Current Known Implementation

Known implementation facts:

- Dashboard host events are expected to be host-persona events.
- Some dashboard code filters events by `created_under_persona = "host"`.
- Organizer identity fields include `organizer_display_name`, `organizer_avatar_url`, and `organizer_bio`.
- Personal identity fields include `display_name`, `avatar_url`, `bio`, or personal avatar/profile equivalents.
- Host identity transfer exists.

These facts are not accepted as canonical rules.

## Unknowns / Needs Verification

The following are Unknown / Needs verification:

- Exact permissions for `user`.
- Exact permissions for `semi_pro`.
- Exact permissions for `pro`.
- Exact permissions for ops/admin access.
- Exact database schema for tiers and personas.
- Exact RLS rules for tiers and personas.
- Exact RPC behavior for persona-sensitive operations.
- Exact host identity transfer behavior.
- Whether organizer identity fields and personal identity fields are stored separately in all required paths.
- Whether Mobile, Dashboard, Web, and Supabase currently agree on tier/persona semantics.

## Acceptance Criteria for v1.0

This document may become `Version: 1.0` only after:

- Tier names are confirmed.
- Persona names are confirmed.
- Tier permissions are specified or explicitly deferred.
- Persona authority rules are specified.
- Backend enforcement expectations are verified.
- Exact schema and RLS behavior are documented or explicitly deferred.
- Host identity transfer behavior is verified and specified.
- Cross-surface consistency requirements are audited.
- Open questions are resolved, explicitly deferred, or moved to tracked status documents.
- Mustafa / JoinFolk approves the document for acceptance.

## Open Questions

- What permissions belong to `user`, `semi_pro`, and `pro`?
- What exact flag or role represents ops/admin authority?
- What backend rules enforce tier and persona permissions?
- What is the exact accepted behavior for host identity transfer?
- Which identity fields are canonical for personal identity and organizer identity?
- Which surface should be audited first for tier/persona consistency?

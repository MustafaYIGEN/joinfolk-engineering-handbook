# Do Not Touch Policy

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary
- canonical: false

## Purpose

This document defines areas that must not be changed without explicit approval.

## Explicit Approval Required

The following areas cannot be changed without explicit approval:

- RLS policies
- SECURITY DEFINER RPCs
- migrations
- storage policies
- event lifecycle transitions
- ticket ownership
- wallet ownership
- host identity transfer
- payment/commerce authority
- staff scanner authorization
- venue layout split/merge/product mapping

## Why These Areas Are Dangerous

These areas control access, authority, ownership, money flow, persistent data, operational trust, or irreversible platform state.

Incorrect changes can expose private data, transfer ownership incorrectly, break event operations, corrupt production records, weaken authorization, create payment disputes, or make future migrations unsafe.

The danger is not limited to code changes. Documentation, configuration, database, policy, and migration changes can also alter platform authority.

## Required Evidence Before Touching

Before touching a do-not-touch area, the patch plan must include:

- The explicit approval record.
- Relevant Accepted specs, or a clear statement that no Accepted spec exists.
- Current observed behavior.
- The intended behavior.
- Risk assessment.
- Verification plan.
- Rollback or forward-fix plan.
- Security and data ownership impact.
- Any platform inconsistencies discovered during review.

For database and authorization areas, evidence must include backend, schema, policy, or RPC review as appropriate.

For commerce, ticketing, wallet, scanner, and ownership areas, evidence must include manual verification of the affected authority path.

## Classification Rule

If unsure, classify the area as do-not-touch.

Uncertainty must stop patching until the scope is clarified and approval is obtained.

Agents must not downgrade a do-not-touch classification based on convenience, assumptions, or incomplete code observation.

## Open questions

- Who can grant explicit approval for each do-not-touch area?
- What format should explicit approval take?
- Which existing platform areas should be added to this policy after audit?

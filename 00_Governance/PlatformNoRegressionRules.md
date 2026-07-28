# Platform No-Regression Rules

## 1. Metadata

- Status: Active governance rule
- Version: 1.0
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-28
- Source confidence: accepted canonical platform scenario decisions
- canonical: true
- Implementation status: Applies to future patch review
- Production mutation status: This document does not authorize production mutation

## 2. Purpose

This document defines no-regression rules for scenario-led JoinFolk engineering work.

These rules prevent future patches from reintroducing the same classes of ambiguity found during ACL, RPC, RLS, schema, frontend, and product-flow alignment.

## 3. Binding Rules

1. No ad-hoc grant patch may proceed without scenario, callsite, and catalog evidence.
2. UI code must not mask denied, network, invalid-contract, offline, or degraded outcomes as empty states.
3. Frontend code must not query nonexistent database columns.
4. New events must not be created with null persona.
5. Mutation containment must not be mixed with product-surface repair.
6. PUBLIC or anon access must not be widened unless a public product surface is explicitly justified and reviewed.
7. Legacy commerce paths must not be revived as app-facing V1 purchase paths.
8. Frontend route access or persona state must not be treated as server authorization.
9. Successful smoke on one surface must not be used as proof for unrelated RPC, RLS, schema, or body behavior.
10. B03 remains untouched unless a dedicated prompt authorizes it.

## 4. Empty, Denied, Invalid, And Degraded States

Empty means a request succeeded and returned zero rows or an equivalent successful empty result.

Denied means the authenticated or anonymous actor does not have access.

Invalid contract means the client requested a field, payload shape, route, RPC signature, or state transition that is not part of the accepted contract.

Degraded means a noncritical dependency failed but the core surface can still render safely.

Offline or network failure must remain distinct from empty and denied.

Mutation failure must fail closed and must not imply success.

## 5. Review Consequence

A patch that violates these rules is blocked until it is narrowed, split, or backed by a new owner-reviewed decision.


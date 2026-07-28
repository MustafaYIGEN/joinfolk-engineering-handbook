# Canonical Platform Scenarios

## 1. Metadata

- Status: Accepted for JoinFolk V1 scenario alignment
- Version: 1.0
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-28
- Source confidence: owner-accepted canonical platform scenario packet
- canonical: true
- Implementation status: Partially implemented through closed gates; remaining waves tracked separately
- Production mutation status: This document does not authorize production mutation

## 2. Purpose

This document is the product-level scenario ledger for V1 JoinFolk behavior.

It summarizes the accepted cross-surface rules that future implementation, patch plans, audits, and release gates must follow.

## 3. Binding Decisions

The binding decision record is `09_Decisions/CanonicalPlatformScenarioDecision.md`.

The accepted V1 rules are:

1. `create_commerce_order_v1` is the only authenticated V1 purchase boundary.
2. Reservations are separate from tickets and never create automatic check-in entitlement.
3. Personal events use friends/groups; host events use public/members; audience is never silently reset.
4. Legacy null-persona events resolve as personal without automatic database backfill.
5. Event lifecycle is monotonic for V1.
6. The existing versioned visual-editor/RPC document is draft authority; immutable snapshot is published authority.
7. Owner, editor, operator, and staff permissions are distinct and server-enforced.
8. Buyer selection requires a published, sellable, product-mapped target constrained by published section capacity.
9. Friends, follows, groups, members, clubs, and blocking have distinct semantics; blocking overrides all.
10. Empty, denied, invalid, offline, and degraded outcomes are distinct.

## 4. Scenario Domains

| Domain | Canonical behavior |
|---|---|
| Buyer / participant journey | Buyer acquisition uses the canonical commerce order boundary. Buyer selection uses published sellable product-mapped targets only. |
| Host event creation and publish | Persona and audience are validated explicitly. Publish must not silently rewrite user intent. |
| Host dashboard commerce / tickets / reservations | Ticket products, queues, stats, and reservations use authenticated host-scoped RPCs. Reservations remain distinct from tickets. |
| Venue layout and product mapping | Draft mapping uses the existing visual-editor/RPC authority. Published buyer behavior uses immutable snapshot authority. |
| Social graph / profile / friends / followers | Friends are mutual, follows are discovery-only, active groups/members gate private visibility, and blocks override all relationships. |
| Staff scanner / check-in | Scanner and check-in authority requires event assignment and narrow staff/operator capability. Reservation alone is not check-in entitlement. |
| Notifications and reminders | Server notifications and local reminders remain separate contracts. Failures must not be rendered as empty success. |
| Auth / password recovery / routing | Auth email flows use the canonical web-only route behavior recorded by closed source-of-truth commit. |
| Persona switching | Switching persona changes viewer mode only. It must not rewrite event origin, ownership, audience, ticket, reservation, or staff state. |
| Event lifecycle | Normal V1 path is `draft -> published -> live -> ended -> archived`; live reversal, reopen, and unarchive are unsupported. |

## 5. Required Outcome States

Every product surface must distinguish:

- ready
- successful empty
- denied
- invalid contract
- degraded
- offline or network failure

Optional panel failure may degrade that panel while preserving core rendering. Mutation failure must fail closed.

## 6. Implementation Boundary

This document is binding product guidance. It does not authorize code, database, migration, ACL, RLS, schema, or production changes.

Implementation must proceed through the patch wave authorization model in `08_PatchPlans/PlatformScenarioPatchWaveAuthorization.md`.


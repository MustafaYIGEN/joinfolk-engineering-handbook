# Priority RPC Decision Register

## 1. Metadata

- Status: Draft
- Version: 0.3
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-14
- Source confidence: Production ACL export + confirmed runtime evidence through `08f` + confirmed family evidence through `09` + confirmed priority contracts through `10g` + read-only local static caller inspection
- canonical: false
- Implementation status: Not authorized

## 2. Purpose

This register captures the binding P0 and P1 RPC decisions that remain open for launch readiness.

Implementation remains not authorized.

## 3. Decision Register

| Priority | Exact production contract | Why it is risky | Evidence still missing | Static caller search required | Function-body checks required | Runtime state | Patch status | Rollback requirement |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| P0 | `public.mark_order_paid_v1(uuid)` live, SECURITY DEFINER, PUBLIC/anon/authenticated/service_role EXECUTE | Revenue mutation remains exposed far beyond a proven payment authority boundary | exact live orchestration authority; canonical raw `10b` export; precise replacement path decision | confirm every caller of `mark_order_paid_v1` and any replacement payment flow | prove whether body is still the local ticket-order dev stub and whether any provider verification exists | `NOT_OBSERVED_IN_08F` | BLOCKED | full ACL rollback and caller compatibility rollback required |
| P0 | `public.confirm_order_payment_v1(uuid, text, text, jsonb)` live, SECURITY DEFINER, service_role only | Payment confirmation is backend-authoritative and cannot drift silently | canonical raw `10b` export; final caller inventory | search backend, Edge, cron, and operator callers | verify explicit service-only guard, provider verification, and issuance handoff | `OBSERVED_IN_08F` with 8 calls | READY_TO_REVIEW_NOT_PATCH | preserve service-role-only posture; any change must be reversible |
| P0 | `public._issue_tickets_from_order_v1(uuid)` live, SECURITY DEFINER, service_role only | Ticket issuance helper is authority-critical and should not be exposed | canonical raw `10b` export; exact inbound caller graph | map every inbound caller, especially order and payment functions | verify no direct client authorization path exists | runtime not directly attributable in `08f` | READY_TO_REVIEW_NOT_PATCH | preserve service-role or internal-only posture |
| P0 | `public.check_in_ticket(text, uuid)` live, SECURITY DEFINER, anon/authenticated/service_role EXECUTE | Legacy check-in path may still be carrying compatibility traffic while bypassing the newer wrapper boundary | exact compatibility decision; canonical raw `10c` export | confirm all mobile, web, dashboard, and support callers; search for raw RPC name everywhere | inspect body authorization and event or staff ownership checks | `OBSERVED_IN_08F` with 217 calls | BLOCKED | full ACL rollback and legacy compatibility rollback required |
| P0 | `public.checkin_ticket_v2_unsafe(uuid, text)` live, SECURITY DEFINER, service_role only | Unsafe inner function must not be exposed to client roles | canonical raw `10c` export; exact non-wrapper caller proof | confirm only wrapper or internal callers exist | verify wrapper-only expectation against body and any inbound calls | not directly targeted in `08f` | READY_TO_REVIEW_NOT_PATCH | keep service-role or internal only |
| P0 | `purchase_event_ticket_v2` through `v5` exact live family | Multiple live versions with only partial caller proof creates unresolved authority and deprecation risk | exact-signature caller comparison; canonical raw `09` and `10c` normalization; body parity review | resolve all active callers of `v2`, `v3`, `v4` overloads, and `v5` | verify guard coverage per exact signature, not by family name | purchase family function names are `OBSERVED_IN_08F`; exact overload-level observation remains unresolved, especially the two `v4` overloads | BLOCKED | exact-signature rollback for every touched function |
| P0 | `create_reservation_v1(uuid)` and both `create_reservation_v2` signatures live | Reservation contract is split and current static callers prove only part of the family | exact-signature caller comparison; canonical raw `09` and `10c` normalization; body parity review | resolve all mobile, web, and dashboard callers | verify body parity and cross-entitlement guard coverage for each signature | `create_reservation_v1` and `create_reservation_v2` function names are `OBSERVED_IN_08F`; exact five-argument versus six-argument `v2` observation remains unresolved | BLOCKED | exact-signature rollback required |
| P0 | `public.search_users_v2(text, integer)` live; stale `search_users_v1` caller still present | Production or source drift can break live surfaces and confuses canonical API ownership | response compatibility check; canonical raw `10a` export | remove or migrate all `search_users_v1` callers, including ticket-claims flow | verify `search_users_v2` body authorization and intended caller scope | `OBSERVED_IN_08F` with 598 calls | BLOCKED | compatibility rollback required for caller migration |
| P0 | live `public.dm_*` RPC family: eight exact SECURITY DEFINER signatures, anon/authenticated/service_role execute, zero trigger attachments; authenticated-only table policies and RI-only triggers are production-validated | Current live grants include anon despite authenticated table policies, authenticated dashboard usage, and high runtime activity; body safety cannot be inferred from the structural export | exact live function-body authorization; `auth.uid()` enforcement; participant membership; persona ownership or scope; sender or target-user authorization; mutation impact; caller-body parity | search all dashboard, mobile, web, Edge, and backend callers; mobile statically references all eight RPCs and dashboard statically references five | inspect persona scope, participant checks, sender or target-user authorization, message mutation authorization, and any body path that can bypass authenticated-only table policies | active family observed; `dm_archive_conversation_v1` specifically not observed | BLOCKED | full DM RPC ACL rollback required before any later change |
| P1 | push delivery RPC family and cron helper | Push contract spans trigger, outbox, edge dispatch, and cron; `07d` linkage is now confirmed, but final contract exports remain incomplete | `10h` through `10j` remain pending; raw `07d` export not yet canonical | search all Edge, operator, and internal callers | verify scheduler helper and dispatcher linkages remain backend-only | cron linkage confirmed; helper runtime not fully export-backed | OPEN | exact ACL and scheduler rollback plan required |

## 4. Binding Outcome

- No RPC removal is approved yet.
- `mark_order_paid_v1(uuid)` remains the immediate body-review and caller-review priority.
- Legacy `check_in_ticket(text, uuid)` remains blocked pending compatibility review.
- `search_users_v2(text, integer)` is the production canonical search candidate.
- `search_users_v1` caller migration is a technical implementation-ready bug-fix candidate once response compatibility is verified.
- Version families require exact-signature caller and body comparison.
- Runtime not observed does not approve removal.
- Backend-only payment confirmation and unsafe check-in functions remain service-role only.

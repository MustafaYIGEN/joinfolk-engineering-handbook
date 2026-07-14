# Pruning Candidate Register

## 1. Metadata

- Status: Draft
- Version: 0.3
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-14
- Source confidence: Production ACL exports + runtime confirmations through `08f` + family confirmations through `09` and `10c`
- canonical: false

## 2. Purpose

This register tracks objects that may later enter deprecation or pruning review.

No removal is approved by this register.

## 3. Binding Rule

Pruning remains blocked until exact production presence, static callers, trigger dependencies, policy dependencies, FK or view dependencies, DB caller dependencies, Edge or cron dependencies, runtime evidence, retention review, and rollback or recreation definitions are complete.

Runtime not observed is not removal approval.

## 4. Candidate Register

| Candidate | Current evidence | Current class | Why removal is blocked | Earliest safe next state |
| --- | --- | --- | --- | --- |
| `public.search_users_v1(text, integer)` caller drift | production function absent in current ACL export; stale static caller still exists in `joinfolk-web/lib/ticket-claims.v1.ts:304` | `DEPRECATE_OBSERVE_CANDIDATE` | export absence does not prove object removal and caller migration is not complete | resolve caller drift toward `search_users_v2`, then observe |
| `public.check_in_ticket(text, uuid)` | live legacy grant surface, static mobile scanner reference, and runtime observation in `08f` | `DEPRECATE_OBSERVE_CANDIDATE` | observed runtime confirms current relevance; compatibility and body authorization are not yet reviewed | complete compatibility audit and approved observation period |
| `purchase_event_ticket_v2` / `v3` / `v4` / `v5` family | multiple live versions with purchase family function-name runtime observation | `DUPLICATE_OR_SUPERSEDED` candidate only | observed runtime proves active family use; exact overload-level runtime use is unresolved and canonical exact signature is not yet bound | complete exact-signature canonicalization review |
| `create_reservation_v1` plus both `create_reservation_v2` signatures | all three exact signatures are live; reservation function-name runtime observation exists | `DUPLICATE_OR_SUPERSEDED` candidate only | canonical version is not yet bound and exact five-argument versus six-argument `v2` runtime use is unresolved | complete exact-signature canonicalization review |
| `public.create_ticket_order_v1(uuid, uuid, integer)` | live function, static caller exists, and not observed in current `08f` sample | `DEPRECATE_OBSERVE_CANDIDATE` | not observed is not non-use proof and static caller still exists | verify caller compatibility and observe over an approved window |
| `public.mark_order_paid_v1(uuid)` | live broad-execute function, static caller exists, and not observed in current `08f` sample | `DEPRECATE_OBSERVE_CANDIDATE` | payment authority is unresolved and not observed is not non-use proof | complete body and caller review first |
| `public.dm_archive_conversation_v1(uuid, text)` | live DM RPC, current runtime sample shows zero calls | `DEPRECATE_OBSERVE_CANDIDATE` | DM family remains active overall and current sample is not enough for removal | complete DM authority review and approved observation window |
| empty application membership on `supabase_realtime` | publication exists with zero application table members; polling-first V1 decision is binding | `KEEP_UNUSED_FOR_V1` and `DEFERRED_POST_LAUNCH_ENHANCEMENT` | this is no longer a pruning target for V1 and no publication mutation is authorized | revisit only in a separate post-launch realtime wave |
| legacy and backup objects not yet inventoried from wave `01` | production object inventory remains incomplete | `UNKNOWN_BLOCKED` | exact dependency closure, retention review, and rollback or recreation definitions are still incomplete | complete missing exports and approved observation window |

## 5. Binding Outcome

- No RPC removal is approved yet.
- No table, view, trigger, publication, bucket, or function drop is approved yet.
- No entry in this register is `REMOVE_APPROVED`.
- Runtime evidence exists, but no candidate has completed an approved deprecation observation window.
- Raw-export hygiene gaps are evidence-quality gaps, not automatic pruning approval.

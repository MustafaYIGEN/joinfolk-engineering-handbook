# Platform Scenario Patch Wave Authorization

## 1. Metadata

- Status: Active authorization map
- Version: 1.0
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-28
- Source confidence: accepted owner decisions and closed source-of-truth gates
- canonical: true
- Implementation status: Future implementation requires separate scoped prompts
- Production mutation status: This document does not authorize production mutation

## 2. Purpose

This document maps platform scenario alignment into patch waves and records what is closed, what is open, and what must stay separate.

It defines sequencing and boundaries. It does not create permission to patch code, run SQL, apply migrations, or change production.

## 3. Closed Waves

| Wave | Scope | Closed evidence |
|---|---|---|
| Wave A1 | Host Workspace ticket/sales RPC ACL grants for `get_event_ticket_products_v1`, `get_event_ticket_queue_v2`, `get_event_stats_v1`, and `get_event_reservations_v1` | Commit `dbca98f8 fix(db): grant host workspace ticket sales RPCs` |
| Wave A2.1 | ProductsPage frontend correction to use canonical layout authority and stop querying nonexistent `venue_layouts.draft_layout` | Commit `92379f3 fix(dashboard): use canonical venue layout authority for products` |
| Social read RPC grants | Authenticated grants for app-facing social read RPCs | Commit `d36adeb0 fix(db): grant authenticated social graph read RPCs` |
| Auth email web-only routing | Browser-only auth confirmation/recovery routing | Commit `524c642 fix(web): keep auth email routes web-only` |

## 4. Next Candidate

Wave A2.2 is the next candidate: server-side immutable published snapshot authority diagnosis.

It should determine how published layout snapshots are created, stored, read, versioned, and consumed by product mapping and buyer display.

Wave A2.2 must not assume new `venue_layouts.draft_layout` or `venue_layouts.published_layout` columns are required.

## 5. Open Waves

| Wave | Scope | Boundary |
|---|---|---|
| Mobile reservation wave | `get_my_reservations_v1`, `create_reservation_v2`, capacity, PII, self/participant state | Reads and mutations must be reviewed separately where needed |
| Profile/block/follower wave | `get_my_blocked_users_v1`, `get_my_host_followers_v1`, block-first visibility semantics | No broad anon/PUBLIC grant |
| Owned media wave | `get_my_owned_media_v1`, `delete_owned_media_v1` | Read/delete split required |
| Event module mutation review | `clear_event_module_v1` | Mutation body and host/event authority review required before any grant |
| Social mutation containment | `send_friend_request_v1`, `respond_friend_request_v1`, `remove_friend_v1`, `unfollow_user_v1`, and related reviewed mutations | Separate from read repair and product surface repair |
| Direct runtime ACL V2 source-of-truth closure | Previously applied direct runtime RPC ACL V2 migration source closure | Separate commit/gate if still open |
| B03 | `20260726023000_unexpected_anon_public_execute_batch_b03.sql` | Untouched unless separately authorized |

## 6. Patch Wave Rules

- No wave may mix product repair with unrelated Advisor warning reduction.
- No wave may mix mutation containment with read-only product repair.
- No wave may include B03 unless the prompt explicitly authorizes B03.
- No wave may widen anon or PUBLIC access unless a public product surface is explicitly proven.
- No wave may revive legacy commerce paths.
- No wave may treat production smoke success as authorization for unrelated repairs.

## 7. Required Gate Shape

Each technical wave must have:

- scenario or product contract evidence
- source/callsite trace
- production catalog evidence where DB/RPC/RLS behavior is involved
- exact candidate scope
- static review
- rollback-only dry-run where applicable
- owner review before production mutation
- post-apply smoke or equivalent verification
- source-of-truth closure after production effect is accepted


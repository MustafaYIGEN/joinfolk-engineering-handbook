# Launch Blocker Register

## 1. Metadata

- Status: Draft
- Version: 0.3
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-14
- Source confidence: Production evidence exports through wave `10c` result confirmations + handbook synthesis
- canonical: false

## 2. Purpose

This register tracks exact DB-to-surface launch blockers exposed by the current evidence boundary.

## 3. Launch Blockers

| Blocker | State | Why it blocks launch readiness |
| --- | --- | --- |
| PAYMENT_AUTHORITY_MARK_ORDER_PAID_V1 | BLOCKER | `public.mark_order_paid_v1(uuid)` is live with PUBLIC, anon, authenticated, and service-role execute while payment authority, body review, and caller review remain unresolved. |
| CHECKIN_LEGACY_COMPATIBILITY | BLOCKER | Legacy `public.check_in_ticket(text, uuid)` remains live, runtime-observed, and not yet reviewed for compatibility or body authorization. |
| PURCHASE_RPC_CANONICALIZATION | BLOCKER | Purchase family function names are runtime-observed, but exact purchase-signature canonicalization remains unresolved, including the two `v4` overloads. |
| RESERVATION_RPC_CANONICALIZATION | BLOCKER | All three reservation exact signatures are live; reservation function names are runtime-observed, but exact five-argument versus six-argument `v2` use remains unresolved. |
| SEARCH_USERS_CALLER_DRIFT | BLOCKER | A stale `search_users_v1` caller remains in source while production evidence points to observed `search_users_v2(text, integer)` as the live contract. |
| DM_BODY_AND_AUTHORITY_REVIEW | BLOCKER | DM family runtime use is proven, but exact bodies, policies, triggers, and authority remain unresolved until `10d` through `10g` are complete. |
| PUSH_FINAL_CONTRACT_CLOSURE | OPEN | Cron production linkage is confirmed, but push contract exports `10h` through `10j` remain open. This is P1 stabilization, not a duplicate P0 blocker. |
| STORAGE_BUCKET_RAW_EXPORT_HYGIENE | OPEN | Production bucket state is result-confirmed: `avatars`, `posters`, `venue-media`, and `venue-posters` are public, while `event-media` and `event-videos` are private. Canonical raw `06a` export remains missing. No storage mutation is authorized. |
| WAVE_07D_CRON_RAW_EXPORT_HYGIENE | OPEN | `07d` result is confirmed, but canonical raw export status still needs evidence normalization. This is not a control-path blocker. |
| PRUNING_WITHOUT_APPROVED_OBSERVATION_WINDOW | BLOCKER | Runtime evidence exists, but no candidate has completed an approved deprecation observation window, retention review is incomplete, dependency closure is incomplete, and rollback or recreation definitions remain incomplete. |

## 4. Closed Decisions

| Decision | State | Notes |
| --- | --- | --- |
| V1_LIVE_UPDATE_POLLING_FIRST | CLOSED / DECIDED | V1 authority is polling-first, realtime is post-launch, polling fallback remains required, empty `supabase_realtime` application membership is non-blocking, and no publication mutation is authorized. |

## 5. Binding Rule

Launch-ready status must remain blocked until every `BLOCKER` item is either:

- closed with production evidence and owner review, or
- explicitly deferred or accepted by the owner in a separate release gate.

Evidence-hygiene `OPEN` items do not become P0 launch blockers merely because a canonical raw CSV is still missing when the production result itself is already confirmed.

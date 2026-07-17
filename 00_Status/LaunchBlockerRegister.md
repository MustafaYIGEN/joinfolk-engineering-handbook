# Launch Blocker Register

## 1. Metadata

- Status: Draft
- Version: 0.3
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-16
- Source confidence: Production evidence exports through wave `10g` result confirmations + handbook synthesis
- canonical: false

## 2. Purpose

This register tracks exact DB-to-surface launch blockers exposed by the current evidence boundary.

## 3. Launch Blockers

| Blocker | State | Why it blocks launch readiness |
| --- | --- | --- |
| CHECKIN_LEGACY_COMPATIBILITY | APPLIED_AND_VERIFIED / CLOSED | Client-reachable legacy `public.check_in_ticket(text, uuid)` execution is contained by canonical commit `745b5846` and migration `20260716013100_p0_legacy_checkin_client_containment`; production body MD5 `a2ab9cba35208d7ec6a9e3e46e676613` and final service-role-only ACL were verified. Classification: `UNUSED_BUT_EXPOSED / HISTORICAL_COMPATIBILITY_SURFACE`; legacy objects remaining does not reopen this blocker. |
| PURCHASE_RPC_CANONICALIZATION | BLOCKER | Purchase family function names are runtime-observed, but exact purchase-signature canonicalization remains unresolved, including the two `v4` overloads. |
| PURCHASE_LEGACY_DIRECT_ISSUANCE_SURFACE | APPLIED_AND_VERIFIED / CLOSED | Legacy direct-issuance `purchase_event_ticket_v3`, both `v4` overloads, and `v5` are client-contained by `20260716013200_p0_purchase_legacy_client_containment`; mobile now uses `createOrder` / `create_commerce_order_v1` as the sole active purchase mutation. This closes only the direct-issuance wave, not `PURCHASE_RPC_CANONICALIZATION`. |
| COMMERCE_ISSUANCE_REPLAY_GAP | P0 / OPEN | Issuance replay/idempotency remains a separate purchase authority decision. |
| INVENTORY_RACE_RISK | P0 / OPEN | Capacity and concurrency behavior remains a separate purchase authority decision. |
| SECTION_PRICE_AUTHORITY_GAP | OPEN | Section-price authority remains unresolved. |
| GIFT_COMMERCE_CONTRACT_DRIFT | OPEN | Gift commerce contract parity remains unresolved. |
| RESERVATION_RPC_CANONICALIZATION | BLOCKER | All three reservation exact signatures are live; reservation function names are runtime-observed, but exact five-argument versus six-argument `v2` use remains unresolved. |
| PUSH_FINAL_CONTRACT_CLOSURE | OPEN | Cron production linkage is confirmed, but push contract exports `10h` through `10j` remain open. This is P1 stabilization, not a duplicate P0 blocker. |
| STORAGE_BUCKET_RAW_EXPORT_HYGIENE | OPEN | Production bucket state is result-confirmed: `avatars`, `posters`, `venue-media`, and `venue-posters` are public, while `event-media` and `event-videos` are private. Canonical raw `06a` export remains missing. No storage mutation is authorized. |
| WAVE_07D_CRON_RAW_EXPORT_HYGIENE | OPEN | `07d` result is confirmed, but canonical raw export status still needs evidence normalization. This is not a control-path blocker. |
| PRUNING_WITHOUT_APPROVED_OBSERVATION_WINDOW | BLOCKER | Runtime evidence exists, but no candidate has completed an approved deprecation observation window, retention review is incomplete, dependency closure is incomplete, and rollback or recreation definitions remain incomplete. |

## 4. Closed Decisions

| Decision | State | Notes |
| --- | --- | --- |
| V1_LIVE_UPDATE_POLLING_FIRST | CLOSED / DECIDED | V1 authority is polling-first, realtime is post-launch, polling fallback remains required, empty `supabase_realtime` application membership is non-blocking, and no publication mutation is authorized. |
| DM_ANON_EXECUTE_CONTAINMENT | CLOSED / APPLIED_AND_VERIFIED | Migration `20260714223000_p0_dm_anon_execute_containment` was applied in production from platform commit `40e804b6`; remote migration history row is present; exact ACL postcondition is PASS 8/8 with anon EXECUTE false, authenticated true, service_role true, and PUBLIC false. |
| PAYMENT_AUTHORITY_MARK_ORDER_PAID_V1 | APPLIED_AND_VERIFIED / CLOSED | The client payment-authority bypass in `public.mark_order_paid_v1(uuid)` is contained by canonical platform commit `35dc24ba` and migration `20260716013000_p0_mark_order_paid_client_containment`; remote migration history contains the version. Exact body MD5 remains `5cbb9851788530daf956a0b283e581b3`; final ACL is PUBLIC=false, anon=false, authenticated=false, service_role=true with explicit service_role EXECUTE=true. No function-body, table, RLS, policy, trigger, UI, Edge Function, or schema change was made; `confirm_order_payment_v1` was not modified; `ticket_orders` and `commerce_orders` contracts were not merged; rollback was not required. |
| SEARCH_USERS_CALLER_DRIFT | CLOSED / NOT_ACTIVE_RUNTIME | Runtime ownership was reconciled: the real mobile runtime is `C:\dev\hostos\apps\mobile` and uses `search_users_v2`; dashboard runtime is `C:\dev\joinfolk-web\dashboard`; web runtime is `C:\dev\joinfolk-web\web`; dashboard and web package roots have no `search_users_v1` or `search_users_v2` caller. The stale `search_users_v1` reference belonged to an inactive root source copy, not the mobile, dashboard, or web runtime. No production database or source patch was required; attempted root-source patch commit `84ab737` was not pushed and was removed before push by resetting to `524c642`. |

## 5. Non-Blocking Notes

| Note | State | Reason |
| --- | --- | --- |
| ROOT_DUPLICATE_SOURCE_PRUNING | P1 / DEFERRED | Root app/lib copies may be stale, but they must not be deleted without a dedicated repo-topology and deployment-ownership audit. This is not an active launch blocker. |
| SUPABASE_MIGRATION_HISTORY_BASELINE_DRIFT | OPEN / separate operational blocker | Migration list showed many historical local migrations absent from remote history. Do not perform mass migration repair, `db push`, `--include-all`, or linked reset; this note must not reopen `PAYMENT_AUTHORITY_MARK_ORDER_PAID_V1` or `CHECKIN_LEGACY_COMPATIBILITY`. |

## 6. Binding Rule

Launch-ready status must remain blocked until every `BLOCKER` item is either:

- closed with production evidence and owner review, or
- explicitly deferred or accepted by the owner in a separate release gate.

Evidence-hygiene `OPEN` items do not become P0 launch blockers merely because a canonical raw CSV is still missing when the production result itself is already confirmed.

DM authority and anon EXECUTE containment are no longer active blockers in this register:

- `11a` exact production-body evidence is `COMPLETE_VALIDATED`
- all eight live DM RPC bodies explicitly reject unauthenticated callers
- no active caller-body mismatch was confirmed
- `DM_CONTRACT_STRUCTURE` remains `PASS`
- `DM_BODY_AUTHORIZATION` remains `PASS`
- `DM_STATIC_CALLER_PARITY` remains `PASS_WITH_NOTES`
- `DM_BODY_AND_AUTHORITY_REVIEW` remains `CLOSED`
- `DM_ANON_EXECUTE_CONTAINMENT` is `APPLIED_AND_VERIFIED`
- production migration version `20260714223000` is present in remote history
- migration commit `40e804b6` applied the exact anon-only ACL containment
- rollback was not used
- mobile and dashboard authenticated manual UAT passed as operator-attested manual evidence, not automated test evidence

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
| PAYMENT_AUTHORITY_MARK_ORDER_PAID_V1 | BLOCKER | `public.mark_order_paid_v1(uuid)` is live with PUBLIC, anon, authenticated, and service-role execute while payment authority, body review, and caller review remain unresolved. |
| CHECKIN_LEGACY_COMPATIBILITY | BLOCKER | Legacy `public.check_in_ticket(text, uuid)` remains live, runtime-observed, and not yet reviewed for compatibility or body authorization. |
| PURCHASE_RPC_CANONICALIZATION | BLOCKER | Purchase family function names are runtime-observed, but exact purchase-signature canonicalization remains unresolved, including the two `v4` overloads. |
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
| SEARCH_USERS_CALLER_DRIFT | CLOSED / NOT_ACTIVE_RUNTIME | Runtime ownership was reconciled: the real mobile runtime is `C:\dev\hostos\apps\mobile` and uses `search_users_v2`; dashboard runtime is `C:\dev\joinfolk-web\dashboard`; web runtime is `C:\dev\joinfolk-web\web`; dashboard and web package roots have no `search_users_v1` or `search_users_v2` caller. The stale `search_users_v1` reference belonged to an inactive root source copy, not the mobile, dashboard, or web runtime. No production database or source patch was required; attempted root-source patch commit `84ab737` was not pushed and was removed before push by resetting to `524c642`. |

## 5. Non-Blocking Notes

| Note | State | Reason |
| --- | --- | --- |
| ROOT_DUPLICATE_SOURCE_PRUNING | P1 / DEFERRED | Root app/lib copies may be stale, but they must not be deleted without a dedicated repo-topology and deployment-ownership audit. This is not an active launch blocker. |

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

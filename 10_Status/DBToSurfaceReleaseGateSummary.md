# DB-to-Surface Release Gate Summary

## 1. Metadata

- Status: Draft
- Version: 0.3
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-14
- canonical: false

## 2. Purpose

This record summarizes the binding release-gate state for the JoinFolk DB-to-surface audit.

## 3. Gate Summary

| Gate | State | Notes |
| --- | --- | --- |
| PROD_FUNCTION_AUTHORITY_BASELINE | OPEN | Wave 02 ACL export is complete and waves `09` and `10c` materially improve family certainty, but several P0 families remain unresolved. |
| PROD_OBJECT_INVENTORY_COMPLETENESS | OPEN | Wave 01 raw exports remain missing canonically. |
| PROD_RUNTIME_EVIDENCE | PASS_WITH_LIMITATIONS | table runtime evidence exists; `pg_stat_statements` evidence exists; `pg_stat_user_functions` is expected zero because `track_functions = none`; zero observation is not non-use proof. |
| PROD_VERSIONED_RPC_EVIDENCE | PASS | wave `09` family evidence is confirmed; raw normalization remains open but execution is not pending. |
| PROD_PRIORITY_CONTRACT_EVIDENCE | PARTIAL | `10a` through `10c` are confirmed; `10d` through `10j` remain pending. |
| PAYMENT_AUTHORITY_MARK_ORDER_PAID_V1 | BLOCKER | `mark_order_paid_v1` remains the immediate blocker; payment authority is not yet bound. |
| CHECKIN_LEGACY_COMPATIBILITY | BLOCKER | Legacy `check_in_ticket` remains live, observed, and not yet reviewed for compatibility. |
| PURCHASE_RPC_CANONICALIZATION | BLOCKER | Purchase family function names are observed, but exact overload-level runtime use remains unresolved and exact-signature canonicalization remains open. |
| RESERVATION_RPC_CANONICALIZATION | BLOCKER | All three reservation exact signatures are live; reservation function names are observed, but exact `create_reservation_v2` overload-level runtime use remains unresolved. |
| SEARCH_USERS_CALLER_DRIFT | BLOCKER | Stale `search_users_v1` caller drift remains unresolved while `search_users_v2` is live and observed. |
| DM_BODY_AND_AUTHORITY_REVIEW | BLOCKER | DM family runtime use is proven, but exact authority and body review remain open until `10d` through `10g`. |
| PUSH_DELIVERY_CHAIN | PARTIAL_PASS | cron job linkage is confirmed; service-only posture is strong; `10h` through `10j` remain pending. |
| STORAGE_BUCKET_RAW_EXPORT_HYGIENE | OPEN | Production bucket state is result-confirmed, canonical raw `06a` remains missing, and no storage mutation is authorized. |
| REALTIME_PRODUCT_CONTRACT | DECIDED | `POLLING_FIRST_V1`; launch non-blocking; realtime is post-launch only. |
| PRUNING_AUTHORIZATION | BLOCKED | Runtime evidence exists, but no candidate has completed an approved deprecation observation window and no removal is approved. |

## 4. Current Outcome

PASS:

- service-only posture is already live for `_issue_tickets_from_order_v1`, `confirm_order_payment_v1`, `expire_stale_orders_v1`, `checkin_ticket_v2_unsafe`, and the push outbox worker functions
- DM base tables are RLS-enabled with authenticated policies
- push outbox table is RLS-enabled and service-role scoped
- runtime evidence exists and confirms positive usage for purchase family function names, reservation function names, legacy and v2 check-in names, `search_users_v2`, and the DM family
- versioned RPC family split is confirmed
- V1 live-update authority is decisively polling-first

OPEN:

- canonical raw export hygiene remains incomplete for waves `01`, `06a`, `07`, `08`, `09`, and `10a` through `10c`
- storage bucket raw export hygiene remains open
- push final contract closure remains P1 open pending `10h` through `10j`

BLOCKER:

- payment authority review
- legacy check-in compatibility review
- purchase family exact-signature canonicalization
- reservation family exact-signature canonicalization
- search caller drift
- DM body and authority review

RISK:

- DM live grants remain broader than the proven dashboard surface
- purchase and reservation runtime evidence is name-level rather than exact overload-level

DECISION REQUIRED:

- none in this evidence-reconciliation wave

## 5. Next Authorized Audit Action

The next authorized audit actions are:

1. normalize existing raw Downloads evidence into canonical exports where available for `08c`, `08f`, and `09`
2. normalize or capture result-confirmed but raw-missing evidence for `07d` and `10a` through `10c`
3. execute `10d` through `10j`
4. run the polling-contract technical verification wave

No implementation wave is authorized before those remaining evidence gaps are reviewed.

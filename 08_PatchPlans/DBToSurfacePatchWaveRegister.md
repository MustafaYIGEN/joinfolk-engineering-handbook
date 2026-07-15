# DB-to-Surface Patch Wave Register

## 1. Metadata

- Status: Draft
- Version: 0.3
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-16
- Source confidence: Audit synthesis only
- canonical: false
- Implementation status: DM anon containment applied and verified; further implementation not authorized

## 2. Purpose

This register proposes the minimum future patch waves implied by the current evidence.

It does not authorize implementation.

## 3. Proposed Waves

| Wave | Scope | Preconditions | Current status |
| --- | --- | --- | --- |
| DBSURF-P0-PAYMENT-AUTHORITY | `mark_order_paid_v1`, `create_commerce_order_v1`, `confirm_order_payment_v1`, `_issue_tickets_from_order_v1`, `expire_stale_orders_v1` | function-body review completed; exact caller inventory completed; canonicalization of `10b` evidence or equivalent reviewed; rollback pack written | BLOCKED |
| DBSURF-P0-CHECKIN-COMPAT | legacy `check_in_ticket`, `checkin_ticket_v2`, `checkin_ticket_by_id_v2`, unsafe wrappers | compatibility and caller audit completed; exact body review completed; canonicalization of `10c` evidence or equivalent reviewed; rollback pack written | BLOCKED |
| DBSURF-P0-PURCHASE-CANONICALIZATION | `purchase_event_ticket_v2` through `v5` exact signatures | exact-signature static caller comparison; exact-signature body parity review; `09` family evidence reviewed; `10c` purchase evidence reviewed; purchase family function-name runtime evidence reviewed; exact overload-level runtime ambiguity recorded; rollback pack written | BLOCKED |
| DBSURF-P0-RESERVATION-CANONICALIZATION | `create_reservation_v1` and both `create_reservation_v2` signatures | exact-signature static caller comparison; exact-signature body parity review; `09` family evidence reviewed; `10c` reservation evidence reviewed; reservation function-name runtime evidence reviewed; exact `v2` overload ambiguity recorded; rollback pack written | BLOCKED |
| DBSURF-P0-SEARCH-DRIFT | `SEARCH_USERS_CALLER_DRIFT` runtime-ownership reconciliation | mobile runtime ownership proven under `C:\dev\hostos\apps\mobile`; dashboard runtime proven under `C:\dev\joinfolk-web\dashboard`; web package proven under `C:\dev\joinfolk-web\web`; stale root `search_users_v1` reference classified as inactive root source copy; production `search_users_v2(text, integer)` contract preserved | CLOSED / NOT_ACTIVE_RUNTIME |
| DBSURF-P0-DM-AUTHORITY | DM exact-signature anon containment with no body, RLS, policy, or caller change | `10d` through `10g` reviewed as `COMPLETE_VALIDATED`; `11a` reviewed as `COMPLETE_VALIDATED`; exact live function-body authorization review completed; exact rollback grants written; production migration version `20260714223000` applied from commit `40e804b6`; remote migration history row present; exact ACL postcondition PASS 8/8; mobile and dashboard authenticated operator-attested manual UAT: PASS; rollback not used | APPLIED_AND_VERIFIED / CLOSED |
| DBSURF-P1-PUSH-RUNTIME | cron, outbox, dispatcher, and push contract closure | `07d` result reviewed; `10h` through `10j` completed; final caller map completed; rollback pack written | OPEN |
| DBSURF-P1-STORAGE-CONTRACT | bucket exposure, route ownership, and parity decisions | canonical `06a_buckets.csv` installed; owner decision on bucket exposure; rollback pack written | OPEN |
| DBSURF-P1-POLLING-CONTRACT-VERIFICATION | verify app-start refresh, foreground/resume refresh, post-mutation refresh, DM unread polling, notification polling, timer pause or stop behavior, duplicate-loop prevention, polling intervals, error and backoff behavior, and explicit refresh | no product decision remains open; test plan and surface inventory prepared | TECHNICAL_VERIFICATION |
| ROOT_DUPLICATE_SOURCE_PRUNING | root app/lib duplicate source topology and deployment ownership | dedicated repo-topology and deployment-ownership audit; prove ownership before any deletion; preserve rollback and source provenance | P1 / DEFERRED |
| DBSURF-P2-PRUNING-OBSERVATION | deprecation observation, retention, and later removals | dependency closure complete; retention review complete; approved runtime observation window complete; rollback or recreation definitions preserved | BLOCKED |
| DBSURF-POSTLAUNCH-REALTIME-ENHANCEMENT | publication membership design, subscription lifecycle, reconnect handling, duplicate-delivery handling, polling fallback retention, rollout and rollback plan | separate product approval; RLS compatibility audit; subscription lifecycle design; rollback plan; production verification plan | DEFERRED_POST_LAUNCH |

## 4. Binding Rule

- No product decision remains open for polling-first V1.
- No wave above may proceed to implementation until its preconditions are met and a separate owner-approved implementation prompt exists.
- Realtime is not a V1 launch requirement and belongs only to the deferred post-launch enhancement wave.

## 5. DM Applied And Verified Scope

`DBSURF-P0-DM-AUTHORITY` is closed for this exact applied DB wave:

- `REVOKE EXECUTE ON FUNCTION public.dm_archive_conversation_v1(uuid,text) FROM anon;`
- `REVOKE EXECUTE ON FUNCTION public.dm_delete_message_v1(uuid) FROM anon;`
- `REVOKE EXECUTE ON FUNCTION public.dm_get_conversations_v1(text,integer,integer) FROM anon;`
- `REVOKE EXECUTE ON FUNCTION public.dm_get_messages_v1(uuid,integer,timestamp with time zone) FROM anon;`
- `REVOKE EXECUTE ON FUNCTION public.dm_get_or_create_conversation_v1(uuid,text,text) FROM anon;`
- `REVOKE EXECUTE ON FUNCTION public.dm_get_unread_count_v1(text) FROM anon;`
- `REVOKE EXECUTE ON FUNCTION public.dm_mark_read_v1(uuid,text) FROM anon;`
- `REVOKE EXECUTE ON FUNCTION public.dm_send_message_v1(uuid,text,text) FROM anon;`

Applied preservation results:

- `anon` EXECUTE is now false for all eight exact signatures
- `authenticated` EXECUTE remains true for all eight exact signatures
- `service_role` EXECUTE remains true for all eight exact signatures
- `PUBLIC` EXECUTE remains false for all eight exact signatures
- function bodies remained unchanged
- RLS and policies remained unchanged
- callers remained unchanged
- no DM RPC was removed
- production migration history contains version `20260714223000`
- platform commit: `40e804b6 fix(security): contain anonymous DM RPC execution`
- production apply: PASS
- rollback: not used

Exact rollback contract retained but not used:

- `GRANT EXECUTE ON FUNCTION public.dm_archive_conversation_v1(uuid,text) TO anon;`
- `GRANT EXECUTE ON FUNCTION public.dm_delete_message_v1(uuid) TO anon;`
- `GRANT EXECUTE ON FUNCTION public.dm_get_conversations_v1(text,integer,integer) TO anon;`
- `GRANT EXECUTE ON FUNCTION public.dm_get_messages_v1(uuid,integer,timestamp with time zone) TO anon;`
- `GRANT EXECUTE ON FUNCTION public.dm_get_or_create_conversation_v1(uuid,text,text) TO anon;`
- `GRANT EXECUTE ON FUNCTION public.dm_get_unread_count_v1(text) TO anon;`
- `GRANT EXECUTE ON FUNCTION public.dm_mark_read_v1(uuid,text) TO anon;`
- `GRANT EXECUTE ON FUNCTION public.dm_send_message_v1(uuid,text,text) TO anon;`

Applied verification record:

- migration dry-run: PASS
- production apply: PASS
- exact ACL postcondition: PASS 8/8
- migration history: PASS
- mobile authenticated manual UAT: PASS
  - conversation list: PASS
  - message load: PASS
  - message send and refresh: PASS
  - mark-read/unread update: PASS
  - create/open conversation from user profile: PASS
- dashboard authenticated host manual UAT: PASS
  - inbox: PASS
  - message load: PASS
  - message send: PASS
  - mark read: PASS
  - Sidebar/Overview unread count: PASS
- no permission denied, `42501`, `AUTH_REQUIRED`, missing-function, or RPC failure was observed in the manual UAT
- these UI checks are operator-attested manual UAT, not automated test evidence

Applied failure-mode change:

- before: anon can enter the function and receives function-level `AUTH_REQUIRED`
- after: anon is denied `EXECUTE` before function entry
- this is expected and accepted because no legitimate unauthenticated DM caller was confirmed

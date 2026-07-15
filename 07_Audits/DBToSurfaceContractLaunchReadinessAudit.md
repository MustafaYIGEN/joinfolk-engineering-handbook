# DB-to-Surface Contract Launch Readiness Audit

## 1. Metadata

- Status: Draft
- Version: 0.3
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-16
- Source confidence: Handbook decisions + canonical production exports through wave 06 + production-result confirmations through wave 10g + read-only local source inspection
- canonical: false
- Implementation status: Not authorized
- Production mutation status: DM anon containment migration applied and verified; no other mutation authorized by this audit

## 2. Purpose

This document binds the current JoinFolk DB-to-surface contract audit to the available production evidence pack, operator-confirmed production result summaries, and local static caller evidence.

This is an evidence synthesis only.

It does not authorize application changes, SQL execution, migrations, grants, revokes, drops, function replacements, runtime testing, or production mutation.

## 3. Evidence Boundary

Evidence used in this audit:

- Handbook decisions and prior contract audits.
- Canonical production exports currently normalized under `C:\dev\joinfolk-evidence\db-surface-audit\2026-07-13\exports`.
- Production export status and manifest files under `C:\dev\joinfolk-evidence\db-surface-audit\2026-07-13`.
- Additional production-result confirmations for wave `07d`, wave `08`, wave `09`, and wave `10a` through `10g`, with `10h` through `10j` still pending.
- Read-only local inspection of `C:\dev\hostos`, `C:\dev\hostos\apps\mobile`, and `C:\dev\joinfolk-web`.

Canonical raw exports currently installed:

- `02_functions_acl.csv`
- `03a_triggers.csv`
- `03b_dependencies.csv`
- `04a_table_security.csv`
- `04b_policies.csv`
- `05b_view_dependencies.csv`
- `06b_storage_policies.csv`

Additional production results confirmed in this audit wave:

- `07b_cron_jobs_if_present.cron_jobs`
- `08_runtime_statistics.availability`
- `08_runtime_statistics.database_reset`
- `08_runtime_statistics.table_stats`
- `08_runtime_statistics.function_stats`
- `08_runtime_statistics.pg_stat_statements_note`
- `08b_pg_stat_statements_if_present.rpc_runtime`
- `09_versioned_rpc_families.inventory`
- `10_priority_contracts.search_functions`
- `10_priority_contracts.payment_order_functions`
- `10_priority_contracts.reservation_ticket_checkin_functions`

Execution confirmation and raw-export validation are tracked separately. A confirmed production result does not imply `COMPLETE_VALIDATED` unless a canonical raw CSV exists in the evidence root.

## 4. Binding Conclusions

- No RPC removal is approved yet.
- `mark_order_paid_v1(uuid)` requires immediate function-body and caller review.
- Legacy `check_in_ticket(text, uuid)` requires compatibility and caller review before any containment or removal decision.
- Purchase and reservation version splits are confirmed.
- `SEARCH_USERS_CALLER_DRIFT` is closed as `NOT_ACTIVE_RUNTIME`: the earlier blocker was based on incorrect runtime ownership, and the stale `search_users_v1` reference belonged to an inactive root source copy rather than the mobile, dashboard, or web runtime.
- Backend-only payment confirmation and unsafe check-in functions must remain service-role only.
- JoinFolk V1 live-update authority is polling-first.
- Realtime is a post-launch enhancement and remains non-blocking for V1 launch.
- Polling remains the mandatory fallback even after realtime is added.
- Runtime evidence exists, but no candidate has completed an approved deprecation observation window.
- Pruning remains blocked until dependency closure, caller closure, retention review, and rollback or recreation definitions are complete.

## 5. Closed Decision

Decision ID:

`V1_LIVE_UPDATE_POLLING_FIRST`

Closed / decided:

- JoinFolk V1 live-update authority is polling-first.
- Realtime is a post-launch enhancement.
- Polling remains the mandatory fallback even after realtime is added.
- V1 launch must not depend on Supabase publication membership.
- Empty `supabase_realtime` application membership is not a launch blocker for V1.
- No production publication mutation is authorized in this wave.
- Existing `postgres_changes` consumers are not authoritative for V1 until a separate realtime enhancement wave is approved and verified.
- Explicit refresh remains required after app start, foreground or resume, successful user mutation, and relevant screen entry.
- DM unread and notification freshness may rely on controlled polling.
- Polling frequency and lifecycle behavior still require technical verification, but the polling-first product decision itself is final.

Binding states:

- `REALTIME_PRODUCT_CONTRACT: DECIDED`
- `V1_LIVE_UPDATE_AUTHORITY: POLLING_FIRST`
- `REALTIME_LAUNCH_EFFECT: NON_BLOCKING`
- `REALTIME_FUTURE_STATE: POST_LAUNCH_ENHANCEMENT`
- `POLLING_FALLBACK: REQUIRED`

## 6. Evidence Availability Matrix

| Wave | Scope | Current state | Launch-readiness effect |
| --- | --- | --- | --- |
| 01 | Objects / relations | `RESULT_CONFIRMED_BUT_RAW_EXPORT_MISSING` | Production inventory remains incomplete for legacy and backup triage. |
| 02 | Function ACL inventory | `COMPLETE_VALIDATED` | Strongest live function authority evidence. |
| 03 | Triggers / dependencies | triggers and `pg_depend` are `COMPLETE_VALIDATED`; sequence ownership is `RESULT_CONFIRMED_BUT_RAW_EXPORT_MISSING` | Trigger linkage is usable; dependency graph remains partial. |
| 04 | RLS / policies | `COMPLETE_VALIDATED` | Strong table and policy evidence for DM and push outbox tables. |
| 05 | FKs / views | foreign keys are `RESULT_CONFIRMED_BUT_RAW_EXPORT_MISSING`; view dependencies are `COMPLETE_VALIDATED` | View dependency evidence is partial; FK-driven reachability remains incomplete. |
| 06 | Storage | bucket inventory is `RESULT_CONFIRMED_BUT_RAW_EXPORT_MISSING`; storage policies are `COMPLETE_VALIDATED` | Production bucket state is result-confirmed; canonical raw bucket export remains missing. |
| 07 | Cron / realtime | `07a` to `07c` are `RESULT_CONFIRMED_BUT_RAW_EXPORT_MISSING`; `07d` is result confirmed with canonical raw export still open | Cron linkage is confirmed; realtime publication facts are confirmed; raw-export hygiene remains open. |
| 08 | Runtime statistics | `08a`, `08b`, `08d`, and `08e` are result confirmed; `08c` and `08f` raw CSVs exist in Downloads but are not canonically installed | Runtime evidence now exists with limitations; zero function-stat rows are expected because `track_functions = none`. |
| 09 | Versioned RPC families | result confirmed; raw CSV exists in Downloads but is not canonically installed | Version splits are confirmed; no version is canonical solely by suffix. |
| 10 | Priority contracts | `PARTIAL_RESULT_CONFIRMED` for `10a` through `10c`; `10d` through `10g` are `COMPLETE_VALIDATED`; `10h` through `10j` remain `PENDING_EXECUTION` | P0 contract evidence is materially improved; DM structural evidence is validated, DM anon containment is applied and verified, and push closure remains incomplete. |

## 7. Technical Interpretation Rules

- `RESULT_CONFIRMED_BUT_RAW_EXPORT_MISSING` is evidence of execution, not canonical raw-export validation.
- Runtime observed is positive usage evidence.
- Runtime not observed is not proof of non-use and does not approve removal.
- `pg_stat_user_functions` zero rows are expected when `track_functions = none`.
- `pg_stat_statements` observation does not identify the exact frontend caller.
- Overloaded signatures cannot be distinguished when runtime evidence matched only function names.
- `pg_depend` partial counts are not removal evidence.
- PUBLIC or anon EXECUTE on a SECURITY DEFINER function requires body review, but is not by itself proof of exploitability.
- No grant, revoke, or removal is authorized by this audit.

## 8. Capability Matrix Status Vocabulary

- `production_presence`: `LIVE_PROD`, `RESULT_CONFIRMED_BUT_RAW_EXPORT_MISSING`, `PROD_ABSENT_IN_02_EXPORT`, `PARTIAL_RESULT_CONFIRMED`, `PENDING_EXECUTION`
- `function_body_authorization_status`: `PROVEN_LOCAL_AUTH_UID_GUARD`, `PROVEN_LOCAL_SERVICE_INTERNAL`, `WRAPPER_OVER_UNSAFE_LOCAL`, `NOT_REVIEWED`, `LOCAL_PROVENANCE_MISSING`
- `runtime_observed`: `OBSERVED_IN_08F`, `NOT_OBSERVED_IN_08F`, `RESULT_CONFIRMED_ONLY`, `EXPECTED_ZERO_ROWS`, `NOT_TARGETED_BY_08F`
- `current_authority_status`: `AUTH_ONLY_EXPECTED_BUT_BROADER_LIVE`, `SERVICE_ONLY_LIVE`, `LIVE_AUTH_ONLY`, `LIVE_LEGACY_COMPAT`, `KEEP_UNUSED_FOR_V1`, `DEFERRED_POST_LAUNCH_ENHANCEMENT`, `STORAGE_BUCKET_RAW_EXPORT_HYGIENE`

## 9. Capability Matrix

```csv
domain,object_type,schema,object_name,exact_signature,production_presence,security_definer,public_execute,anon_execute,authenticated_execute,service_role_execute,rls_enabled,runtime_observed,runtime_calls,static_callers,route_or_surface,function_body_authorization_status,trigger_dependency,foreign_key_or_view_dependency,cron_or_edge_dependency,current_authority_status,severity,required_decision,proposed_patch_wave,rollback_requirement,final_classification,evidence_gap
commerce_order,rpc,public,create_ticket_order_v1,"public.create_ticket_order_v1(uuid, uuid, integer)",LIVE_PROD,true,true,true,true,true,,NOT_OBSERVED_IN_08F,0,"joinfolk-web/lib/ticket-orders.v1.ts:32",WEB_CHECKOUT,PROVEN_LOCAL_AUTH_UID_GUARD,no,unknown,no,AUTH_ONLY_EXPECTED_BUT_BROADER_LIVE,P1,"Confirm whether legacy ticket_orders path remains launch scope or must consolidate into commerce_orders path",DBSURF-P0-PAYMENT-AUTHORITY,"Reversible EXECUTE grant rollback required before any later revoke",DEPRECATE_OBSERVE,"runtime not observed is not removal proof; 10b result confirmed but raw export missing"
commerce_order,rpc,public,mark_order_paid_v1,"public.mark_order_paid_v1(uuid)",LIVE_PROD,true,true,true,true,true,,NOT_OBSERVED_IN_08F,0,"joinfolk-web/lib/ticket-orders.v1.ts:56",WEB_CHECKOUT,PROVEN_LOCAL_AUTH_UID_GUARD,no,unknown,no,AUTH_ONLY_EXPECTED_BUT_BROADER_LIVE,P0,"Immediate body and caller review; decide whether this dev stub is still part of the live payment authority",DBSURF-P0-PAYMENT-AUTHORITY,"Full ACL rollback plus compatibility rollback for any caller migration",UNKNOWN_BLOCKED,"broad execute remains live; runtime not observed does not approve removal"
commerce_order,rpc,public,create_commerce_order_v1,"public.create_commerce_order_v1(uuid, jsonb, boolean, uuid, jsonb, text, uuid)",LIVE_PROD,true,true,true,true,true,,OBSERVED_IN_08F,66,"no direct static caller proven in current app repos",NO_DIRECT_UI_CALLER_PROVEN,PROVEN_LOCAL_AUTH_UID_GUARD,no,unknown,possible_backend_only,AUTH_ONLY_EXPECTED_BUT_BROADER_LIVE,P0,"Decide whether this is the canonical order creation path and align grants to proven callers only after exact caller review",DBSURF-P0-PAYMENT-AUTHORITY,"Grant rollback required; do not change body until contract export is reviewed",UNKNOWN_BLOCKED,"10b result confirmed but raw export missing; direct caller proof remains incomplete"
commerce_order,rpc,public,_issue_tickets_from_order_v1,"public._issue_tickets_from_order_v1(uuid)",LIVE_PROD,true,false,false,false,true,,NOT_TARGETED_BY_08F,,"called from create_commerce_order_v1 in hostos/supabase/migrations/20260626_commerce_standing_tickets_v1.sql:255",BACKEND_INTERNAL,PROVEN_LOCAL_SERVICE_INTERNAL,no,unknown,possible_backend_only,SERVICE_ONLY_LIVE,P0,"Keep backend-only; verify inbound callers before any further narrowing",DBSURF-P0-PAYMENT-AUTHORITY,"Service-role or internal EXECUTE rollback required if changed",KEEP_BACKEND_ONLY,"10b result confirmed but raw export missing; 08f cannot distinguish helper-only internal execution"
commerce_order,rpc,public,confirm_order_payment_v1,"public.confirm_order_payment_v1(uuid, text, text, jsonb)",LIVE_PROD,true,false,false,false,true,,OBSERVED_IN_08F,8,"no direct static caller proven in current app repos",BACKEND_PAYMENT_CONFIRMATION,NOT_REVIEWED,no,unknown,possible_backend_only,SERVICE_ONLY_LIVE,P0,"Keep service-role only; inspect body for explicit provider and service-only guard before any launch sign-off",DBSURF-P0-PAYMENT-AUTHORITY,"Service-role grant rollback required; no client grant allowed",KEEP_BACKEND_ONLY,"10b result confirmed but raw export missing"
commerce_order,rpc,public,expire_stale_orders_v1,"public.expire_stale_orders_v1()",LIVE_PROD,true,false,false,false,true,,NOT_TARGETED_BY_08F,,"no direct static caller proven in current app repos",BACKEND_CLEANUP,NOT_REVIEWED,no,unknown,cron_confirmed,SERVICE_ONLY_LIVE,P1,"Keep backend-only; verify caller path against confirmed cron inventory and remaining push or job provenance",DBSURF-P0-PAYMENT-AUTHORITY,"Service-role or internal rollback required if touched",KEEP_BACKEND_ONLY,"service-only posture proven; specific runtime row not targeted in 08f"
checkin,rpc,public,check_in_ticket,"public.check_in_ticket(text, uuid)",LIVE_PROD,true,false,true,true,true,,OBSERVED_IN_08F,217,"joinfolk-web/app/(tabs)/event/[id]/scan.tsx:85",MOBILE_SCANNER_LEGACY,NOT_REVIEWED,no,unknown,no,LIVE_LEGACY_COMPAT,P0,"Review compatibility, body authorization, and whether current mobile surface still depends on this legacy signature",DBSURF-P0-CHECKIN-COMPAT,"Full ACL rollback and mobile compatibility rollback required",DEPRECATE_OBSERVE,"observed runtime confirms live compatibility relevance"
checkin,rpc,public,checkin_ticket_v2,"public.checkin_ticket_v2(uuid, text)",LIVE_PROD,true,false,false,true,true,,OBSERVED_IN_08F,202,"joinfolk-web/lib/tickets.v2.ts:202",MOBILE_TICKET_V2,WRAPPER_OVER_UNSAFE_LOCAL,no,unknown,no,LIVE_AUTH_ONLY,P1,"Keep exposed to authenticated caller; verify canonical wrapper versus by-id path",DBSURF-P0-CHECKIN-COMPAT,"Authenticated grant rollback required if changed",KEEP_EXPOSED,"observed runtime is name-level only, not exact caller attribution"
checkin,rpc,public,checkin_ticket_by_id_v2,"public.checkin_ticket_by_id_v2(uuid, uuid, text)",LIVE_PROD,true,false,false,true,true,,OBSERVED_IN_08F,164,"joinfolk-web/lib/tickets.v2.ts:291; joinfolk-web/dashboard/src/lib/api.ts:307",MOBILE_AND_DASHBOARD_SCANNER,NOT_REVIEWED,no,unknown,no,LIVE_AUTH_ONLY,P1,"Keep exposed to authenticated caller; verify whether this is the canonical launch scanner path",DBSURF-P0-CHECKIN-COMPAT,"Authenticated grant rollback required if changed",KEEP_EXPOSED,"observed runtime is name-level only, not exact caller attribution"
checkin,rpc,public,checkin_ticket_v2_unsafe,"public.checkin_ticket_v2_unsafe(uuid, text)",LIVE_PROD,true,false,false,false,true,,NOT_TARGETED_BY_08F,,"wrapped by checkin_ticket_v2 in local provenance",BACKEND_INTERNAL,WRAPPER_OVER_UNSAFE_LOCAL,no,unknown,no,SERVICE_ONLY_LIVE,P0,"Keep service-role or internal only; do not expose to clients",DBSURF-P0-CHECKIN-COMPAT,"Service-role rollback required if changed",KEEP_BACKEND_ONLY,"wave 10c confirms live signature; 08f did not target unsafe helper separately"
purchase,rpc_family,public,purchase_event_ticket,"public.purchase_event_ticket_v2(uuid, uuid); public.purchase_event_ticket_v3(uuid, uuid, integer, boolean, uuid, jsonb, jsonb); public.purchase_event_ticket_v4(uuid, jsonb, boolean, uuid, jsonb); public.purchase_event_ticket_v4(uuid, jsonb, boolean, uuid, uuid[]); public.purchase_event_ticket_v5(uuid, jsonb, boolean, uuid, jsonb)",PARTIAL_RESULT_CONFIRMED,true,true,true,true,true,,OBSERVED_IN_08F,"v2=9; v3=75; v4-name=12; v5=41","joinfolk-web/lib/ticket-sales.v1.ts:130 uses v2; :329 uses v3; no current static caller found for v4 or v5",MOBILE_TICKET_SALES,PARTIAL_LOCAL_GUARD_COVERAGE,no,unknown,no,AUTH_ONLY_EXPECTED_BUT_BROADER_LIVE,P0,"Resolve canonical purchase path; purchase family function names were runtime-observed, but exact overload-level runtime use remains unresolved, especially the two v4 overloads",DBSURF-P0-PURCHASE-CANONICALIZATION,"Grant rollback plus compatibility rollback across all exact signatures required",CONSOLIDATE_TO_CANONICAL,"wave 09 family export confirms live split; runtime is by function name, not overload signature"
reservation,rpc_family,public,create_reservation,"public.create_reservation_v1(uuid); public.create_reservation_v2(uuid, text, integer, text, text); public.create_reservation_v2(uuid, text, integer, text, text, uuid)",PARTIAL_RESULT_CONFIRMED,true,true,true,true,true,,OBSERVED_IN_08F,"v1=4; v2-name=50","joinfolk-web/lib/reservations.v1.ts:60 uses v1",MOBILE_RESERVATIONS,PARTIAL_LOCAL_GUARD_COVERAGE,no,unknown,no,AUTH_ONLY_EXPECTED_BUT_BROADER_LIVE,P0,"All three exact reservation signatures are live in production; create_reservation_v1 and create_reservation_v2 function names were runtime-observed, but exact five-argument versus six-argument v2 runtime use remains unresolved",DBSURF-P0-RESERVATION-CANONICALIZATION,"Grant rollback required for any exact signature change",CONSOLIDATE_TO_CANONICAL,"wave 10c confirms live exact signatures; runtime cannot distinguish v2 overloads"
search,rpc,public,search_users_v2,"public.search_users_v2(text, integer)",PARTIAL_RESULT_CONFIRMED,true,false,false,true,true,,OBSERVED_IN_08F,598,"hostos/apps/mobile/lib/friends.v1.ts",MOBILE_SEARCH_AND_GIFT_RECIPIENT_SEARCH,NOT_REVIEWED,no,unknown,no,LIVE_AUTH_ONLY,P0,"Keep authenticated exposure; mobile active caller uses this production contract; dashboard and web actual package roots have no search caller",DBSURF-P0-SEARCH-DRIFT,"Authenticated grant rollback required only if changed in a separately authorized DB wave",KEEP_EXPOSED,"10a result confirmed but raw export missing; production ACL preserved: PUBLIC=false, anon=false, authenticated=true, service_role=true; mobile operator-attested UAT PASS"
search,rpc,public,search_users_v1,"public.search_users_v1(text, integer)",PROD_ABSENT_IN_02_EXPORT,,,,,,NOT_TARGETED_BY_08F,,"inactive root source copy only: joinfolk-web/lib/ticket-claims.v1.ts",ROOT_DUPLICATE_SOURCE_PRUNING,LOCAL_PROVENANCE_MISSING,no,unknown,no,NOT_ACTIVE_RUNTIME,P1,"Treat root source only as a stale/pruning candidate until a dedicated pruning audit is complete; no launch blocker remains",ROOT_DUPLICATE_SOURCE_PRUNING,"No production or source rollback required; attempted root patch commit 84ab737 was not pushed and was removed by reset to 524c642",P1_DEFERRED,"stale/pruning-candidate root copy, not active mobile/dashboard/web runtime"
dm,table_family,public,dm_relations,"public.dm_conversations; public.dm_messages; public.dm_participants; public.dm_conversations_pkey; public.dm_messages_pkey; public.dm_participants_pkey",LIVE_PROD,,,,,,true,COMPLETE_VALIDATED,,"mobile direct table references exist for dm_participants; dashboard DM callers depend on family RPCs",MOBILE_AND_DASHBOARD_DM,NOT_APPLICABLE,internal_ri_only,unknown,push_indirect,AUTH_ONLY_EXPECTED_BUT_BROADER_LIVE,P0,"Preserve DM backend state; exact relations, RLS state, authenticated-only policies, and RI-only trigger structure are production-validated, and RPC body authorization is now production-validated separately for the DM RPC family",DBSURF-P0-DM-AUTHORITY,"RLS or policy rollback required only in a separate authorized wave",KEEP_BACKEND_ONLY,"10d COMPLETE_VALIDATED: exact live DM tables and primary-key indexes confirmed; 10f COMPLETE_VALIDATED: authenticated-only participant-scoped policies confirmed; 10g COMPLETE_VALIDATED: no custom JoinFolk DM trigger helper observed; 11a COMPLETE_VALIDATED: exact live DM RPC bodies reviewed"
dm,rpc_family,public,dm_rpcs,"public.dm_archive_conversation_v1(uuid,text); public.dm_delete_message_v1(uuid); public.dm_get_conversations_v1(text,integer,integer); public.dm_get_messages_v1(uuid,integer,timestamp with time zone); public.dm_get_or_create_conversation_v1(uuid,text,text); public.dm_get_unread_count_v1(text); public.dm_mark_read_v1(uuid,text); public.dm_send_message_v1(uuid,text,text)",LIVE_PROD,true,false,false,true,true,,OBSERVED_IN_08F,"archive=0; delete=1; get_conversations=694; get_messages=494; get_or_create=45; unread=35589; mark_read=330; send=171","hostos/apps/mobile/lib/dm.v1.ts calls all eight RPCs; joinfolk-web/dashboard/src/lib/dm.ts calls get_conversations, get_messages, send_message, mark_read, and get_unread_count",MOBILE_AND_DASHBOARD_DM,PROD_BODY_VALIDATED,no,policy_linked,push_indirect,LIVE_AUTH_ONLY,P0,"All eight exact live bodies explicitly acquire auth.uid(), reject unauthenticated callers, and current active callers match the production bodies. Exact-signature anon EXECUTE containment is applied and verified with authenticated and service-role grants preserved.",DBSURF-P0-DM-AUTHORITY,"Rollback pack exists for exact anon-only grant restoration; rollback not used",KEEP_EXPOSED,"10e COMPLETE_VALIDATED and 11a COMPLETE_VALIDATED; migration 20260714223000 from commit 40e804b6 applied; remote migration history row present; exact ACL postcondition PASS 8/8: anon=false, authenticated=true, service_role=true, PUBLIC=false; mobile and dashboard authenticated operator-attested manual UAT: PASS; not automated test evidence"
push,table,public,notification_push_deliveries_v1,"public.notification_push_deliveries_v1",LIVE_PROD,,,,,,true,RESULT_CONFIRMED_ONLY,,"hostos/supabase/functions/push-dispatch/index.ts consumes push RPCs",BACKEND_PUSH_OUTBOX,NOT_APPLICABLE,trigger_source,unknown,edge_and_cron,SERVICE_ONLY_LIVE,P1,"Keep backend-only outbox; confirm final contract with remaining 10h through 10j evidence",DBSURF-P1-PUSH-RUNTIME,"Table privilege rollback required in a separate authorized DB wave only",KEEP_BACKEND_ONLY,"07d cron linkage confirmed; 10h through 10j remain pending"
push,rpc_family,public,push_delivery_functions,"public.enqueue_notification_push_delivery_v1(); public.claim_notification_push_deliveries_v1(integer, interval); public.record_notification_push_delivery_result_v1(uuid, integer, text, text); public.invoke_notification_push_dispatch_v1()",PARTIAL_RESULT_CONFIRMED,true,false,false,false,mixed,,RESULT_CONFIRMED_ONLY,,"hostos/supabase/functions/push-dispatch/index.ts:180 calls record_notification_push_delivery_result_v1",EDGE_PUSH_DISPATCH,PROVEN_LOCAL_SERVICE_INTERNAL,"enqueue trigger on notifications_v2 proven in 03a",unknown,edge_and_cron,SERVICE_ONLY_LIVE,P1,"Keep backend-only; cron jobs are confirmed and final push contract closure now depends on 10h through 10j",DBSURF-P1-PUSH-RUNTIME,"Service-role or internal rollback required if any function grant changes later",KEEP_BACKEND_ONLY,"07d result confirmed but raw export missing; 10h through 10j pending"
storage,bucket_family,storage,public_buckets,"avatars; posters; venue-media; venue-posters public; event-media and event-videos private",RESULT_CONFIRMED_BUT_RAW_EXPORT_MISSING,,,,,,,RESULT_CONFIRMED_ONLY,,"joinfolk-web/lib/posterSnapshot.ts; joinfolk-web/lib/event-media.v1.ts; joinfolk-web/dashboard/src/lib/api.ts",MOBILE_WEB_DASHBOARD_MEDIA,NOT_APPLICABLE,no,storage_policy_evidence,possible_edge_usage,STORAGE_BUCKET_RAW_EXPORT_HYGIENE,OPEN,"Production bucket state is result-confirmed; canonical raw 06a export remains missing; no storage mutation is authorized",DBSURF-P1-STORAGE-CONTRACT,"Bucket policy rollback belongs to a separate authorized storage wave",UNKNOWN_BLOCKED,"canonical raw 06a export remains missing"
realtime,publication,public,supabase_realtime,"publication supabase_realtime",RESULT_CONFIRMED_BUT_RAW_EXPORT_MISSING,,,,,,,RESULT_CONFIRMED_ONLY,,"joinfolk-web/screens/control/ControlScreen.tsx:934 static postgres_changes consumer",CONTROL_SCREEN_REALTIME,NOT_APPLICABLE,no,publication_membership,possible_edge_usage,KEEP_UNUSED_FOR_V1,P1,"No V1 publication change; keep existing empty application membership unused for V1 and treat realtime as a deferred enhancement",DBSURF-POSTLAUNCH-REALTIME-ENHANCEMENT,"Rollback depends on a future approved realtime design; no DB mutation authorized in V1",DEFERRED_POST_LAUNCH_ENHANCEMENT,"07b and 07c raw exports missing; V1 launch does not depend on publication membership"
legacy,inventory,public,backup_and_legacy_objects,"legacy and backup objects not fully classified",RESULT_CONFIRMED_BUT_RAW_EXPORT_MISSING,,,,,,,RESULT_CONFIRMED_ONLY,,"none proven",NO_UI_ENTRY_PROVEN,NOT_APPLICABLE,unknown,unknown,unknown,AUTH_ONLY_EXPECTED_BUT_BROADER_LIVE,P2,"Do not prune until waves 01, 05, 10d through 10j, and observation evidence are complete",DBSURF-P2-PRUNING-OBSERVATION,"Rollback and recreation definition preservation mandatory for any later removal",UNKNOWN_BLOCKED,"production object inventory incomplete"
```

## 10. Priority Findings

### 10.1 Payment and order authority remains the main P0 launch blocker

`mark_order_paid_v1(uuid)` is live in production with PUBLIC, anon, authenticated, and service-role execution, and it is still called from `joinfolk-web/lib/ticket-orders.v1.ts:56`.

`08f` does not currently observe it, but that is not removal proof. Current evidence is still insufficient to approve containment, removal, or launch sign-off.

### 10.2 Check-in has a live compatibility split

Production confirms:

- legacy `check_in_ticket(text, uuid)` is live and observed at runtime
- `checkin_ticket_v2(uuid, text)` and `checkin_ticket_by_id_v2(uuid, uuid, text)` are live and observed at runtime
- `checkin_ticket_v2_unsafe(uuid, text)` is live and service-role only

This confirms a compatibility split. The legacy path cannot be removed or narrowed until exact caller and compatibility evidence is complete.

### 10.3 Purchase and reservation families are split in production and partially observed

Production family evidence confirms:

- `purchase_event_ticket_v2` through `v5` exact signatures are live
- all three reservation exact signatures are live

Runtime confirms purchase family function names and reservation function names, but not exact overload-level attribution. In particular:

- exact runtime use of `purchase_event_ticket_v4(...jsonb)` versus `purchase_event_ticket_v4(...uuid[])` remains unresolved
- exact runtime use of five-argument versus six-argument `create_reservation_v2` remains unresolved

Canonicalization therefore remains open.

### 10.4 Search caller drift is closed as not active runtime

Current production contains `search_users_v2(text, integer)` with PUBLIC=false, anon=false, authenticated=true, and service_role=true, and it is observed at runtime.

Runtime ownership was reconciled:

- the real mobile runtime is `C:\dev\hostos\apps\mobile`, launched with `npx expo start -c`
- mobile active-source search has no RPC caller of `search_users_v1`
- mobile `lib/friends.v1.ts` calls `search_users_v2`
- mobile operator-attested manual UAT passed for global user search, correct user identity/navigation, gift-recipient search, and successful gift send
- the real dashboard runtime is `C:\dev\joinfolk-web\dashboard`
- the actual web package is `C:\dev\joinfolk-web\web`
- static search under dashboard and web found no `search_users_v1` or `search_users_v2` caller
- the `C:\dev\joinfolk-web` root has no `index.html` or Vite entry and is not the verified deployed/runtime surface
- root `lib/friends.v1.ts` and `lib/ticket-claims.v1.ts` are physically separate from the mobile copies, are not symlinks or junctions, have different SHA256 values, and are not imported by the mobile runtime
- attempted root-source patch commit `84ab737 fix(search): align user callers with v2 contract` was never pushed and was removed before push with `git reset --hard 524c642`

The earlier blocker was based on incorrect runtime ownership. No production database or source patch was required for launch-blocker closure. Historical SQL definitions remain untouched.

Non-blocking follow-up:

- `ROOT_DUPLICATE_SOURCE_PRUNING`: P1 / DEFERRED. Root app/lib copies may be stale, but they must not be deleted without a dedicated repo-topology and deployment-ownership audit. This is not an active launch blocker.

### 10.5 DM is active, and anon containment is applied and verified

Runtime confirms active DM family usage, including very high `dm_get_unread_count_v1` volume.

`10d` through `10g` are `COMPLETE_VALIDATED`, `11a` exact live body review is `COMPLETE_VALIDATED`, and the exact-signature anon EXECUTE containment migration is applied and verified in production.

Binding DM closure facts:

- migration commit: `40e804b6 fix(security): contain anonymous DM RPC execution`
- production migration version: `20260714223000`
- migration history: PASS
- production apply: PASS
- exact ACL postcondition: PASS 8/8
- anon execute: false
- authenticated execute: true
- service_role execute: true
- PUBLIC execute: false
- mobile authenticated operator-attested manual UAT: PASS
- dashboard authenticated host operator-attested manual UAT: PASS
- rollback: not used

The manual UAT checks are operator-attested manual evidence, not automated test evidence. Archive/delete wrappers with no confirmed active UI callers, direct `dm_participants` reads, wrappers that may execute before explicit session readiness, and fail-soft wrappers remain non-blocking notes.

### 10.6 Realtime is no longer a V1 product decision blocker

The V1 product and architecture decision is now binding:

- polling-first authority is final for V1
- empty `supabase_realtime` application membership is non-blocking
- realtime remains a deferred post-launch enhancement
- existing `postgres_changes` consumers are not authoritative for V1

## 11. Non-Approval Rules

This audit does not approve:

- RPC removal
- grant revokes
- grant additions
- function body changes
- search-path changes
- RLS or policy changes
- storage policy changes
- trigger changes
- cron or Edge deployment changes
- pruning, dropping, or archival mutation

## 12. Recommended Next Audit Gate

The next authorized audit actions are:

1. canonically normalize any missing raw CSVs that already exist in Downloads for `08c`, `08f`, and `09`
2. collect or normalize result-confirmed but raw-missing evidence for `07d` and `10a` through `10c`
3. preserve the closed DM authority state without reopening body authorization or caller parity
4. execute the remaining production contract exports:
   - `10h_push_contracts`
   - `10i_push_triggers`
   - `10j_push_cron_linkage_note`
5. run the polling-contract technical verification wave for V1 refresh behavior

No implementation wave should start before those remaining evidence gaps are reviewed against this audit.

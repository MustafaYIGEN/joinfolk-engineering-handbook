# V1 Live Update Polling-First Decision

## 1. Metadata

- Status: Accepted
- Version: 1.0
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-14
- canonical: true
- Scope: V1 live-update authority, polling refresh behavior, realtime launch boundary, and post-launch enhancement prerequisites

## 2. Purpose

This decision records the binding JoinFolk V1 product and architecture rules for live-update behavior across mobile, web, dashboard, DM, notifications, and event-facing surfaces.

It freezes the V1 authority model as polling-first, records the required refresh boundaries for launch, and defers realtime to a separately approved post-launch enhancement wave.

This decision does not authorize production SQL, migrations, publication changes, source-code changes, grant or revoke changes, function changes, RLS or policy changes, trigger changes, cron changes, Edge changes, or storage changes.

## 3. Binding Decision

Decision ID:

`V1_LIVE_UPDATE_POLLING_FIRST`

Binding contract:

1. JoinFolk V1 live-update authority MUST be polling-first.
2. V1 launch MUST NOT depend on Supabase Realtime publication membership.
3. Realtime is deferred to a separate post-launch enhancement wave.
4. Polling MUST remain a mandatory fallback after realtime is introduced.
5. Empty application-table membership in `supabase_realtime` is non-blocking for V1.
6. Existing `postgres_changes` consumers are non-authoritative for V1 unless separately approved and verified.
7. No production publication mutation is authorized by this decision.
8. No RLS, policy, trigger, function, grant, cron, Edge, or storage mutation is authorized by this decision.

## 4. V1 Authoritative Refresh Behavior

V1 surfaces MUST obtain fresh state through polling and explicit refresh at appropriate lifecycle boundaries.

Required technical behavior:

1. refresh on app or relevant surface startup
2. refresh on foreground or resume
3. refresh on relevant screen entry
4. refresh after successful user mutation
5. controlled polling where freshness is operationally required
6. polling loops MUST stop, pause, or reduce activity when the surface is inactive or backgrounded
7. duplicate polling timers MUST NOT be created
8. retry and backoff behavior MUST prevent request storms
9. a transient polling failure MUST NOT corrupt local state
10. manual or explicit refresh MUST remain available where appropriate

Exact polling intervals are intentionally not defined by this decision. Exact cadence belongs to implementation and verification artifacts because it may vary by surface and operational load.

## 5. Domain Implications

### DM

- DM freshness may use controlled polling for conversation lists, unread counts, and message refresh.
- Realtime delivery is not required for V1 launch.
- Message send success MUST be followed by deterministic local update or refresh.
- Polling MUST NOT create duplicate messages or duplicate unread counts.

### Notifications

- Notification freshness may use controlled polling plus explicit refresh.
- Mutation actions such as mark-read, mark-all-read, or seen-state updates MUST reconcile local state deterministically.
- Realtime notification delivery is post-launch scope.

### Events and live surfaces

- Event state may refresh on entry, foreground, mutation completion, and controlled polling where needed.
- No V1 flow may assume publication delivery is guaranteed.

### Background behavior

- Polling is not required to run continuously while the app is suspended.
- Push notifications and foreground refresh may complement polling.
- Push is not treated as a replacement for authoritative refresh.

## 6. Rationale

This is the safest V1 launch contract for the current JoinFolk production state.

Principal rationale:

1. Production evidence shows `supabase_realtime` exists but currently has no application-table membership.
2. Runtime evidence shows active DM and notification RPC usage, consistent with an existing polling-oriented system.
3. Introducing realtime before launch would add publication, RLS, subscription lifecycle, reconnect, duplicate-event, foreground, background, and fallback complexity.
4. Polling-first preserves current production behavior and minimizes launch risk.
5. Realtime remains valuable, but it must be introduced through a separate controlled post-launch wave.

This decision does not claim polling is universally superior to realtime. It defines the accepted V1 launch contract for the current JoinFolk production state.

## 7. Explicit Non-Decisions

This decision does not decide:

- exact polling intervals
- exact per-screen polling cadence
- whether every surface requires polling
- which tables will later join realtime publications
- whether realtime will use Postgres Changes, Broadcast, Presence, or a mixed design
- post-launch realtime rollout order
- long-term polling cost optimization
- any publication, policy, or source-code mutation

These remain the subject of later technical audit and patch-plan artifacts.

## 8. Evidence Basis

This decision is based on the current handbook and evidence synthesis recorded in:

- [07_Audits/DBToSurfaceContractLaunchReadinessAudit.md](../07_Audits/DBToSurfaceContractLaunchReadinessAudit.md)
- [00_Status/LaunchBlockerRegister.md](../00_Status/LaunchBlockerRegister.md)
- [00_Status/PriorityRpcDecisionRegister.md](../00_Status/PriorityRpcDecisionRegister.md)
- [10_Status/DBToSurfaceReleaseGateSummary.md](../10_Status/DBToSurfaceReleaseGateSummary.md)

The decision is also compatible with:

- [08_PatchPlans/DBToSurfacePatchWaveRegister.md](../08_PatchPlans/DBToSurfacePatchWaveRegister.md)
- [09_Decisions/NotificationDeliveryBoundaryDecision.md](./NotificationDeliveryBoundaryDecision.md)

## 9. Required Implementation Follow-Up

This decision requires compatibility with the existing implementation follow-up wave:

- `DBSURF-P1-POLLING-CONTRACT-VERIFICATION`

Required verification areas:

- app-start refresh
- foreground or resume refresh
- relevant screen-entry refresh
- post-mutation refresh
- DM unread polling
- notification polling
- background timer suspension
- duplicate-loop prevention
- retry and backoff behavior
- excessive RPC traffic detection
- deterministic local-state reconciliation

This decision does not authorize that implementation wave. Implementation requires a separately approved patch plan.

## 10. Post-Launch Realtime Wave

This decision defers realtime to:

- `DBSURF-POSTLAUNCH-REALTIME-ENHANCEMENT`

Required preconditions:

- separate owner and product approval
- publication-membership design
- RLS compatibility review
- subscription lifecycle design
- reconnect behavior
- duplicate event handling
- foreground and background behavior
- polling fallback retention
- rollback plan
- production verification plan

Status:

`DEFERRED_POST_LAUNCH`

## 11. Supersession and Conflict Rules

1. This decision supersedes any implication that V1 launch requires active application-table membership in `supabase_realtime`.
2. This decision resolves the prior `REALTIME_VS_POLLING_DECISION` item.
3. Existing audits and status records MUST interpret the realtime V1 launch effect as non-blocking.
4. No existing security, RLS, authorization, notification, push, or data-integrity decision is superseded.
5. Polling-first does not weaken authorization requirements.
6. Realtime MUST NOT be enabled later merely to compensate for broken polling, missing refresh, or incorrect local-state reconciliation.

## 12. Acceptance Criteria

This decision is complete only because it explicitly records:

- V1 live-update authority = polling-first
- realtime = post-launch enhancement
- polling fallback = required
- empty application publication membership = non-blocking
- production publication mutation = not authorized
- exact polling cadence = intentionally deferred
- implementation requires a separate patch plan
- no product decision remains open for realtime versus polling

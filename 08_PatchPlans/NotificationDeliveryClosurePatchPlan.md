# Notification Delivery Closure Patch Plan

## 1. Metadata

- Status: Draft / Controlled closure plan
- Version: 1.0
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-11
- canonical: true
- Scope: remaining notification rollout closure only

## 2. Purpose

This patch plan records the remaining controlled work after the notification security and delivery implementation completed in production.

It does not authorize any production SQL, migration execution, or deployment by itself.

## 3. Remaining Work

1. Release a mobile build containing commits `c7f7b3b` and `10a4700`.
2. Test a reminder while the application is closed.
3. Confirm the reminder fires at the local 09:00 device time derived from `reminder.date - remindBeforeDays`.
4. Confirm there is no duplicate reminder notification and no server outbox row for reminder delivery.
5. Confirm visible iOS device notification behavior.
6. Observe push failures, retries, and dead-token cleanup in the server delivery path.
7. After rollout completes, revoke authenticated EXECUTE from the legacy notification RPC.
8. Preserve service_role and owner access throughout.
9. Verify installed legacy clients no longer call the old RPC before Phase B.

## 4. Prerequisites

- Notification delivery decision accepted.
- Notification delivery audit updated.
- Mobile build containing the local reminder scheduling change is available.
- TestFlight or equivalent rollout channel exists.
- Installed-client compatibility window is understood.

## 5. Rollout Order

1. Ship the mobile build with local reminder scheduling.
2. Validate reminder device visibility in closed-app state.
3. Validate server push worker retries and dead-token cleanup.
4. Confirm there is no duplicate reminder delivery between local scheduling and server push.
5. Confirm installed clients are off the legacy notification RPC.
6. Execute legacy RPC Phase B only after rollout completion.

## 6. Rollback

- If reminder delivery fails on-device, keep the mobile build rollback path open and leave the legacy notification RPC authenticated until the replacement build is stable.
- If server push delivery regresses, keep the outbox/scheduler boundary intact and pause rollout rather than broadening client access.
- If duplicate delivery appears, preserve the reminder local owner rule and re-check the server exclusion rule before any further rollout.

## 7. Verification Categories

- Mobile device visibility.
- Local reminder closed-app delivery.
- Reminder duplication prevention.
- Server push eligibility and policy enforcement.
- Worker claim/retry behavior.
- Dead-token cleanup.
- Legacy RPC rollout compatibility.

## 8. Acceptance Criteria

This closure plan is complete only when:

- local reminders are accepted on-device in the closed-app state;
- no duplicate reminder delivery path remains;
- server push worker evidence remains stable;
- legacy clients are no longer using the old notification RPC;
- the legacy RPC Phase B revoke is executed in a controlled release step.

## 9. Prohibited Actions

- No production SQL execution.
- No deploy without owner approval.
- No secrets in the handbook.
- No unscoped notification behavior changes.
- No direct client access to the outbox.

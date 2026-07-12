# Notification Delivery Boundary Decision

## 1. Metadata

- Status: Accepted
- Version: 1.0
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-11
- canonical: true
- Scope: notification security, delivery, reminders, and legacy RPC rollout

## 2. Purpose

This decision records the binding JoinFolk rules for notification security and delivery. It separates server notification delivery from reminder local scheduling and defines the remaining rollout boundary for the legacy notification RPC.

## 3. Binding Decision

1. Normal server notifications MUST flow through `notifications_v2` -> unique outbox -> atomic worker claim -> guarded Edge Function -> Expo provider.
2. Reminder OS delivery MUST use native Expo local scheduling only.
3. `type = 'reminder'` MUST NEVER be server-pushed through the outbox.
4. Edge privileged dispatch MUST require the custom dispatch secret before service-role access.
5. Scheduler credentials MUST be stored only in Edge environment and Supabase Vault.
6. Delivery guarantee MUST be bounded at-least-once, not external exactly-once.
7. Client roles MUST NOT inspect or claim outbox rows.
8. Legacy `public.create_notification_v1(uuid,text,text,text,jsonb,text)` authenticated access is temporary and MUST be removed after the new mobile build rollout is complete.
9. Secret values MUST NEVER appear in Git, the handbook, logs, or evidence reports.

## 4. Operational Notes

- The self-targeted wrapper `public.create_my_notification_v1(text,text,text,jsonb,text)` is the approved mobile-facing notification RPC.
- `public.create_notification_v1(uuid,text,text,text,jsonb,text)` remains temporarily authenticated-executable only for compatibility during rollout.
- The server push worker and scheduler are operational boundaries, not client features.
- Reminder local delivery is implemented but remains pending device-closed verification.

## 5. Evidence Basis

This decision is based on the confirmed production state recorded in:

- [07_Audits/NotificationPushReminderContractAudit.md](../07_Audits/NotificationPushReminderContractAudit.md)
- [00_Status/ReleaseReadinessProductionHardeningGapRegister.md](../00_Status/ReleaseReadinessProductionHardeningGapRegister.md)
- [00_Status/PP01ProductionVerificationExecutionReport.md](../00_Status/PP01ProductionVerificationExecutionReport.md)

Implementation commit anchors:

- `e9a61f55`
- `c7f7b3b`
- `a412d4f8`
- `10a4700`

## 6. Acceptance Criteria

This decision remains binding while:

- server notification delivery stays behind the guarded outbox/worker/scheduler boundary;
- reminder delivery stays local-only at the OS layer;
- the legacy notification RPC remains temporarily authenticated only for rollout compatibility;
- no secret values are committed or reported.

## 7. Rollout Boundary

Phase B for the legacy notification RPC is rollout-dependent and MUST NOT be executed until installed clients have moved to the self-targeted wrapper.

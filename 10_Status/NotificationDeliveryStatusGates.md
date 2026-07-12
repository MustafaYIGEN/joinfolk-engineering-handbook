# Notification Delivery Status Gates

## 1. Metadata

- Status: Accepted
- Version: 1.0
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-11
- canonical: true

## 2. Purpose

This status gate record captures the current JoinFolk notification delivery state after the completion of the server notification security and delivery work.

## 3. Status Gates

| Gate | State | Notes |
|---|---|---|
| SERVER_NOTIFICATION_RPC_BOUNDARY | PASS | Self-targeted wrapper is the approved mobile RPC. Legacy RPC remains temporary for installed-client compatibility. |
| SERVER_PUSH_OUTBOX_SECURITY | PASS | Outbox is RLS-enabled, service-role constrained, and has bounded retry / claim fencing. |
| SERVER_PUSH_SCHEDULER | PASS | pg_cron + pg_net + Vault-backed scheduler is in place and cron calls only the internal helper. |
| SERVER_PUSH_AUTHORIZATION | PASS | push-dispatch requires the custom secret before service-role access. |
| SERVER_PUSH_POLICY_ENFORCEMENT | PASS | Reminder notifications are excluded from server push; delivery respects the guarded policy path. |
| SERVER_PUSH_PROVIDER_DISPATCH | PASS | Expo provider dispatch is guarded, token cleanup is recorded, and aggregate-only responses are preserved. |
| SERVER_PUSH_DEVICE_VISIBILITY | DEVICE_UAT_REQUIRED | Server push is proven, but closed-app visible device receipt still needs explicit device evidence if not already recorded. |
| LOCAL_REMINDER_IMPLEMENTATION | IMPLEMENTED_NOT_RELEASED | Mobile local reminder scheduling exists, but it is not yet accepted as released device evidence. |
| LOCAL_REMINDER_DEVICE_DELIVERY | DEVICE_UAT_REQUIRED | Closed-app reminder delivery must be confirmed on device. |
| LEGACY_NOTIFICATION_RPC_PHASE_B | ROLLOUT_DEPENDENT | Authenticated access to the legacy RPC remains temporary until rollout completion. |
| NOTIFICATION_DOMAIN_OVERALL | CONDITIONAL_PASS / NOT_FULLY_CLOSED | Server delivery closure is proven; reminder device UAT and legacy RPC Phase B remain open. |

## 4. Evidence Reference

- [07_Audits/NotificationPushReminderContractAudit.md](../07_Audits/NotificationPushReminderContractAudit.md)
- [09_Decisions/NotificationDeliveryBoundaryDecision.md](../09_Decisions/NotificationDeliveryBoundaryDecision.md)
- [08_PatchPlans/NotificationDeliveryClosurePatchPlan.md](../08_PatchPlans/NotificationDeliveryClosurePatchPlan.md)

## 5. Remaining Open Gates

- Local reminder TestFlight/device acceptance.
- Legacy notification RPC Phase B revocation after rollout completion.
- Visible device receipt if not separately proven in the repository.

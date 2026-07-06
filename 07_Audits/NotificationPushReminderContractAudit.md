# Notification / Push / Reminder Contract Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false

## 2. Purpose

This audit maps JoinFolk notification, push, and reminder surfaces across in-app notification records, unread counts, push token registration, push eligibility, local Edge Function source, reminder storage/listing, and notification side effects from lifecycle, commerce, claim, transfer, reservation, and public/share flows.

This is not a patch plan, cleanup plan, migration plan, or implementation authorization. It does not authorize backend/RPC/RLS/storage/auth changes. Frontend unread badges and local notification UI are product evidence only; notification ownership, push token ownership, push delivery eligibility, and reminder ownership must be backend-authoritative where security, privacy, revenue, or operational behavior is involved.

## 3. Audit Scope

Read-only inspection covered handbook audit/architecture/decision documents and targeted source searches under:

- `C:\dev\joinfolk-engineering-handbook`
- `C:\dev\hostos`
- `C:\dev\joinfolk-web`
- `C:\dev\hostos\apps\mobile`

Current system context preserved from prior handbook evidence:

- Future accepted Supabase migration working target: `C:\dev\hostos\supabase\migrations`.
- This is not proof of historical sole canonical source.
- Split-source migration history remains unresolved.
- Production SQL/RPC evidence remains stronger than local source assumptions.
- Database Functions / RPC evidence is separate from Edge Function deployment evidence.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Local Edge Function source folders exist in some Supabase trees, but deployment status is not confirmed.
- No backend patch or migration is authorized by this audit.

## 4. Notification / Push / Reminder Contract Summary

Observed notification architecture is split into several surfaces:

- Mobile V2 notifications use typed RPCs for reads, counts, mark-read, mark-all-read, and seen-state.
- Dashboard host notifications use host-specific RPCs for unread count, list, and mark-all-read.
- Older local-only notification centers exist in app/web code and store per-user notification items in AsyncStorage.
- Push token registration is RPC-mediated, but local code has two push registration paths: one active Expo foundation path and one blocked/minimal boundary path.
- Push delivery exists as local Edge Function source (`push-dispatch`, `send-test-push`), but prior manual Dashboard evidence did not show deployed Edge Functions.
- Notification settings are RPC-mediated and include event lifecycle, reservations, social, reminders, marketing, and private-content preview toggles.
- Personal reminders are server-RPC backed with AsyncStorage cache/fallback and older local-only migration behavior.
- Transfer review/invite email Edge Function source exists under the mobile Supabase tree, but deployment status is Unknown / Needs verification.

Clean contract expectation:

- In-app notification records are owner-scoped and read/mark-read authority is backend/RPC/RLS controlled.
- Push tokens are owner-scoped to authenticated user/device and treated as privacy-sensitive.
- Push delivery respects notification settings, visibility, mutes, throttles, and entitlement unless an accepted product exception exists.
- Reminder listing and mutation are owner-scoped; reminder dispatch is separate from reminder storage.
- Edge Function source existence is not deployment proof.

## 5. Notification Surface Inventory Matrix

| Surface / domain | User-visible effect | Access path observed | Expected authority owner | Production evidence status | Risk class | Recommendation |
|---|---|---|---|---|---|---|
| Mobile V2 notification center | Notification list, unread counts, seen/read state | RPC-mediated read/mutation | Backend/RPC/RLS/auth | `notifications_v2` RLS confirmed; full policy correctness not reviewed | Privacy-sensitive / Product correctness | Preserve and document V2 contract |
| Dashboard host notification bell | Host unread count and dropdown notifications | RPC-mediated read/mutation | Backend/RPC/RLS/auth | Host RPC production status not fully verified | Operational/admin-sensitive | Verify host notification RPC authority |
| Local-only notification center | Local AsyncStorage messages after client actions | UI/local storage | Mobile/Web UI only | Not production backend evidence | UX-only / Product correctness | Reconcile with V2 or label as local UX |
| Push token registration | Stores Expo device token | RPC-mediated mutation | Backend/RPC/auth | `push_tokens_v1` RLS confirmed | Privacy-sensitive | Document token ownership and stale-token lifecycle |
| Push settings | User toggles notification categories and private preview | RPC-mediated read/mutation | Backend/RPC/RLS/auth | `user_notification_settings_v1` RLS confirmed | Privacy-sensitive / Product correctness | Verify delivery consumption |
| Push eligibility | Settings, visibility, throttle, mute checks | Database Function / RPC from local source | Backend/RPC/service-role caller | Local migration evidence; production function body not manually reviewed here | Privacy-sensitive / Security-sensitive | Verify production grant/body parity |
| Push dispatch | Sends Expo push payloads | Local Edge Function source | Edge Function deployment + backend RPC | Not visible as deployed in Dashboard | Security-sensitive / Privacy-sensitive | Verify deployment before treating active |
| Send test push | Developer smoke push | Local Edge Function source | Edge Function deployment + auth | Not visible as deployed in Dashboard | Operational-sensitive | Verify deployment and auth if used |
| Personal reminders | User reminder list/upsert/delete | RPC with local cache/fallback | Backend/RPC/RLS/auth | Reminder table production RLS not covered by supplied production report; local migration evidence exists | Privacy-sensitive / Product correctness | Verify reminder RLS and function hardening |
| Transfer invite/review emails | Host transfer email side effects | Local Edge Function source | Edge Function deployment + webhook secret | Not visible as deployed in Dashboard | Operational/admin-sensitive | Verify deployment before relying on source |
| Public/share triggers | Claim/share routes may precede notifications | Mostly UI handoff; backend mutation elsewhere | Backend/RPC for side effects | Public web does not directly mutate notifications in observed route | Revenue-sensitive / Privacy-sensitive | Keep public routes side-effect free |

## 6. In-App Notification Contract Assessment

V2 notification evidence:

- `C:\dev\hostos\apps\mobile\lib\notifications\notifications.api.ts` uses RPCs for `get_my_notifications_v2`, `get_my_notification_counts_v2`, `mark_notification_read_v2`, `mark_all_notifications_read_v2`, and `mark_notifications_seen_v2`.
- `notifications.types.ts` defines notification rows with type, actor, target persona, event, host, conversation, media, grouping, deep link, read, seen, and created fields.
- Count helpers derive app badge and persona-specific unread counts from V2 count RPC output.

Legacy/local evidence:

- `notifications.local.ts` exists in both mobile and joinfolk-web code paths.
- It stores notification items in AsyncStorage under a per-user local key.
- It is explicitly described as local-only with no push and no database table.
- Local notifications are emitted after some client actions such as ticket purchase, gift claim, gift send, and checklist changes.

Interpretation:

- V2 appears to be the backend-authoritative in-app notification model.
- Local-only notifications are useful UX affordances but are not authority evidence and can drift from backend truth.
- `notifications_v1` exists in migration history as a prior generation; active production use is Unknown / Needs verification.

## 7. Push Token Ownership Assessment

Observed token registration:

- `C:\dev\hostos\apps\mobile\lib\notifications\push-token.ts` requests Expo notification permission, obtains an Expo token, and calls `register_push_token_v1`.
- `C:\dev\hostos\apps\mobile\lib\push.ts` is a separate minimal push boundary that currently halts token retrieval and calls `register_push_token_v1` only if a token exists.
- Token registration comments state it should run only after authentication.

Production evidence:

- `push_tokens_v1` RLS was confirmed enabled in prior production evidence.
- Prior focused evidence states authenticated-role policies exist for `push_tokens_v1`.

Interpretation:

- Push tokens are privacy-sensitive because they bind users/devices to external delivery.
- Registration and deactivation should remain RPC-mediated and owner-scoped.
- Token uniqueness, stale-token cleanup, multi-device semantics, and deactivation behavior need a canonical contract.

## 8. Push Eligibility and Delivery Assessment

Observed eligibility source:

- Local migration/source evidence references `evaluate_push_eligibility_v1` and `log_push_sent_v1`.
- Local `push-dispatch` source calls `evaluate_push_eligibility_v1`, reads notification settings, reads active push tokens, masks private payloads when private preview is disabled, sends via Expo, marks dead tokens inactive, and logs throttle entries for selected types.

Deployment evidence:

- Manual Supabase Dashboard evidence showed no deployed Edge Functions visible.
- Therefore `push-dispatch` and `send-test-push` must be treated as local source or future deployment candidates unless deployment evidence changes.

Interpretation:

- Push delivery should be considered Unknown / Needs verification in production.
- The local source suggests a good intended boundary: eligibility, settings, privacy masking, token lookup, provider dispatch, stale-token cleanup, and throttle logging.
- Because `push-dispatch` uses service-role behavior in local source, deployment auth/signature/caller controls remain critical if it is ever deployed.

## 9. Reminder Contract Assessment

Observed reminder behavior:

- `C:\dev\hostos\apps\mobile\lib\reminders.ts` describes personal reminders as personal-only and never visible to other users.
- The implementation uses Supabase RPCs for `upsert_personal_reminder`, `list_personal_reminders`, and `delete_personal_reminder`.
- AsyncStorage is used as local cache and fallback.
- A one-time migration path pushes local-only reminders to server.
- The file also tracks locally emitted reminder notification ids to prevent duplicate local emissions.

Prior production/security evidence:

- Reminder RPCs such as `delete_personal_reminder`, `list_active_reminders`, `list_personal_reminders`, and `upsert_personal_reminder` appeared in the production SECURITY DEFINER missing `search_path` candidate set.
- Reminder table production RLS status was not included in the supplied production evidence set.

Interpretation:

- Reminder storage/listing is separate from notification or push dispatch.
- Reminder records are privacy-sensitive because titles, notes, dates, and timing can reveal personal events.
- Reminder function hardening is Candidate P1 from prior SECURITY DEFINER evidence, but exploitability is not assumed.

## 10. Notification Settings Assessment

Observed settings behavior:

- Mobile settings calls `get_notification_settings_v1` and `upsert_notification_settings_v1`.
- Settings include event lifecycle, reservations, social, reminders, marketing, and private-content preview toggles.
- Local settings code includes a note that server-side push delivery consumption of preferences may not be fully wired in at that source point.

Production evidence:

- `user_notification_settings_v1` RLS was confirmed enabled in prior production evidence.
- Prior focused evidence states authenticated-role policies exist.

Interpretation:

- Settings must be owner-scoped.
- Push delivery must respect settings unless product explicitly defines exceptions.
- The gap is not the presence of settings; it is whether every delivery path consumes settings consistently.

## 11. Lifecycle Trigger Assessment

Expected lifecycle notification triggers:

| Lifecycle action/state | Expected notification/reminder behavior | Observed evidence | Status |
|---|---|---|---|
| draft | No public/user notification unless explicit collaborator flow exists | No complete trigger evidence | Unknown / Needs verification |
| publish | Invite/follower notification evidence exists in local migration history | `publish_event_with_groups_and_snapshot_v2` and publish notification migrations referenced in prior reports | Needs production parity review |
| live / go-live | Possible lifecycle notification and check-in readiness | Local/handbook linkage only | Unknown / Needs verification |
| ended | Possible memory/highlight/reminder notification | Local/handbook linkage only | Unknown / Needs verification |
| archived | Usually no broad notification unless explicit | No complete trigger evidence | Unknown / Needs verification |
| cancelled/canceled | Should notify affected participants/ticket holders if product requires | Some event control/security evidence exists; notification side effects incomplete | Unknown / Needs verification |
| deleted | Should not expose unauthorized detail; notification policy unknown | No complete trigger evidence | Unknown / Needs verification |
| reminder before event | Should be owner/recipient scoped and settings-aware | Reminder functions and settings exist; dispatch path unclear | Unknown / Needs verification |

Interpretation:

- Lifecycle notifications must be triggered by backend-authoritative lifecycle mutations, not frontend-only status changes.
- UI unread counts and local notifications may mirror lifecycle events but cannot define delivery authority.

## 12. Commerce / Claim / Transfer / Reservation Trigger Assessment

Observed or expected trigger areas:

- Ticket purchase/order paid: local-only notifications appear after some client purchase success flows; backend notification side effects are Unknown / Needs verification.
- Reservation created/approved/rejected/cancelled: notification categories/settings exist; exact backend trigger coverage is Unknown / Needs verification.
- Gift claim created/sent/accepted/revoked: local-only notifications exist in app/web claim/gift flows; authoritative backend notification side effects need verification.
- Ticket transfer: transfer RPC and host transfer email functions exist in local source; production deployment/status is Unknown.
- Host identity transfer invite/review: `notify-transfer-invite` and `notify-transfer-review` Edge Function source exists under the mobile Supabase tree with webhook secret validation and idempotency comments, but deployment is not confirmed.
- Venue reservation decision: dashboard and backend reservation surfaces exist; notification side effects are Unknown / Needs verification.

Interpretation:

- Commerce, claim, transfer, and reservation notifications are revenue-sensitive because they can affect trust in payment, ownership, and acceptance state.
- Notification side effects should originate from the authoritative mutation path, not from client success screens alone.

## 13. Public Web / Share Trigger Assessment

Observed public/share evidence:

- Public web `/e/:id`, `/claim/:token`, and `/v/:token` routes do not directly mutate notification, push, or reminder state in the inspected local web source.
- Claim and transfer mutations happen through app/RPC handoff paths, not public web page mutation.

Expected contract:

- Public/share routes may preview or hand off, but should not directly create notification, push, or reminder side effects unless explicitly backend-authorized.
- Claim/transfer notifications should be tied to backend claim/transfer mutations.
- Public proof verification should not create or mutate notifications.

## 14. Mobile Notification UX / Permission Map

Observed mobile UX surfaces:

- Notification center and counts use V2 RPC wrappers.
- App icon badge uses V2 count output.
- Push tap handler routes from push payload data into app screens.
- Push receive handler logs diagnostics for foreground receipt.
- Settings screen controls notification preferences through RPC.
- Push token registration asks permission and registers the Expo token through RPC.
- Reminder screens use server RPCs with local cache/fallback.

Interpretation:

- Mobile is the primary user-visible notification surface.
- Badge/unread UI is acceptable as a mirror of backend counts.
- Push tap routing is UI behavior and must not grant access beyond normal route/backend authorization.

## 15. Dashboard / Ops Notification Surface Map

Observed dashboard evidence:

- `C:\dev\hostos\apps\dashboard\app\dashboard\layout.tsx` has a notification bell with unread count polling.
- It calls `get_host_unread_count_v1`, `get_host_notifications_v1`, and `mark_all_notifications_read_v1`.
- Notifications route users to event or venue reservation dashboard pages based on entity type.

Interpretation:

- Dashboard host notifications appear RPC-mediated.
- Host/ops notification visibility is operational/admin-sensitive and should be scoped to host authority.
- Production grants/body checks for these host notification RPCs were not fully verified in supplied reports.

## 16. Backend RPC / RLS Authority Evidence Map

Known production evidence from prior handbook reports:

- `notifications_v2` RLS was confirmed enabled.
- `push_tokens_v1` RLS was confirmed enabled.
- `user_notification_settings_v1` RLS was confirmed enabled.
- `notifications_v1` production RLS status was not covered by supplied evidence.
- Reminder table production RLS status was not covered by supplied evidence.
- Reminder RPCs such as `list_active_reminders`, `list_personal_reminders`, `upsert_personal_reminder`, and `delete_personal_reminder` had prior SECURITY DEFINER `search_path`/`proconfig` concerns in production evidence.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Local Edge Function folders exist but are not production deployment proof.
- Production SQL/RPC evidence remains stronger than local source assumptions.

Unreviewed tables/functions must not be treated as safe solely because RLS is enabled or a local RPC wrapper exists.

## 17. Edge Function Deployment Evidence Map

| Edge Function / source folder | Local source path if observed | Observed role | Deployment evidence | Risk if assumed active | Recommendation |
|---|---|---|---|---|---|
| `push-dispatch` | `C:\dev\hostos\supabase\functions\push-dispatch` | Push delivery with eligibility/settings/token/privacy masking/source service-role behavior | Not visible as deployed in Dashboard | Could overstate production push delivery or ignore deployment auth boundary | Verify deployment and inbound auth/signature before treating active |
| `send-test-push` | `C:\dev\hostos\supabase\functions\send-test-push` | Authenticated smoke-test push to caller token | Not visible as deployed in Dashboard | Could expose operational push if deployed with weak auth | Verify deployment and auth if used |
| `transactional-email` | `C:\dev\hostos\supabase\functions\transactional-email` | Local transactional email source candidate | Not visible as deployed in Dashboard | Could conflate local email source with production delivery | Verify deployment before relying on it |
| `snapshot` | `C:\dev\hostos\supabase\functions\snapshot` | Local function source candidate; role not fully assessed here | Not visible as deployed in Dashboard | Could be mistaken for production-active | Classify before use |
| `notify-transfer-invite` | `C:\dev\hostos\apps\mobile\supabase\functions\notify-transfer-invite` | Host transfer recipient invite email via webhook source | Not visible as deployed in Dashboard | Could overstate host transfer email production state | Verify deployment/source path |
| `notify-transfer-review` | `C:\dev\hostos\apps\mobile\supabase\functions\notify-transfer-review` | Host transfer internal review email via webhook source | Not visible as deployed in Dashboard | Could overstate ops review email production state | Verify deployment/source path |

## 18. Direct Data Access / RLS Reliance Map

| Data surface | Direct access observed | RPC-mediated access observed | RLS reliance | Recommendation |
|---|---:|---:|---|---|
| `notifications_v2` | No primary direct mobile/dashboard access observed in focused files | Yes, V2 notification API | RLS confirmed enabled; policy correctness incomplete | Preserve RPC boundary |
| `push_tokens_v1` | Edge Function source reads/updates with service role; client uses RPC | Yes, registration via RPC | RLS confirmed enabled; Edge deployment unknown | Verify service-role deployment boundary |
| `user_notification_settings_v1` | Edge Function source reads with service role; client uses RPC | Yes, settings get/upsert RPC | RLS confirmed enabled | Verify all delivery paths consume settings |
| `personal_reminders` | No direct app table access observed in focused file | Yes, reminder RPCs | Production RLS not covered by supplied evidence | Verify reminder table/policy/function posture |
| Local notification cache | AsyncStorage local read/write | No backend | Not applicable | Treat as UX mirror only |
| Host notifications | No direct dashboard table access observed in focused file | Yes, host notification RPCs | Production details incomplete | Verify host RPC authority |

## 19. Duplicated / Split / Legacy Notification Surfaces

| Surface / helper / RPC / table | Observed role | Current / legacy / unknown | Risk if still active or authoritative | Evidence type | Recommendation |
|---|---|---|---|---|---|
| `notifications.local.ts` | Local-only notification center | Legacy / UX-only candidate | Can drift from backend V2 truth | Local source | Reconcile or label as local UX |
| V2 notification RPCs | Backend notification list/count/read authority | Current likely | Needs production policy/body verification | Local source + production RLS evidence | Preserve and document contract |
| `notifications_v1` / `create_notification_v1` | Older notification generation in migrations | Unknown | Duplicate creation semantics if still active | Local migration evidence | Determine active status |
| Dashboard host notification RPCs | Host unread/list/read UI | Current likely | Host authority unclear without body/grant review | Local source | Verify host scoping |
| Two mobile push token paths | Expo foundation path and blocked minimal boundary | Split | Registration semantics can drift | Local source | Choose canonical push registration contract |
| `push-dispatch` local source | Intended external push delivery | Dormant/future deployment candidate | Service-role function risk if deployed without controls | Local source + manual Dashboard evidence | Verify deployment before treating active |
| Transfer email Edge Functions | Host identity transfer email side effects | Unknown | Split-source deployment ambiguity | Local source | Verify deployment/source path |
| Personal reminder RPCs | Reminder storage/list/mutation | Current likely | Missing search_path hardening candidate; production RLS incomplete | Prior production evidence + local source | Verify and harden later if accepted |

## 20. Notification-Critical Invariants

- Users can only read their own notification records unless a host/staff/ops exception is explicitly defined.
- Users can only mark their own notifications read/unread.
- Push tokens are owner-scoped to the authenticated user/device.
- Push delivery respects notification settings and eligibility unless product explicitly defines exceptions.
- Public web/share routes cannot create notification, push, or reminder side effects directly.
- Claim/transfer/reservation/lifecycle notifications are triggered by backend-authoritative mutations, not frontend-only events.
- Reminder listing and dispatch are owner-scoped and time-scoped.
- Edge Function source existence is not deployment proof.
- Notification payloads do not expose private event/group/claim/profile data to unauthorized recipients.
- Notification systems do not bypass event visibility, entitlement, claim ownership, or transfer authority.

## 21. Unknown / Needs Verification Surfaces

- Whether `notifications_v1` remains active in production.
- Production body/grant parity for all V2 notification RPCs.
- Production body/grant parity for dashboard host notification RPCs.
- Whether push delivery is deployed at all.
- If push delivery is deployed later, what inbound auth/signature controls apply.
- Whether all push paths respect `user_notification_settings_v1`.
- Whether `evaluate_push_eligibility_v1` and `log_push_sent_v1` are production-active and service-role-only.
- Production RLS/policy status for `personal_reminders`.
- Whether local-only notification centers remain product-intended or legacy.
- Whether transfer invite/review Edge Functions are deployed, stale, or future-only.
- Whether lifecycle, commerce, claim, transfer, reservation, media, and social triggers are centralized in backend mutations.

## 22. Notification / Push / Reminder Gaps / Risk Register Seeds

### NPR-GAP-001

- Gap ID: NPR-GAP-001
- Domain: Push delivery
- Current issue: Local `push-dispatch` source exists with service-role push delivery behavior, but manual Dashboard evidence did not show deployed Edge Functions.
- Expected clean notification/push/reminder contract: Push delivery deployment state, inbound auth/signature controls, eligibility, settings, and privacy masking are documented separately from local source.
- Risk: Security/privacy risk if future deployment occurs without controls; product correctness risk if push is assumed active when it is not.
- Priority candidate: Candidate P1 if deployed without auth/signature controls; otherwise Unknown.
- Blocked by: Edge Function deployment verification.
- Recommended next action: Verify deployment status before any push patch planning.

### NPR-GAP-002

- Gap ID: NPR-GAP-002
- Domain: Notification generations
- Current issue: V2 notification RPCs and older local/v1 notification surfaces both exist.
- Expected clean notification/push/reminder contract: One canonical in-app notification model is documented, with legacy/local-only behavior explicitly scoped.
- Risk: Product correctness and privacy drift between local UI notifications and backend notification truth.
- Priority candidate: Candidate P2
- Blocked by: Active-use review for `notifications_v1` and local notification centers.
- Recommended next action: Document current notification generation and deprecate or label local-only surfaces later if approved.

### NPR-GAP-003

- Gap ID: NPR-GAP-003
- Domain: Reminder ownership and hardening
- Current issue: Personal reminder RPCs are used by mobile and appeared in prior SECURITY DEFINER missing `search_path` evidence; production reminder table RLS was not covered by supplied evidence.
- Expected clean notification/push/reminder contract: Reminder records are owner-scoped with verified RLS and hardened function definitions.
- Risk: Privacy-sensitive reminder data exposure or mutation risk if ownership checks or function hardening are incomplete.
- Priority candidate: Candidate P1 / Needs verification
- Blocked by: Production reminder RLS/policy and function definition review.
- Recommended next action: Perform focused reminder production parity verification.

### NPR-GAP-004

- Gap ID: NPR-GAP-004
- Domain: Push settings consumption
- Current issue: Notification settings exist and local code notes delivery consumption may not be fully wired in one source path.
- Expected clean notification/push/reminder contract: Every push delivery path respects settings and private-content preview unless product exceptions are documented.
- Risk: Privacy/product correctness risk from sending unwanted or overly detailed push content.
- Priority candidate: Candidate P1 / Unknown
- Blocked by: Production delivery path verification.
- Recommended next action: Verify settings consumption in any active delivery path.

### NPR-GAP-005

- Gap ID: NPR-GAP-005
- Domain: Commerce/claim/transfer side effects
- Current issue: Some client flows emit local-only notifications after purchase, claim, or gift actions; backend notification side effects are not fully mapped.
- Expected clean notification/push/reminder contract: Revenue-sensitive side effects originate from backend-authoritative mutations.
- Risk: Users may see misleading or missing notifications around ownership/payment/claim state.
- Priority candidate: Candidate P2
- Blocked by: Commerce/claim/reservation trigger review.
- Recommended next action: Map notification creation calls from authoritative mutation RPCs.

### NPR-GAP-006

- Gap ID: NPR-GAP-006
- Domain: Dashboard host notifications
- Current issue: Dashboard uses host notification RPCs, but production authority body/grant evidence was not fully reviewed.
- Expected clean notification/push/reminder contract: Host notification reads and mark-read actions are scoped to host/ops authority.
- Risk: Operational/admin-sensitive notification visibility drift.
- Priority candidate: Candidate P2
- Blocked by: Host notification RPC production review.
- Recommended next action: Verify host notification RPC grants and internal host ownership checks.

### NPR-GAP-007

- Gap ID: NPR-GAP-007
- Domain: Transfer email Edge Functions
- Current issue: Transfer invite/review Edge Function source exists in a split mobile Supabase tree, but deployment status is unknown.
- Expected clean notification/push/reminder contract: Transfer email side effects have confirmed deployment source, webhook auth, idempotency, and ops/audit semantics.
- Risk: Operational confusion or missed transfer notifications if assumed active incorrectly.
- Priority candidate: Unknown / Needs verification
- Blocked by: Edge Function deployment and source-path confirmation.
- Recommended next action: Verify transfer email deployment separately from Database Function/RPC evidence.

## 23. Product Decisions Required

- Is V2 the only canonical in-app notification system?
- Should local-only notifications remain as UX mirrors, or should they be removed/reconciled later?
- Which notification types can include private event, group, claim, profile, or media previews?
- Which push categories are mandatory versus user-configurable?
- Is push delivery currently part of the production product, or only future infrastructure?
- Should reminders produce local-only notifications, in-app notification records, external push, or some combination?
- Which lifecycle states trigger notifications, and to which viewer roles?
- Which commerce, claim, transfer, and reservation actions trigger notifications?
- Are host transfer emails part of the production operational contract?

## 24. Recommended Next Audits

1. Media / Gallery / Memory Wall Contract Audit

   Focus on media likes, comments, highlights, event media, memory wall exposure, signed URLs, public/private media visibility, and privacy-sensitive notification payloads.

2. Profile / Persona / Public Identity Contract Audit

   Focus on avatars, host identity, organizer persona fields, public relics, tiers, profile visibility, and notification payload identity exposure.

3. Social Graph / Groups / Visibility Contract Audit

   Focus on friend, follow, group, invite, mute, block, private event visibility, notification eligibility, and push privacy masking.
## 25. Non-Goals

- No application, dashboard, mobile, web, or Supabase code changes.
- No SQL, migration, policy, storage, RPC, or Edge Function implementation.
- No production connection or Supabase CLI usage.
- No claim that notification/push/reminder access is unsafe solely because it exists.
- No claim that RLS is correct solely because it is enabled.
- No claim that Edge Functions are deployed solely because local source folders exist.
- No feature removal or cleanup recommendation.
- No release readiness conclusion.

## 26. Open Questions

- Is push delivery intentionally disabled in production right now?
- Which app path is canonical for push token registration?
- Are `evaluate_push_eligibility_v1` and `log_push_sent_v1` live and service-role-only in production?
- Does every notification delivery path respect notification settings?
- Which V1 notification functions remain callable or used?
- Are dashboard host notification RPCs scoped to host ownership and ops roles?
- Do reminders dispatch notifications or only store/list personal reminder records?
- Are transfer invite/review email functions deployed from any Supabase tree?
- Which notification payload fields are allowed for private/group/invite-only events?

## 27. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `07_Audits/NotificationPushReminderContractAudit.md` was created/modified.

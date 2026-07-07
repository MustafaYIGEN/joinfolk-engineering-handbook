# PP-06 Notification / Diagnostics Privacy Pack

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook audit synthesis only
- canonical: false
- Execution status: Not executed
- Legal status: Engineering planning only; not legal advice
- Notification status: Not executed / Delivery not verified
- Diagnostics status: Not executed / Payloads not verified

## 2. Purpose

This is a decision-pack for defining JoinFolk notification, diagnostics, push token, reminder, private preview, deep link, analytics/crash disclosure, `app_diagnostics`, support/admin visibility, retention, redaction, and privacy semantics before implementation work begins.

It is not notification implementation, not Edge implementation, not diagnostics implementation, not production verification, not legal advice, and not patch authorization.

## 3. Evidence Boundary

This document is based only on handbook audits, the Release Readiness / Production Hardening Gap Register, PP-01, PP-02, PP-03, PP-04, and PP-05.

No source-code inspection, production connection, Supabase CLI, SQL, builds, tests, push delivery verification, diagnostics payload verification, Edge deployment verification, legal review, policy-copy modification, or final copy drafting was performed.

## 4. PP-06 Scope Summary

PP-06 covers:

- Notification surface inventory.
- Diagnostics surface inventory.
- Notification preference model.
- Private preview and push payload model.
- Deep-link reauthorization model.
- Push token and device data model.
- In-app notification history and reminder model.
- Diagnostics and `app_diagnostics` model.
- Analytics/crash disclosure model.
- Support/admin diagnostics visibility model.
- Retention, redaction, deletion, and export constraints.
- Backend/RPC/RLS/Edge dependencies.
- PP-02 policy-copy dependencies.
- PP-03 deletion/data-request dependencies.
- PP-04 commerce notification dependencies.
- PP-05 public visibility dependencies.
- Dependency mapping to PP-07 through PP-10.

PP-06 does not execute PP-01 and does not authorize push delivery changes, diagnostics changes, Edge Function changes, RLS changes, RPC changes, database changes, policy copy changes, or production changes.

## 5. Source Register Coverage

| Release gap | Why PP-06 covers it | PP-06 limitation |
|---|---|---|
| RR-GAP-003 | Notification, push token, settings, diagnostics, and direct table access surfaces need RLS/policy verification. | PP-06 does not verify production policies. |
| RR-GAP-009 | Notification deep links and previews must respect public/private route suppression. | PP-05 owns public visibility contract. |
| RR-GAP-010 | Notification preferences, private previews, push payload privacy, token deletion, reminders, and Edge deployment are central PP-06 scope. | PP-06 does not verify delivery. |
| RR-GAP-011 | Diagnostics payload minimization, user linkage, support read access, retention, and redaction are central PP-06 scope. | PP-06 does not inspect payloads. |
| RR-GAP-017 | DM notifications, private previews, delete/archive, and support visibility affect messaging privacy. | PP-10 owns full messaging lifecycle. |
| RR-GAP-018 | Support/admin diagnostics visibility and notification/admin process auditability need explicit authority. | PP-08 owns support/admin authority. |
| RR-GAP-019 | Report/moderation notifications and evidence privacy depend on trust/safety workflow. | PP-07 owns moderation workflow. |
| RR-GAP-020 | Account deletion, data export, push token deletion, notification history, and diagnostics retention need privacy decisions. | PP-03 owns deletion/data-request model. |
| RR-GAP-021 | Notification/private-preview/analytics/diagnostics disclosure copy needs legal/product reconciliation. | PP-02 owns policy copy review. |
| RR-GAP-022 | Commerce notifications, receipts, provider logs, and revenue auditability need commerce contract alignment. | PP-04 owns commerce/refund/payment contract. |
| RR-GAP-023 | Product decisions must be accepted before patches. | PP-06 recommends decision records only. |

## 6. Notification / Diagnostics Privacy Problem Statement

Notification and diagnostics privacy is not one toggle. It includes:

- Push token.
- Device identifier.
- User notification settings.
- Notification history.
- Unread state.
- Reminders.
- Private preview.
- Push payload.
- Deep link.
- Public/private route re-check.
- In-app notification inbox.
- Commerce, ticket, reservation, claim, and refund notification.
- DM notification.
- Social, group, invite, block, and mute notification.
- Moderation/report notification.
- Diagnostics event.
- Crash/analytics event.
- `app_diagnostics` row.
- Support/admin diagnostics visibility.
- Retention, export, deletion, and redaction.
- Legal/privacy copy.

UI settings are not delivery authority. Push preview masking is not deep-link authorization. Deep links must re-check backend visibility and authority at open time.

## 7. Notification Surface Inventory

| Surface | Example user expectation | Data/function domains affected | Owner | PP-01 evidence dependency | PP-02 copy dependency | PP-03 deletion dependency | PP-04 commerce dependency | PP-05 visibility dependency | Current status | Recommended PP-06 decision need |
|---|---|---|---|---|---|---|---|---|---|---|
| Push notification delivery | Preferences affect device delivery. | Push dispatch, notification rows, tokens. | Notifications + backend | Delivery/Edge evidence | Notification settings copy | Token deletion | Commerce notifications | Route visibility | Unknown / Needs verification | Decide enforced vs advisory settings. |
| Push token registration | Device token is stored safely. | `push_tokens_v1`, device metadata. | Notifications + privacy | Token table/RLS | Push disclosure | Logout/account deletion | None | None | Unknown / Needs verification | Decide token ownership and retention. |
| Push token deletion | Opt-out/logout/deletion removes token. | Tokens, device records. | Notifications + privacy | Token delete policy evidence | Privacy copy | Account deletion | None | None | Unknown / Needs verification | Decide deletion/revocation model. |
| User notification settings | Settings control categories. | `user_notification_settings_v1`, delivery. | Product + notifications | Settings/delivery evidence | Settings copy | Settings deletion | Category semantics | Visibility re-checks | Unknown / Needs verification | Decide promise level per setting. |
| Private preview setting | Sensitive preview is masked. | Push payload, inbox, settings. | Product + privacy | Payload/settings evidence | Private preview copy | Redaction | Commerce content | Public/private data | Unknown / Needs verification | Decide masked fields. |
| In-app notification inbox/history | User sees notification history. | `notifications_v2`, read state. | Product | Table/RLS evidence | Notification copy | History deletion/export | Commerce refs | Suppressed links | Unknown / Needs verification | Decide retention/export/redaction. |
| Unread badge/count | Badge matches unread state. | Read/unread state, notification rows. | Product | Read state evidence | Badge copy if any | Deletion | None | None | Unknown / Needs verification | Decide scope and reset semantics. |
| Reminder notification | User gets event/ticket reminders. | Reminder jobs/functions, event data. | Product + notifications | Reminder evidence | Reminder copy | Reminder deletion | Ticket/event state | Cancelled/private events | Unknown / Needs verification | Decide stale/cancelled reminder behavior. |
| Event lifecycle notification | Status changes notify users. | Events, notifications, preferences. | Product | Lifecycle notification evidence | Event copy | Event deletion | Cancellation/refund state | Public/private event | Unknown / Needs verification | Decide mandatory vs suppressible. |
| Ticket purchase/confirmation notification | Purchase confirmation arrives. | Tickets, orders, notifications. | Commerce + notifications | Commerce notification evidence | Receipt/confirmation copy | Commerce retention | Purchase contract | Public/deep-link state | Unknown / Needs verification | Decide confirmation vs receipt. |
| Refund/cancellation/dispute notification | User is notified about commerce changes. | Orders, tickets, provider, support. | Commerce + support | Provider/notification evidence | Refund copy | Audit retention | Refund/dispute contract | Cancelled/revoked visibility | Unknown / Needs verification | Decide audit and delivery rules. |
| Reservation notification | Reservation status changes notify users. | Reservations, venue, notifications. | Product + venue | Reservation notification evidence | Reservation copy | Reservation retention | Reservation contract | Public/private event | Unknown / Needs verification | Decide category and sensitive fields. |
| Claim/gift/transfer notification | Claim/transfer recipients are notified. | Claims, tickets, users, notifications. | Commerce + product | Claim notification evidence | Claim copy | Claim retention | Transfer semantics | Token/share behavior | Unknown / Needs verification | Decide payload and deep link authority. |
| Check-in notification | Check-in status may notify holder/host. | Tickets, proof, notifications. | Staff/host + commerce | Check-in evidence | Check-in copy | Proof retention | Check-in contract | Public verification | Unknown / Needs verification | Decide if needed and what is sensitive. |
| DM notification | New private message notifies recipient. | DMs, conversations, sender identity. | Messaging + privacy | DM notification evidence | Private preview copy | DM deletion | None | Deep-link reauth | Unknown / Needs verification | Decide snippet masking and block/mute. |
| Group/invite/social notification | Invites/follows/groups notify user. | Social graph, groups, notifications. | Social/product | Social notification evidence | Social copy | Social deletion | None | Group/private visibility | Unknown / Needs verification | Decide visibility before delivery. |
| Media/comment/moderation notification if applicable | Uploader/host notified of media action. | Media, moderation, notifications. | Media + trust/safety | Media notification evidence | Media/safety copy | Evidence retention | None | Takedown visibility | Unknown / Needs verification | Decide payload minimization. |
| Report/takedown/appeal notification | Safety process updates notify parties. | Reports, moderation, support. | Trust/safety + legal | Report notification evidence | Report/appeal copy | Evidence retention | None | Suppression behavior | Unknown / Needs verification | Decide sensitive evidence boundaries. |
| Support/admin notification if applicable | Ops/support gets actionable notice. | Admin/support, diagnostics, logs. | Support + ops | Admin notification evidence | Support copy | Audit retention | Support process | Admin-only links | Unknown / Needs verification | Decide access and auditability. |
| Deep link from notification | Tap opens authorized destination. | Routes, linked resources, auth. | Product + security | Deep-link evidence | Deep-link copy | Deletion state | Commerce state | Visibility checks | Unknown / Needs verification | Decide reauthorization model. |

## 8. Diagnostics Surface Inventory

| Surface | Example user expectation | Data domains affected | Owner | PP-01 evidence dependency | PP-02 copy dependency | PP-03 retention/export dependency | Current status | Recommended PP-06 decision need |
|---|---|---|---|---|---|---|---|---|
| `app_diagnostics` | Debug data is minimal and protected. | Runtime events, user/session/device/context. | Diagnostics + privacy | Schema/payload/RLS evidence | Diagnostics disclosure | Retention/export | Unknown / Needs verification | Classify linkage and allowed payload. |
| Crash reports | Crashes may be collected. | Stack/error/device/app metadata. | Diagnostics | Crash tooling evidence | Crash disclosure | Retention/export | Unknown / Needs verification | Decide anonymous/pseudonymous/user-linked status. |
| Analytics events | Usage analytics may be collected. | Event names, user/session/device. | Product + privacy | Analytics evidence | Analytics copy | Retention/export | Unknown / Needs verification | Decide disclosure and opt-out if any. |
| Remote diagnostics | Runtime state may be submitted. | App state, errors, device metadata. | Diagnostics | Remote diagnostics evidence | Diagnostics copy | Retention/export | Partial local-source evidence in prior audits | Decide trust and payload rules. |
| Client logs | Client may log local runtime errors. | Low-trust device/client data. | Engineering | Client logging evidence | Diagnostics copy | Retention/export | Unknown / Needs verification | Treat as low-trust unless server-backed. |
| Support debug data | Support may inspect diagnostics. | User/event/ticket/device context. | Support + diagnostics | Support access evidence | Support/privacy copy | Audit retention | Unknown / Needs verification | Define access and auditability. |
| Admin/ops diagnostics views | Ops may view telemetry. | Diagnostics/log tables. | Ops + diagnostics | Admin route/function evidence | Ops/support copy | Audit retention | Unknown / Needs verification | Define least privilege. |
| Provider/push delivery logs if any | Delivery troubleshooting exists. | Provider status, token refs, payload metadata. | Notifications + support | Delivery log evidence | Push disclosure | Retention | Unknown / Needs verification | Decide payload minimization. |
| Notification delivery logs | Delivery status may be recorded. | Notification id, token/device, status. | Notifications | Log evidence | Notification copy | Retention/export | Unknown / Needs verification | Decide whether logs are product or operational data. |
| Error reports tied to user/session/event/ticket | Debugging links errors to context. | User IDs, event IDs, ticket IDs. | Diagnostics + privacy | Payload evidence | Diagnostics disclosure | Retention/export | Unknown / Needs verification | Decide redaction and support access. |
| Device metadata | Device/app/platform data may be stored. | Platform, version, device token metadata. | Privacy + notifications | Token/diagnostic evidence | Device disclosure | Token deletion | Unknown / Needs verification | Decide retention and export. |
| App version/build/platform metadata | Debugging version/platform behavior. | App version, build, OS/platform. | Diagnostics | Payload evidence | Diagnostics copy | Retention | Unknown / Needs verification | Decide public/non-sensitive classification. |

## 9. Notification State / Privacy Taxonomy

- Delivery allowed.
- Delivery suppressed.
- Category opt-in.
- Category opt-out.
- Private preview enabled.
- Private preview disabled.
- Silent/in-app only.
- Push delivered.
- Push failed.
- In-app notification only.
- Read/unread.
- Archived/deleted notification.
- Deep link authorized.
- Deep link re-auth required.
- Deep link suppressed.
- Public-safe preview.
- Sensitive preview.
- Token active.
- Token revoked/deleted.

Do not collapse settings UI with delivery enforcement.

## 10. Diagnostics / Telemetry / Analytics Taxonomy

- Anonymous: not linked to user, device, session, event, ticket, or account in a way that identifies or tracks a person.
- Pseudonymous: uses an identifier that may link events without directly displaying identity.
- User-linked: contains or joins to user/account/profile identity.
- Device-linked: tied to push token, device identifier, platform instance, or install.
- Session-linked: tied to a login/session/runtime instance.
- Event-linked: tied to event, venue, media, or route context.
- Ticket/order-linked: tied to commerce or entitlement context.
- Low-trust client telemetry: client-supplied data useful for debugging but not authoritative audit evidence.
- Crash report.
- Analytics event.
- Support diagnostic.
- Admin audit log.
- Retained diagnostic.
- Redacted diagnostic.
- Exportable diagnostic.
- Non-exportable retained log.

Do not call diagnostics anonymous unless PP-01 evidence supports it.

## 11. Notification Preference Decision Model

Decision areas:

- Global notification setting.
- Category settings.
- Event reminders.
- Commerce/ticket notifications.
- DM notifications.
- Group/social notifications.
- Private preview.
- Quiet/silent delivery if any.

Decisions needed:

- Which settings are user promises.
- Which settings are advisory UI state.
- Which settings are enforced by delivery.
- Which safety/account/commerce notifications are mandatory, if any.
- How settings interact with in-app history versus push delivery.

## 12. Private Preview / Push Payload Decision Model

Push payload fields requiring classification:

- Notification title.
- Body.
- Actor name.
- Event title.
- Message snippet.
- Ticket/order/refund status.
- Media/report/moderation content.
- Deep link metadata.

Decisions needed:

- What is allowed in push payloads.
- What is masked when private preview is disabled.
- What is never sent in push payloads.
- Whether sensitive content is in-app only.
- Whether payloads contain only identifiers plus generic text.

## 13. Deep Link / Route Re-Authorization Decision Model

Deep-link destinations:

- Event link.
- Ticket link.
- Claim link.
- Check-in link.
- DM link.
- Profile link.
- Report/moderation link.
- Support/admin link.

Decision needed: notification deep links must re-check backend visibility and authority at open time. Expired, deleted, private, refunded, revoked, blocked, muted, moderated, or inaccessible states must suppress or redirect safely.

## 14. Push Token / Device Data Decision Model

Decision areas:

- Push token.
- Device platform.
- App version.
- Device identifier if any.
- Token owner.
- Token rotation.
- Logout.
- Account deletion.
- Opt-out.

Decisions needed:

- When token is deleted, revoked, retained, or rotated.
- Whether inactive tokens are purged.
- Whether token data is exportable or redacted.
- Whether token deletion is immediate on logout/account deletion.

## 15. In-App Notification History Decision Model

Decision areas:

- Notification rows.
- Read state.
- Actor references.
- Event, ticket, message, profile, media, or report references.
- Deletion/redaction references.
- Account deletion handling.

Decision needed: whether notification history is deleted, redacted, retained, archived, or exportable; and how linked deleted/private resources display.

## 16. Reminder / Scheduled Notification Decision Model

Decision areas:

- Event reminders.
- Ticket reminders.
- Reservation reminders.
- Scheduled jobs.
- Local notification vs server push.
- Cancellation or updated event behavior.

Decision needed: stale reminder cancellation, private event reminder visibility, deleted/refunded/cancelled event behavior, and whether reminders respect preference categories.

## 17. Commerce / Ticket / Reservation Notification Decision Model

Commerce notification states:

- Purchase confirmation.
- Order paid.
- Order failed.
- Ticket issued.
- Refund requested/approved/denied.
- Event cancelled.
- Reservation created/confirmed/cancelled.
- Claim/gift/transfer created/claimed/expired.
- Check-in status.

Decisions needed:

- Which notifications are mandatory.
- Which are suppressible.
- Whether notifications are receipts/invoices or only product confirmations.
- What auditability is required for refund/dispute notifications.

## 18. Messaging / DM Notification Decision Model

Decision areas:

- DM notification.
- Message snippet.
- Sender name.
- Private preview.
- Deleted/redacted sender.
- Blocked/muted sender.
- Support/report context.

Decisions needed: snippet masking, block/mute suppression, deep-link reauthorization, deletion/redaction behavior, and whether private message bodies ever leave in-app context.

## 19. Social / Group / Invite / Block Notification Decision Model

Decision areas:

- Invite.
- Group membership.
- Follow/friend/host follower.
- Block/mute effects.
- Private/group-only events.

Decision needed: visibility checks before delivery and deep-link behavior after group removal, invite expiry, block, mute, or membership change.

## 20. Media / Moderation / Report Notification Decision Model

Decision areas:

- Media approved/hidden/taken down.
- Report received.
- Report resolved.
- Appeal/restoration.
- Uploader/host/support notifications.

Decision needed: sensitive report evidence must not leak through push payloads, and moderation/takedown notifications may need legal/support review.

## 21. Diagnostics / app_diagnostics Decision Model

Decision areas:

- Payload fields.
- User id, session id, device id, event id, ticket id linkage.
- Stack traces.
- Error messages.
- Support visibility.
- Admin visibility.
- Retention.
- Deletion, redaction, and export.

Decision needed: classify diagnostics as anonymous, pseudonymous, user-linked, device-linked, mixed, or Unknown / Needs verification; define allowed payload fields, retention, and access.

## 22. Analytics / Crash Reporting Disclosure Decision Model

Decision areas:

- Analytics events.
- Crash reports.
- Device/app metadata.
- User/session linkage.
- Third-party processors if any.
- Opt-out if any.

Decision needed: disclosure must match evidence. Do not call analytics/crash/diagnostics anonymous unless verified. Decide whether analytics/crash data is exportable, deletable, or redactable.

## 23. Support / Ops / Admin Diagnostics Visibility Decision Model

Decision areas:

- Who can read diagnostics.
- Who can search by user, event, ticket, device, session, or route.
- Support use cases.
- Admin audit logs.
- Escalation.
- Redaction.

Decision needed: access control, auditability, least privilege, retention, and whether support read access is logged.

## 24. Retention / Redaction / Deletion / Export Decision Model

Data classes:

- Push tokens.
- Notification history.
- Unread state.
- Notification settings.
- Reminders.
- Delivery logs.
- Diagnostics.
- Crash/analytics.
- Support/admin logs.

Decisions needed:

- What is deleted on logout.
- What is deleted on account deletion.
- What is exportable.
- What is retained for audit/security/support.
- What is redacted rather than deleted.

## 25. Backend / RPC / RLS / Edge / Storage Verification Dependencies

PP-06 requires PP-01 verification for:

- Notification tables.
- Push token tables.
- Settings tables.
- Reminder tables/jobs/functions if present.
- Edge Functions for push dispatch if deployed.
- RPCs/functions that create, send, read, update, or mark notifications.
- RLS policies for notification, token, settings, reminder, and diagnostics tables.
- Grants, `search_path`, and `SECURITY DEFINER` posture where relevant.
- `app_diagnostics` table and policies.
- Support/admin diagnostics access.
- Public/deep-link route authority checks.
- Notification delivery logs if present.

No SQL is included in this pack.

## 26. Notification / Diagnostics Data Domain Inventory Matrix

| Domain | Example data | User expectation | Privacy decision needed | Legal/product review need | PP-01 evidence need | PP-03/PP-05 dependency | Later pack dependency |
|---|---|---|---|---|---|---|---|
| `notifications_v2` | Notification rows, actor/resource refs | History is private and scoped | Retention, export, redaction | High | RLS/table evidence | Deletion + deep-link visibility | PP-08/PP-10 |
| `push_tokens_v1` | Device token, platform | Token removed on opt-out/deletion | Delete/revoke/retain model | High | Token RLS evidence | Account deletion | PP-08 |
| `user_notification_settings_v1` | Category/private preview settings | Settings are enforced | Promise vs enforcement | High | Settings/delivery evidence | Settings deletion | PP-06 |
| Reminder jobs/functions if present | Scheduled event/ticket reminders | Stale reminders do not leak data | Cancellation/suppression | Medium/High | Job/function evidence | Event visibility | PP-05 |
| In-app notification history | Notification list/read state | History is private | Delete/archive/export | Medium/High | Table/read evidence | Account deletion | PP-06 |
| Unread/read state | Badge counters | Counts are user-scoped | Scope/reset semantics | Medium | Read state evidence | Deletion | PP-06 |
| Notification deep links | Resource links | Links reauthorize | Re-check model | High | Route/RPC evidence | Public/private visibility | PP-05 |
| DM notification payloads | Sender/snippet/thread link | Private content masked | Snippet/private preview | High | DM notification evidence | DM deletion + visibility | PP-10 |
| Commerce notification payloads | Ticket/refund/order status | Accurate but private | Receipt/confirmation scope | High | Commerce notification evidence | Commerce contract | PP-04 |
| Group/invite notification payloads | Invite/group/event refs | Private groups protected | Membership check | High | Social/group evidence | Group visibility | PP-07 |
| Report/moderation notification payloads | Report/takedown status | Evidence not leaked | Payload minimization | High | Report notification evidence | Takedown visibility | PP-07 |
| `app_diagnostics` | Runtime state/errors | Minimal protected diagnostics | Linkage/payload/retention | High | Schema/payload/RLS evidence | Deletion/export | PP-08 |
| Crash/analytics events | Crash, usage, device/app metadata | Disclosure matches behavior | Anonymous vs linked | High | Tooling/payload evidence | Retention/export | PP-02 |
| Client logs | Low-trust local logs | Not authoritative | Retain/minimize | Medium | Client/server evidence | Deletion | PP-06 |
| Support/admin diagnostics views | Search/debug tools | Access is gated/audited | Least privilege/audit | High | Admin/support evidence | Audit retention | PP-08 |
| Delivery logs/provider logs if present | Push status/token refs | Troubleshooting without leakage | Payload minimization/retention | High | Delivery log evidence | Token deletion | PP-08 |

## 27. Policy-to-Privacy Mismatch Register

| Copy/policy signal | Missing privacy contract decision | Risk | Owner | Recommended next action |
|---|---|---|---|---|
| Notification preferences vs delivery enforcement unknown | Which settings are enforced by delivery. | Privacy-sensitive | Product + notifications | Verify and decide promise level. |
| Private preview vs push payload unknown | Masked fields and forbidden payload data. | Privacy-sensitive | Product + privacy | Define payload classification. |
| Anonymous analytics/crash claim vs possible user-linked diagnostics | Diagnostics classification. | Privacy-sensitive | Legal/privacy + diagnostics | Verify linkage before final disclosure. |
| Diagnostics retention/export/deletion unknown | Retention and data request behavior. | Privacy-sensitive | Product + legal | Align with PP-03. |
| Support/admin diagnostics visibility unknown | Access/audit model. | Operational/admin-sensitive | Ops + support | Define least privilege and auditability. |
| Push token deletion on logout/account deletion unknown | Token lifecycle. | Privacy-sensitive | Notifications + privacy | Decide deletion/revocation model. |
| Deep links vs public/private visibility unknown | Reauthorization behavior. | Privacy-sensitive | Product + security | Link to PP-05 public visibility contract. |
| Commerce notification receipt/invoice ambiguity | Confirmation vs receipt language. | Revenue-sensitive | Commerce + legal | Link to PP-04 decision. |
| DM notification snippet privacy unknown | Snippet masking and block/mute. | Privacy-sensitive | Messaging + privacy | Link to PP-10. |
| Report/moderation notification privacy unknown | Evidence and status payload. | Trust/safety-sensitive | Trust/safety + legal | Link to PP-07. |

## 28. Implementation-without-Privacy-Contract Register

| Existing technical/product surface | Missing notification/diagnostics privacy contract | Risk | Owner | Recommended next action |
|---|---|---|---|---|
| Push token table | Token deletion, owner scope, device metadata retention. | Privacy-sensitive | Notifications | Verify and decide lifecycle. |
| Notification settings table | Whether delivery consumes settings. | Privacy-sensitive | Product + notifications | Verify enforcement. |
| Notification history table | Retention/export/redaction model. | Privacy-sensitive | Product | Define history contract. |
| Push dispatch source/deployment uncertainty | Active delivery path and Edge deployment. | Privacy-sensitive | Backend + notifications | Verify via PP-01. |
| `app_diagnostics` | Payload allowlist, linkage, retention. | Privacy-sensitive | Diagnostics | Define diagnostics classification. |
| Unread count/badge behavior | User-scope and read-state semantics. | Product correctness | Product | Define count model. |
| Reminder scheduling | Stale/private/cancelled reminder behavior. | Privacy-sensitive | Product | Define reminder lifecycle. |
| Commerce/ticket notifications | Receipt/confirmation and refund semantics. | Revenue-sensitive | Commerce | Link to PP-04. |
| DM notifications | Snippet/private preview/block/mute. | Privacy-sensitive | Messaging | Link to PP-10. |
| Group/social notifications | Group/private invite visibility. | Privacy-sensitive | Social | Link to PP-07. |
| Moderation/report notifications | Evidence/status payload. | Trust/safety-sensitive | Trust/safety | Link to PP-07. |
| Support/admin diagnostics views | Access, audit, support scope. | Operational/admin-sensitive | Ops + support | Link to PP-08. |

## 29. PP-01 Evidence Dependencies

PP-06 needs PP-01 evidence for:

- Production notification tables and RLS.
- Push token table and RLS.
- Settings table and delivery usage.
- Push dispatch Edge deployment state.
- Notification creation functions/RPCs.
- Delivery logs/provider logs.
- `app_diagnostics` schema, payloads, and RLS.
- Diagnostics read/write authority.
- Support/admin diagnostics access.
- Public/private deep-link backend checks.

## 30. PP-02 Policy Copy Dependencies

PP-06 must respect PP-02 constraints:

- Notification settings/private preview copy must match enforcement.
- Analytics/crash/diagnostics disclosure must match payload and linkage.
- Push token/device data disclosure must be accurate.
- Support/privacy copy must not overpromise deletion or export.
- No final privacy/legal copy should be treated as approved until owner approval.

## 31. PP-03 Deletion / Data Request Dependencies

PP-06 must respect PP-03 constraints:

- Push tokens may need deletion on logout or account deletion.
- Notification history needs deletion, redaction, retention, and export decisions.
- Diagnostics need retention, redaction, export, and access decisions.
- Support/admin logs may require retention.
- Account deletion must not leave public or private notification leaks.

## 32. PP-04 Commerce / Refund / Payment Dependencies

PP-06 must respect PP-04 constraints:

- Purchase, refund, cancellation, reservation, claim, transfer, and check-in notifications must match the accepted commerce contract.
- Notifications must not be treated as receipts/invoices unless accepted.
- Refund/dispute notifications may require auditability.
- Commerce deep links must re-check entitlement/payment state.

## 33. PP-05 Public Visibility Dependencies

PP-06 must respect PP-05 constraints:

- Notification previews must not leak private or public-suppressed data.
- Deep links must re-check route visibility.
- Deleted, private, group-only, cancelled, moderated, refunded, revoked, expired, transferred, or claimed states must suppress or redirect safely.
- OpenGraph, preview, and caching behavior, if applicable, needs review.

## 34. Product Decision Dependency Checklist

- Notification category model.
- Mandatory vs suppressible notifications.
- Private preview model.
- Push payload allowed fields.
- Deep-link reauthorization model.
- Token deletion/retention model.
- Notification history retention/export model.
- Diagnostics classification model.
- `app_diagnostics` payload allowlist.
- Support/admin diagnostics access.
- Diagnostics retention/redaction model.
- Beta vs public launch scope.

## 35. Legal / Privacy Review Dependency Checklist

- Push notification disclosure.
- Private preview wording.
- Analytics/crash disclosure.
- Diagnostics/`app_diagnostics` disclosure.
- Push token/device data retention.
- Account deletion/export for notification and diagnostics data.
- Third-party processors if any.
- Support/admin diagnostics visibility.
- Commerce receipt/confirmation wording.
- DM/report/moderation notification privacy.

## 36. Risk Priority Matrix

| Priority candidate | Items | Rationale |
|---|---|---|
| Candidate P0 | None assigned by this pack. | Current handbook evidence does not support P0 without production verification. |
| Candidate P1 | Notification settings not enforced; private preview payload leakage; user-linked diagnostics called anonymous; push token deletion/account deletion mismatch; diagnostics support/admin visibility without audit; deep-link route reauthorization gap. | These can create privacy-sensitive mismatch between user promises and actual behavior. |
| Candidate P2 | Notification history/export model; commerce notification receipt ambiguity; DM snippet masking; report/moderation notification privacy; stale reminder delivery. | Important beta/pre-scale hardening with incomplete evidence. |
| Candidate P3 | Copy polish and documentation after decisions. | Lower-risk after privacy model is accepted. |
| Unknown / Needs verification | Delivery pipeline, Edge deployment, payload fields, diagnostics linkage, support visibility, deep-link behavior. | Do not convert to patch work before PP-01 evidence and owner decisions. |

## 37. Recommended Decision Records

- Notification Preference Enforcement Decision.
- Private Preview / Push Payload Decision.
- Deep Link Reauthorization Decision.
- Push Token Retention Deletion Decision.
- Notification History Retention Decision.
- Diagnostics Classification Decision.
- `app_diagnostics` Payload Allowlist Decision.
- Support/Admin Diagnostics Visibility Decision.
- Analytics/Crash Disclosure Decision.

## 38. Dependency Map to Later Patch Plan Groups

PP-06 depends on PP-01, PP-02, PP-03, PP-04, and PP-05.

| Later pack | PP-06 dependency |
|---|---|
| PP-07 Abuse/Moderation Workflow Pack | Report/moderation notification privacy, appeal/takedown notifications, evidence payload boundaries. |
| PP-08 Ops/Admin Support Auditability Pack | Support/admin diagnostics visibility, delivery logs, auditability, admin notification surfaces. |
| PP-09 Media Storage Lifecycle Pack | Media notifications that expose storage URLs, public media, uploader identity, or takedown state. |
| PP-10 Messaging Privacy Lifecycle Pack | DM notification snippets, private preview, block/mute, delete/archive, and DM deep links. |

## 39. PP-06 Output Artifacts

Recommended artifacts after execution, not created now:

- `NotificationPrivacyContractDecision.md`
- `NotificationPreferenceEnforcementMatrix.md`
- `PrivatePreviewPushPayloadDecision.md`
- `DeepLinkReauthorizationDecision.md`
- `PushTokenRetentionDeletionDecision.md`
- `DiagnosticsClassificationDecision.md`
- `AppDiagnosticsPayloadAllowlist.md`
- `SupportAdminDiagnosticsVisibilityReview.md`
- `NotificationDiagnosticsImplementationReadinessChecklist.md`

## 40. Execution Preconditions

Before executing PP-06:

- Product owner assigned.
- Legal/privacy owner assigned.
- Notification owner assigned.
- Diagnostics/observability owner assigned.
- Backend/security owner assigned.
- Support/admin owner assigned.
- PP-01 production evidence available where needed.
- PP-02 copy constraints available.
- PP-03 deletion/data-request constraints available.
- PP-04 commerce constraints available.
- PP-05 visibility constraints available.
- Launch scope defined.
- No production changes planned as part of decision work.
- No push delivery execution.
- No diagnostics export.
- No SQL/RLS/RPC/Edge changes.
- No final legal claims made.
- Sanitized evidence rules accepted.

## 41. Explicitly Blocked Actions

PP-06 blocks:

- Push delivery execution.
- Notification behavior changes.
- Diagnostics export.
- Analytics/crash configuration changes.
- Edge Function deployment.
- RLS/RPC/storage changes.
- Production access.
- SQL or Supabase CLI.
- Migrations.
- Source code changes.
- Token deletion.
- User data export.
- Policy publication.
- Legal compliance claims.
- Immediate patch authorization.

## 42. Unknown / Needs Verification Items

- Whether notification preferences are consumed by delivery.
- Which notification categories exist.
- Which notifications are mandatory.
- What private preview masks.
- What push payloads contain.
- Whether deep links re-check backend visibility and authority.
- How push tokens are deleted on logout/account deletion.
- Whether notification history is exportable or deletable.
- Whether reminders are local or server-side.
- How stale/cancelled reminders are removed.
- Whether diagnostics are anonymous, pseudonymous, user-linked, device-linked, mixed, or unknown.
- What fields exist in `app_diagnostics`.
- Who can view diagnostics.
- Whether diagnostics are included in account deletion/export.
- Whether notifications are receipts/invoices or product confirmations.
- How DM/report/moderation notifications are masked.

## 43. Acceptance Criteria for PP-06 Completion

PP-06 is complete only when:

- Notification surface inventory is confirmed.
- Diagnostics surface inventory is confirmed.
- Notification preference model is accepted or explicitly deferred.
- Private preview/push payload model is accepted or explicitly deferred.
- Deep-link reauthorization model is accepted or explicitly deferred.
- Push token retention/deletion model is accepted or explicitly deferred.
- Notification history/reminder model is accepted or explicitly deferred.
- Commerce notification model is accepted or explicitly deferred.
- DM/social/group/moderation notification privacy model is accepted or explicitly deferred.
- Diagnostics classification model is accepted or explicitly deferred.
- `app_diagnostics` payload/visibility model is accepted or explicitly deferred.
- Support/admin diagnostics visibility model is accepted or explicitly deferred.
- PP-01 evidence dependencies are linked.
- PP-02 copy constraints are linked.
- PP-03 deletion constraints are linked.
- PP-04 commerce constraints are linked.
- PP-05 visibility constraints are linked.
- Product owner decisions are assigned.
- Legal/privacy review dependencies are assigned.
- Notification owner is assigned.
- Diagnostics/observability owner is assigned.
- Backend/security owner is assigned.
- Support/admin owner is assigned.
- Follow-up PP-07 through PP-10 groups are updated or explicitly marked unchanged based on notification/diagnostics privacy decisions.
- No final privacy/legal/notification/diagnostics text is treated as approved unless the responsible owner confirms it.

## 44. Recommended Follow-Up Reports

Recommended follow-up reports after execution:

- Notification Privacy Contract Decision.
- Notification Preference Enforcement Matrix.
- Private Preview / Push Payload Decision.
- Deep Link Reauthorization Decision.
- Push Token Retention Deletion Decision.
- Diagnostics Classification Decision.
- `app_diagnostics` Payload Allowlist.
- Support/Admin Diagnostics Visibility Review.
- Notification Delivery / Preference Verification Report.
- Notification Diagnostics Implementation Readiness Checklist.

## 45. Non-Goals

- No code changes.
- No SQL or migrations.
- No production execution.
- No push delivery execution.
- No diagnostics export.
- No analytics/crash configuration changes.
- No Edge Function deployment.
- No notification behavior changes.
- No RLS/RPC/storage changes.
- No token deletion.
- No user data export.
- No legal advice.
- No compliance claim.
- No launch readiness claim.
- No final notification/diagnostics/privacy copy.
- No immediate patch authorization.
- No source-code re-audit.

## 46. Open Questions

- Are notification preferences enforced by delivery?
- What notification categories exist?
- Which notifications are mandatory?
- What does private preview mask?
- What is included in push payloads?
- Do deep links re-check backend visibility and authority?
- How are push tokens deleted on logout/account deletion?
- Is notification history exportable or deletable?
- Are reminders local or server-side?
- How are stale/cancelled reminders removed?
- Is diagnostics anonymous, pseudonymous, user-linked, or mixed?
- What fields exist in `app_diagnostics`?
- Who can view diagnostics?
- Are diagnostics included in account deletion/export?
- Are notifications receipts/invoices or product confirmations?
- How are DM/report/moderation notifications masked?

## 47. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No push/notification/diagnostics/analytics/Edge/RLS/RPC/storage action was executed.
- No files were staged or committed.
- Only `08_PatchPlans/PP06NotificationDiagnosticsPrivacyPack.md` was created/modified.

# Payments / Refunds / Disputes Operations Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false

## 2. Purpose

This audit maps JoinFolk payment, refund, dispute, chargeback, commerce-support, and revenue-operations authority. It separates user purchase UI from backend financial authority, order/payment state from ticket entitlement, cancellation from refund, reservations from paid tickets, and local payment-provider placeholders from deployed payment-provider evidence.

This is not an implementation plan, patch plan, cleanup plan, or migration plan. No application, dashboard, mobile, web, Supabase, backend/RPC/RLS/storage/auth, payment-provider, or migration work is authorized by this audit.

## 3. Audit Scope

In scope:

- Commerce orders, payment-like state, order expiry, and ticket issuance.
- Ticket purchase RPC families and current mobile commerce wrapper behavior.
- Refund, dispute, chargeback, provider, and webhook evidence if found.
- Reservation, claim, gift, transfer, wallet, QR/check-in, venue seat/session, and notification boundaries.
- Host revenue visibility and ops/admin/support revenue-operation authority.
- Revenue auditability and direct data/RLS reliance.

Read-only inspection covered the handbook and targeted source paths only. No production connection was made.

## 4. Payments / Refunds / Disputes Operations Contract Summary

Observed evidence supports an order-centric commerce model, but not an active external payment-provider or refund/dispute system.

High-confidence observations:

- Mobile `commerce.ts` is a provider-agnostic commerce boundary around `create_commerce_order_v1`.
- Mobile `commerce.ts` states `COMMERCE_PAYMENT_ENABLED` defaults to false and describes provider adapters as placeholder/future contract.
- Free orders can return `paid` state and issue tickets through server-side `_issue_tickets_from_order_v1`.
- Paid orders are modeled as `pending_payment`, but mobile source warns that paid orders require a future payment collection flow.
- Local migration/source evidence contains `commerce_orders`, `payment_attempts`, `provider_event_log`, `_issue_tickets_from_order_v1`, `confirm_order_payment_v1`, and `expire_stale_orders_v1`.
- Older local evidence names `mark_order_paid_v1` as a development stub intended to be replaced by a provider webhook.
- No active Stripe/provider webhook implementation was confirmed in targeted runtime source.
- Refund and dispute/chargeback implementation was not confirmed. Refund appears in order status vocabulary and user-facing policy text, but no dedicated refund operation surface was verified.

Clean contract expectation:

- Backend/RPC/RLS/payment-provider authority owns financial state.
- Frontend screens are purchase intent and display surfaces only.
- Ticket entitlement derives from backend-issued tickets, not from UI payment assumptions.
- Refund, cancellation, dispute, chargeback, reservation cancellation, and ticket cancellation are separate state transitions.
- Revenue mutations are ops/admin/support-authorized only when explicitly backend-gated and auditable.

## 5. Payments and Revenue Surface Inventory Matrix

| Surface / domain | Payment/refund/dispute action or visibility exposed | Access path observed | Expected authority owner | Scope | Production evidence status | Risk class | Recommendation |
|---|---|---|---|---|---|---|---|
| Mobile commerce boundary | Create order intent and receive order/ticket result | RPC-mediated mutation: `create_commerce_order_v1` | Backend/RPC/RLS/auth | Buyer | Local source + prior reports; production body needs verification | Revenue-sensitive | Preserve; verify RPC body/grants |
| Commerce order table | Stores order/payment-like state | RPC-only reliance expected | Backend/RPC/RLS/auth | Buyer/ops/provider | RLS enabled with deny-all style authenticated policy evidence | Revenue-sensitive; compliance/audit-sensitive | Document contract |
| Payment attempts | Provider-facing attempt log candidate | Backend/local migration evidence | Backend/RPC/RLS/payment provider | Internal/provider | Local-source-only in this audit | Revenue-sensitive | Verify active production surface |
| Provider event log | Webhook/event ingestion audit candidate | Local migration evidence | Payment provider + backend | Internal/provider | Local-source-only; deployment unknown | Compliance/audit-sensitive | Verify provider/webhook boundary |
| Free order issuance | Issue tickets for zero-price basket | RPC/internal helper candidate | Backend/RPC/RLS/auth | Buyer | Local source; production active path needs verification | Revenue-sensitive | Verify issuance helper grants |
| Paid order pending state | Hold order pending external payment | RPC/local contract | Backend/RPC/payment provider | Buyer/provider | Local source says provider flow future/disabled | Revenue-sensitive | Needs product decision |
| Purchase RPC v3/v4/v5 | Ticket purchase/issuance paths | RPC-mediated mutation | Backend/RPC/RLS/auth | Buyer/claim sender | Split/versioned; production canonical path unclear | Revenue-sensitive | Reconcile |
| `mark_order_paid_v1` | Development payment mark candidate | RPC candidate | Backend/payment provider only | Internal/support unknown | Local evidence describes dev stub | Security-sensitive; revenue-sensitive | Verify reachability |
| `confirm_order_payment_v1` | Payment confirmation candidate | Server-only RPC in local evidence | Backend/payment provider | Provider/internal | Local-source-only deployment path | Revenue-sensitive | Verify provider/webhook boundary |
| `expire_stale_orders_v1` | Expire stale/pending orders | RPC/cleanup utility candidate | Backend/RPC/process | Internal | Local evidence; production schedule unknown | Product correctness; revenue-sensitive | Verify lifecycle |
| Refunds | Refund money or mark refunded | Not confirmed | Ops/admin RPC + provider | Support/ops/provider | Not confirmed | Revenue-sensitive | Needs product decision |
| Disputes/chargebacks | Provider dispute handling | Not confirmed | Payment provider + ops/admin process | Provider/support | Not confirmed | Revenue-sensitive; compliance-sensitive | Unknown / Needs verification |
| Reservations | Reservation status and capacity | RPC-mediated reservation flows | Backend/RPC/RLS/auth | Reservation owner/host | RLS high-level evidence; payment boundary unclear | Product correctness | Document money boundary |
| Claims/gifts/transfers | Transfer entitlement, not payment by default | RPC-mediated claim/transfer | Backend/RPC/RLS/auth | Claim sender/recipient | Tickets/claims RLS evidence; zero direct policies for claims | Revenue-sensitive | Preserve RPC authority |
| Wallet/ticket UI | Shows ticket/claim/order-like results | RPC reads/UI state | Backend-derived truth expected | Ticket holder | Local app evidence; table policy varies | Revenue-sensitive; privacy-sensitive | Keep UI non-authoritative |
| Check-in/QR | Verify entitlement at door | RPC-mediated check-in | Backend/RPC/RLS/auth | Ticket holder/staff | Positive controls in prior reports | Security-sensitive | Keep separate from payment |
| Host revenue visibility | Ticket stats/order-like visibility | Dashboard/mobile host surfaces | Backend/RPC/RLS/auth | Host | No dedicated order support page confirmed | Revenue-sensitive | Verify visibility only |
| Ops/admin revenue support | Refund/dispute/order mutation | Not confirmed | Ops/admin RPC + audit | Support/ops | Not confirmed | Revenue-sensitive; operational/admin-sensitive | Needs decision |
| Notification side effects | Notify purchase/reservation/claim state | RPC/local notification evidence | Backend/RPC/RLS/auth | User/host | RLS high-level evidence; payload correctness incomplete | Privacy-sensitive | Verify semantics |

## 6. Role Vocabulary and Authority Boundary

- Buyer: authenticated user creating a purchase/order intent.
- Ticket holder: user with backend-issued ticket entitlement.
- Reservation owner: user with reservation state. Reservation state is not payment state unless product explicitly says so.
- Host: event owner/operator who may see event-scoped revenue or ticket stats, but is not refund/dispute authority by default.
- Staff: event operational role. Staff/scanner authority is not revenue operations authority.
- Ops/admin: internal authority for sensitive revenue support only if backend-gated and auditable.
- Support: possible read/review role. Support visibility is not refund/dispute mutation authority.
- Payment provider: external processor or webhook authority if integrated and verified.
- Claim sender: user creating or transferring gift/claim entitlement.
- Claim recipient: user accepting an entitlement.
- Public/authenticated user: product access state, not financial authority.

Payment/order state is not the same as ticket entitlement unless backend contract says so. Ticket cancellation is not refund. Reservation cancellation is not refund unless payment exists. Host revenue visibility is not ops/admin payment authority. Frontend purchase screens are not financial authority.

## 7. Commerce Order Data Model Assessment

Observed model candidates:

- `commerce_orders` stores order state, buyer/event/session references, amount/currency-like fields, provider-like fields, status, expiry, and paid timestamp candidates in local source/migration evidence.
- Mobile order status vocabulary includes `draft`, `pending_payment`, `paid`, `failed`, `expired`, `canceled`, and `refunded`.
- Local evidence includes `payment_attempts` as provider-facing attempt log candidate.
- Local evidence includes `provider_event_log` as provider event/webhook audit candidate.
- Prior production evidence found `commerce_orders` RLS enabled with deny-all style authenticated policy evidence.

Assessment:

- `commerce_orders` appears to be product order state and partial financial traceability, not necessarily a formal financial audit log.
- Order idempotency is modeled in mobile and migration evidence.
- Exact production schema, grants, active functions, and provider fields remain Needs verification.
- Direct user reads/writes to orders should remain blocked or explicitly RLS-scoped; sensitive mutation should go through backend RPC/provider authority.

## 8. Ticket Purchase / Payment Authority Assessment

Observed purchase surfaces:

- Mobile ticket sales wrapper calls `purchase_event_ticket_v2`, `purchase_event_ticket_v3`, and `purchase_event_ticket_v5`.
- Mobile commerce wrapper calls `create_commerce_order_v1`.
- Prior Commerce audit identified `purchase_event_ticket_v4` overloads, `create_ticket_order_v1`, `create_commerce_order_v1`, `mark_order_paid_v1`, `confirm_order_payment_v1`, `_issue_tickets_from_order_v1`, and `expire_stale_orders_v1`.
- Prior Commerce audit identified cross-entitlement guard evidence for some order/purchase/reservation paths.

Assessment:

- Ticket purchase and issuance must be backend/RPC-authoritative.
- UI basket, seat picker, buyer preview, and wallet refresh bus are not financial authority.
- `purchase_event_ticket_v5` is a strong active candidate in mobile source, but production canonical active path still needs verification.
- `create_commerce_order_v1` appears to be the newer commerce boundary for multi-product/seat/session flow.
- Older direct purchase RPCs are duplicated/legacy candidates unless product declares them active.

## 9. Payment Provider / Webhook Boundary Assessment

Provider evidence:

- Mobile `commerce.ts` defines a provider adapter interface but describes it as not instantiated yet.
- `COMMERCE_PAYMENT_ENABLED` is false in observed mobile source.
- Local migration evidence references provider names and provider event logging.
- Local evidence references `confirm_order_payment_v1` as server-only and provider/webhook-facing.
- Older local migration evidence labels `mark_order_paid_v1` as a development stub intended to be replaced by a provider webhook.
- No active Stripe/provider webhook route or deployed Edge Function was confirmed in targeted runtime source.

Contract expectation:

- Payment callbacks/webhooks must be authenticated, idempotent, and provider-verified.
- Provider events should not expose raw sensitive provider payloads to product users.
- Local provider placeholders are not production provider evidence.

Status: Unknown / Needs verification for active provider deployment.

## 10. Refund Operations Assessment

Refund implementation was not confirmed.

Observed evidence:

- Mobile order status vocabulary includes `refunded`.
- Mobile terms/purchase UI contain refund-policy text.
- Migration comments mention operational states where active tickets may need cancellation or refund before layout changes.
- No dedicated refund RPC, support page, provider refund callback, or refund audit log was confirmed in targeted runtime source.

Contract expectation:

- Refund is distinct from ticket cancellation, claim invalidation, reservation cancellation, and support notes.
- Refund mutation must be ops/admin/support-authorized, provider-backed where money moved, and auditable.
- Refund state must define entitlement consequences: ticket canceled, ticket remains valid, partial refund, failed refund, or Unknown.

Status: Not confirmed / Needs product decision.

## 11. Dispute / Chargeback Operations Assessment

Dispute and chargeback implementation was not confirmed.

Observed evidence:

- Targeted search did not confirm active dispute or chargeback handlers.
- Terms text references disputes between users and venues, but this is product/legal text, not implementation evidence.
- No provider dispute webhook, support dispute page, chargeback status, or dispute audit log was confirmed.

Contract expectation:

- Disputes/chargebacks require provider-state authority, support process, auditability, and entitlement consequences.
- Chargeback state should not be inferred from cancellation or refund state.
- Dispute payloads are privacy- and compliance-sensitive.

Status: Not confirmed.

## 12. Cancellation / Stale Order / Expiry Assessment

Observed evidence:

- Local order status vocabulary includes `expired` and `canceled`.
- Local order functions include `expire_stale_orders_v1`.
- Local commerce order evidence includes `expires_at` and pending-payment expiry.
- Venue/seat/session audits identified capacity and hold release as revenue-sensitive.

Assessment:

- Stale pending orders should release capacity, seats, sections, and sessions according to backend contract.
- Cancellation is not refund unless provider/payment state exists and refund flow is executed.
- Expiry should not issue tickets after hold expiration.
- The active scheduler/process for stale-order cleanup was not confirmed.

Status: Product-critical and revenue-sensitive; Needs verification.

## 13. Reservation Money Boundary Assessment

Observed evidence:

- Reservation RPCs and reservation status flows exist from prior Commerce, Staff/Host, and Direct Access audits.
- Cross-entitlement guard evidence connects reservations with ticket/claim acquisition conflict prevention.
- No paid reservation provider path was confirmed in targeted source.

Contract assessment:

- Event reservations appear primarily as reservation/participation state, not confirmed payment state.
- Venue reservations may include operational approval/cancellation but no payment/refund boundary was confirmed here.
- Reservation cancellation is not refund unless a paid reservation product exists and is verified.
- Reservation ownership should not imply ticket wallet ownership or QR/check-in entitlement unless backend explicitly converts it.

Status: Product-critical; financial boundary Unknown / Needs verification.

## 14. Claim / Gift / Transfer Financial Boundary Assessment

Observed evidence:

- Claim/gift/transfer RPC families exist in local and prior audit evidence: `create_ticket_claim_v1`, `claim_ticket_v1`, and `transfer_gift_ticket_v1`.
- `event_ticket_claims_v1` had RLS enabled with zero direct policies in prior focused evidence.
- Gift claim transfers can move entitlement, but no money movement/refund/dispute path was confirmed.
- Cross-entitlement guard evidence covers active gift entitlement and pending recipient claims.

Contract assessment:

- Claims/gifts/transfers should transfer entitlement, not create duplicate paid entitlement.
- Claim token possession should not bypass payment/order/ticket authority.
- A claim recipient accepting a gift should not be treated as the payer unless order/payment records explicitly say so.
- Host identity transfer and gift ticket transfer are different domains.

Status: Revenue-sensitive; mostly RPC-mediated but active financial semantics need documentation.

## 15. Wallet / Entitlement / Ticket Truth Assessment

Truth hierarchy expectation:

- `commerce_orders` and provider confirmation represent order/payment state.
- `tickets` represent issued ticket entitlement.
- `event_ticket_claims_v1` represents pending or accepted claim/gift entitlement.
- `reservations` represent reservation participation state unless payment is explicitly added.
- Wallet UI displays backend-derived entitlement and is not financial authority.
- Check-in proof and QR state derive from ticket entitlement and event/check-in rules, not from payment UI.

Observed evidence:

- Mobile purchase wrappers emit wallet refresh events after successful ticket/claim result.
- Ticket reads and wallet screens rely on RPC/table-backed ticket state.
- Prior reports found tickets RLS enabled with zero direct policies, making RPC/default-deny assumptions critical.

Status: Product-critical and revenue-sensitive.

## 16. Check-In / QR / Revenue Boundary Assessment

Check-in and QR are entitlement verification, not payment processing.

Observed evidence:

- Prior Staff/Host audit found positive controls in `checkin_ticket_by_id_v2` and `staff_checkin_ticket_v1`.
- Mobile ticket screen can present receipt-like ended/archived mode.

Contract assessment:

- Checked-in status does not imply refund eligibility.
- Scanner/staff authority does not imply refund, dispute, payment, or order mutation authority.
- Check-in denial or attendance status should not change payment state unless an explicit backend support process does so.

Status: Mostly deterministic for active check-in path, but revenue consequences are Unknown / Needs verification.

## 17. Host Revenue Visibility Assessment

Observed evidence:

- Host/dashboard ticket product, sales stats, attendees, and ticket/reservation views exist in prior audits.
- No dedicated host order/refund/dispute mutation surface was confirmed.
- Ops/Admin audit did not confirm a dedicated ops ticket/order/refund/dispute page in focused route inspection.

Contract expectation:

- Host revenue visibility is event-scoped.
- Host can see sales/status metrics only as product permits.
- Host visibility does not grant refund/dispute mutation authority.
- Host should not see private payment provider payloads unless product/security accepts a specific field set.

Status: Needs verification for exact fields and host visibility contract.

## 18. Ops / Admin / Support Revenue Operations Assessment

Observed evidence:

- No dedicated support/admin refund/dispute/order mutation page was confirmed in focused ops route inspection.
- Manual support override concerns were flagged by Ops/Admin and Diagnostics audits.
- Revenue-sensitive support actions should be backend/RPC-authoritative and auditable.

Contract expectation:

- Support read visibility is not mutation authority.
- Refund/dispute/cancel/reissue/manual order actions require explicit backend gates and audit logs.
- External/manual provider console actions, if used, need process-level auditability.
- Ops/admin authority must not leak into host/staff/public surfaces.

Status: Unknown / Needs verification.

## 19. Notification / Push / Receipt Side Effects Assessment

Observed evidence:

- Notification audit linked ticket purchase, reservation, claim, transfer, and payment-like states to notification side effects.
- Local gift/claim migrations include notification-related evidence.
- No receipt/invoice email implementation was confirmed.
- Local Edge Function source exists in some trees, but no deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.

Contract assessment:

- Notification records are not financial receipts unless product explicitly defines them as such.
- Push/email deployment evidence must be separate from local source.
- Receipt/invoice payloads, if added, must avoid private provider payload leakage.
- Refund/dispute notifications should not be claimed active without evidence.

Status: Unknown / Needs verification for receipt/refund/dispute side effects.

## 20. Audit Log / Revenue Traceability Assessment

Revenue-sensitive actions should be traceable.

Expected audit fields:

- Actor.
- Action.
- Target order/ticket/reservation/claim.
- Amount and currency where relevant.
- Previous and new state.
- Timestamp.
- Non-secret provider reference if accepted.
- Reason or support case reference where product requires.

Observed evidence:

- `commerce_orders`, `payment_attempts`, and provider event log candidates provide partial operational traceability in local source.
- Diagnostics audit flagged commerce/ticket/reservation auditability as unresolved.
- Formal refund/dispute audit logs were not confirmed.

Status: Candidate P1/P2 depending on active paid flow and provider deployment.

## 21. Public Web / Share / Claim Boundary Assessment

Observed evidence:

- Public claim/share surfaces were audited separately.
- Claim/gift flows can expose preview and handoff state.
- No public payment state exposure was confirmed in targeted source.

Contract expectation:

- Public claim/share routes must not expose private payment state.
- Claim token possession does not bypass order/payment/ticket authority.
- Public verification/check-in surfaces should expose only public-safe proof, not payment state.
- Gift/claim acceptance must not duplicate paid entitlement.

Status: Revenue-sensitive and privacy-sensitive; Needs verification for exact field exposure.

## 22. Venue / Seat / Session Availability Financial Boundary

Observed evidence:

- Venue Buyer audit linked seat/section/standing/session availability to `create_commerce_order_v1`, `purchase_event_ticket_v5`, and availability functions.
- Local commerce/order functions include pending-payment and expiry concepts.
- Local source includes exact-seat validation and product-section usage functions.

Contract expectation:

- Paid or pending orders must not double-sell capacity.
- Stale, canceled, failed, or expired orders should release capacity according to backend contract.
- Seat, section, standing area, and session availability must be backend-authoritative at purchase time.
- UI buyer maps and previews cannot reserve revenue-sensitive inventory by themselves.

Status: Revenue-sensitive; active capacity release semantics need verification.

## 23. Backend RPC / RLS Authority Evidence Map

Prior handbook evidence only; no production connection was made.

- `commerce_orders` had deny-all style authenticated policy evidence.
- `tickets` and `event_ticket_claims_v1` had RLS enabled with zero direct policies in prior evidence and likely depend on RPC/default-deny assumptions.
- Reservations RLS was confirmed at a high level, but policy correctness remains incomplete.
- Purchase/commerce RPC versions exist in local/production evidence, but canonical active path needs verification.
- Cross-entitlement guard evidence exists for some RPCs; overload/version coverage remains Needs verification.
- Local evidence includes `payment_attempts` and provider event log candidates, but supplied production RLS evidence did not fully cover those surfaces.
- `confirm_order_payment_v1`, `_issue_tickets_from_order_v1`, and `expire_stale_orders_v1` exist in local/backend evidence; active production caller model needs verification.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- Production SQL/RPC evidence remains stronger than local source assumptions.

Do not treat unreviewed payment/refund/dispute functions or tables as safe.

## 24. Direct Data Access / RLS Reliance Map

| Data surface | Direct/RPC access observed | Authority concern | Evidence status | Recommendation |
|---|---|---|---|---|
| `commerce_orders` | RPC-mediated in observed app paths | Order/payment state | Deny-all style policy evidence | Preserve RPC-only posture |
| `payment_attempts` | Local backend/provider log candidate | Provider attempt privacy and auditability | Local-source-only here | Verify production surface |
| Provider event log candidate | Local webhook/event audit candidate | Provider authenticity and payload minimization | Local-source-only here | Verify provider boundary |
| `tickets` | RPC/default-deny reliance | Entitlement truth | RLS enabled with zero direct policies in prior evidence | Preserve RPC authority |
| `event_ticket_claims_v1` | RPC/default-deny reliance | Claim/gift entitlement | RLS enabled with zero direct policies in prior evidence | Preserve RPC authority |
| `event_ticket_products_v1` | Product/pricing/capacity setup | Price/capacity correctness | Production evidence incomplete | Verify host/product authority |
| `reservations` | RPC/direct mixed by prior audits | Reservation/payment boundary | RLS high-level evidence | Document money boundary |
| `venue_reservations` | Venue reservation operations | Paid/unpaid semantics unknown | Production evidence incomplete | Needs verification |
| `event_sessions_v1` | Session-scoped order/availability | Capacity and expiry | Local evidence | Verify active path |
| `notifications_v2` | Notification side effects | Receipt/payload ambiguity | RLS high-level evidence | Verify semantics |
| Profiles/user_profiles | Buyer/host/support identity context | Privacy in revenue views | Production evidence incomplete | Verify field contract |
| Audit logs | Revenue traceability | Formal audit completeness | Incomplete | Define audit contract |

## 25. Duplicated / Split / Legacy Payment Surfaces

| Surface / helper / RPC / table | Observed role | Current / legacy / unknown | Risk if still active or authoritative | Evidence type | Recommendation |
|---|---|---|---|---|---|
| `purchase_event_ticket_v2` | Older direct ticket purchase | Legacy / unknown | May bypass newer order semantics | Mobile wrapper | Verify active usage |
| `purchase_event_ticket_v3` | Multi-product purchase | Active/legacy candidate | May differ from commerce order path | Mobile + local backend | Reconcile |
| `purchase_event_ticket_v4` | Overloaded purchase path | Unknown / overloaded | Ambiguous active body and guard coverage | Prior Commerce audit | Review source of truth |
| `purchase_event_ticket_v5` | Basket purchase replacement for v4 | Active candidate | Production-current status unverified | Mobile + local backend | Confirm canonical path |
| `create_ticket_order_v1` vs `create_commerce_order_v1` | Order creation alternatives | Split / unknown | Duplicate order semantics | Prior audits | Name canonical order path |
| `mark_order_paid_v1` | Development payment mark candidate | Legacy / unsafe unknown | Revenue-sensitive if callable by wrong actor | Local migration evidence | Verify reachability |
| `confirm_order_payment_v1` | Server-only payment confirmation candidate | Unknown | Needs provider/auth/idempotency boundary | Local backend evidence | Verify provider boundary |
| `_issue_tickets_from_order_v1` | Ticket issuance helper | Internal candidate | Revenue-sensitive if externally reachable | Local/backend evidence | Verify grants/caller model |
| `expire_stale_orders_v1` | Pending order cleanup | Unknown | Capacity release may be inconsistent | Local/backend evidence | Verify scheduler/process |
| `refunded` status without refund flow | Refund state vocabulary | Unknown / incomplete | Product may imply unsupported operation | Mobile/local schema evidence | Needs product decision |

## 26. Revenue-Critical Invariants

- Frontend purchase UI is not financial authority.
- Payment/order state and ticket entitlement must be backend-authoritative.
- Refund is distinct from cancellation.
- Dispute/chargeback is distinct from refund.
- Reservation status is not payment state unless product explicitly says so.
- Claim/gift/transfer flows must not duplicate paid entitlement.
- Ticket holder/check-in state does not grant refund/dispute authority.
- Host revenue visibility is not refund/dispute mutation authority.
- Staff/scanner authority is not revenue operations authority.
- Support/admin revenue mutations are auditable.
- Stale/expired orders release capacity according to backend contract.
- Notifications are not receipts unless product defines them.
- Local Edge Function source is not payment/provider deployment evidence.
- Public claim/share routes do not expose private payment state.

## 27. Unknown / Needs Verification Surfaces

- Active canonical purchase path among `create_commerce_order_v1`, purchase v3/v4/v5, and older order RPCs.
- Production grants/caller model for `confirm_order_payment_v1`, `mark_order_paid_v1`, `_issue_tickets_from_order_v1`, and `expire_stale_orders_v1`.
- Whether a payment provider is deployed.
- Whether payment webhooks exist, are authenticated, and are idempotent.
- Whether `payment_attempts` and provider event logs exist in production with correct policies.
- Whether refund operations exist.
- Whether dispute/chargeback operations exist.
- Whether stale-order expiry is scheduled or manually invoked.
- Whether order expiry releases seat/section/standing/session capacity consistently.
- Whether host revenue visibility exposes only accepted fields.
- Whether support/admin revenue mutations are audited.
- Whether notifications are receipts or only product updates.

## 28. Payments / Refunds / Disputes Gaps / Risk Register Seeds

| Gap ID | Domain | Current issue | Expected clean payments/refunds/disputes contract | Risk | Priority candidate | Blocked by | Recommended next action |
|---|---|---|---|---|---|---|---|
| PRD-GAP-001 | Canonical purchase path | Multiple purchase/order RPCs exist and active production path is not fully established | One accepted order/purchase authority owns payment and issuance | Revenue-sensitive | Candidate P1 | Production RPC/body/grant review | Reconcile purchase/order source of truth |
| PRD-GAP-002 | Provider/webhook boundary | Provider adapter and webhook concepts exist locally, but deployment was not confirmed | Provider callbacks are authenticated, idempotent, and deployed only with accepted contract | Security-sensitive; revenue-sensitive | Candidate P1 | Provider deployment evidence | Verify provider/webhook architecture |
| PRD-GAP-003 | Payment confirmation | `mark_order_paid_v1` and `confirm_order_payment_v1` have unclear active caller model | Payment confirmation is provider/backend-only and auditable | Revenue-sensitive | Candidate P1 | Function reachability review | Verify grants and caller path |
| PRD-GAP-004 | Ticket issuance helper | `_issue_tickets_from_order_v1` is revenue-critical and active exposure is unclear | Issuance helper is internal/backend-authoritative only | Revenue-sensitive | Candidate P1 | Function grant review | Verify helper reachability |
| PRD-GAP-005 | Refunds | Refund state appears in vocabulary but refund flow was not confirmed | Refund operations are provider-backed, ops/admin-gated, and auditable | Revenue-sensitive; compliance-sensitive | Candidate P2 | Product/provider decision | Decide refund product contract |
| PRD-GAP-006 | Disputes/chargebacks | No dispute/chargeback implementation was confirmed | Dispute handling has provider state, support process, entitlement impact, and audit trail | Compliance/audit-sensitive | Unknown | Provider/support policy decision | Define dispute/chargeback process |
| PRD-GAP-007 | Stale orders | Expiry function exists locally, but schedule/capacity release semantics are unverified | Stale orders expire predictably and release inventory | Revenue-sensitive; product correctness | Candidate P1 | Scheduler/process verification | Audit stale order lifecycle |
| PRD-GAP-008 | Reservations money boundary | Reservations exist, but paid/unpaid boundary is unclear | Reservation status and payment state are explicitly separate or integrated | Product correctness; revenue-sensitive | Candidate P2 | Product decision | Document reservation money contract |
| PRD-GAP-009 | Claims/gifts/transfers | Claims transfer entitlement, but payer/recipient money semantics need documentation | Gift/claim/transfer flows do not duplicate paid entitlement and preserve payment truth | Revenue-sensitive | Candidate P2 | Commerce/claim review | Document financial ownership semantics |
| PRD-GAP-010 | Revenue auditability | Formal logs for refund/dispute/payment support operations were not confirmed | Revenue mutations record actor, target, amount/currency, state change, and provider reference | Compliance/audit-sensitive | Candidate P1 | Diagnostics/audit review | Define revenue audit log contract |

## 29. Product Decisions Required

- Is paid ticketing active today, or are only free commerce orders supported?
- Which purchase/order RPC is canonical?
- Which provider, if any, owns payment confirmation?
- Are payment provider webhooks deployed?
- Is `mark_order_paid_v1` still callable or purely legacy?
- What statuses are accepted for orders?
- What does `refunded` mean if no refund flow is active?
- Are refunds supported, and who can initiate them?
- Are disputes/chargebacks supported, and where are they tracked?
- Do stale orders reserve capacity until expiry?
- Are reservations ever paid?
- Who is the payer for gift/claim/transfer scenarios?
- What host revenue fields are visible?
- What support/admin revenue operations are allowed?
- Which notification or email is a formal receipt, if any?

## 30. Recommended Next Audits

1. Privacy / Data Retention / Deletion Contract Audit.
2. Legal / Trust & Safety Policy Mapping Audit.
3. Release Readiness / Production Hardening Gap Register.

These follow from unresolved provider payloads, financial audit retention, refund/dispute policy, and production hardening questions.

## 31. Non-Goals

- This audit does not authorize payment provider integration.
- This audit does not authorize backend, RLS, RPC, storage, auth, Edge Function, or Supabase changes.
- This audit does not create migrations or database statements.
- This audit does not verify production directly.
- This audit does not claim refund/dispute support exists unless evidence supports it.
- This audit does not claim local Edge Function source is active payment/provider infrastructure.
- This audit does not claim RLS is correct solely because it is enabled.
- This audit does not recommend immediate patches.

## 32. Open Questions

- Is `COMMERCE_PAYMENT_ENABLED = false` consistent with production product behavior?
- Are any paid products currently offered to users?
- Is `payment_attempts` active in production?
- Is provider event logging active in production?
- What actor may invoke payment confirmation?
- Is stale order expiry automated?
- How are seats/standing/session holds released after expiration?
- Are refunds intentionally unsupported or simply incomplete?
- Are disputes handled outside the app?
- Do host dashboards show order-level financial data?
- Are support/admin revenue reads audited?
- Are public claim/share previews prevented from exposing payment state?

## 33. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `07_Audits/PaymentsRefundsDisputesOperationsAudit.md` was created/modified.

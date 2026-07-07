# PP-04 Commerce / Refund / Payment Contract Pack

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook audit synthesis only
- canonical: false
- Execution status: Not executed
- Legal status: Engineering planning only; not legal advice
- Payment status: Not executed / Provider not verified

## 2. Purpose

This is a decision-pack for defining JoinFolk commerce, refund, payment, ticketing, reservation, claim, dispute, chargeback, provider, and support-operation contract semantics before implementation work begins.

It is not payment execution, not payment-provider integration, not legal advice, not final refund/payment policy, not production verification, and not patch authorization.

## 3. Evidence Boundary

This document is based only on handbook audits, the Release Readiness / Production Hardening Gap Register, PP-01, PP-02, and PP-03.

No source-code inspection, production connection, Supabase CLI, SQL, builds, tests, provider verification, payment execution, refund execution, dispute action, ticket issuance, reservation mutation, legal review, or final copy drafting was performed.

## 4. PP-04 Scope Summary

PP-04 covers:

- Commerce contract vocabulary.
- Canonical commerce flow options.
- Ticket, order, payment, refund, dispute, reservation, claim, transfer, check-in, and capacity boundaries.
- Payment-provider and webhook evidence dependencies.
- Capacity/inventory hold, expiry, and release dependencies.
- Checkout, refund, support, public web, wallet, and ticket copy constraints.
- Commerce retention and data-request constraints from PP-03.
- Backend/RPC/RLS verification dependencies from PP-01.
- Dependency mapping to PP-05 through PP-10.

PP-04 does not select a final commerce model, activate a provider, create final legal copy, or authorize implementation.

## 5. Source Register Coverage

| Release gap | Why PP-04 covers it | PP-04 limitation |
|---|---|---|
| RR-GAP-002 | Sensitive commerce RPC bodies, grants, security mode, overloads, and RLS policies need production verification. | PP-04 lists verification needs; PP-01 executes verification later if authorized. |
| RR-GAP-003 | Commerce and ticketing rely on mixed direct table access, RLS, RPCs, and UI state. | PP-04 does not verify policies or callsites. |
| RR-GAP-005 | Purchase/order/ticket/claim/reservation source of truth remains unresolved. | PP-04 defines decision model, not code changes. |
| RR-GAP-006 | Refund policy copy conflicts with checkout copy while provider/refund/dispute behavior is not confirmed. | PP-04 does not provide final legal refund terms. |
| RR-GAP-007 | Venue buyer seat, standing, session, product, capacity, and purchase revalidation need authoritative contract. | PP-04 covers capacity/availability decisions; PP-05/venue work handles public parity. |
| RR-GAP-008 | Event lifecycle affects commerce availability, cancellation, refunds, and notifications. | PP-04 does not decide full lifecycle state machine. |
| RR-GAP-009 | Public share/claim/check-in routes must not expose private or incorrect commerce state. | PP-04 identifies commerce copy/public boundary needs; PP-05 owns suppression. |
| RR-GAP-016 | Staff/scanner/check-in proof authority affects ticket entitlement and refund/cancel decisions. | PP-04 does not verify staff/check-in RPC bodies. |
| RR-GAP-018 | Support/admin refund, override, dispute, and ticket correction process needs authority and auditability. | PP-08 owns support/admin authority. |
| RR-GAP-020 | Commerce retention and account deletion exceptions affect payment/order/ticket data. | PP-04 depends on PP-03 retention decisions. |
| RR-GAP-021 | Refund/payment copy requires legal/product reconciliation. | PP-04 depends on PP-02 copy constraints. |
| RR-GAP-022 | Provider payloads, receipts, refunds, disputes, and revenue auditability need contract decisions. | PP-04 maps decisions; it does not verify provider deployment. |
| RR-GAP-023 | Commerce decisions must be accepted before patches. | PP-04 recommends decision records only. |

## 6. Commerce Contract Problem Statement

Commerce is not one operation. It crosses product, payment, entitlement, availability, support, legal, and audit domains:

- Product/listing.
- Ticket entitlement.
- Reservation request.
- Venue/table/booth reservation.
- Order.
- Payment attempt.
- Provider event.
- Webhook.
- Refund.
- Dispute.
- Chargeback.
- Cancellation.
- Stale or expired order.
- Capacity hold/release.
- Claim, gift, and transfer.
- QR, check-in, proof, and public verification.
- Host, venue, support, and admin actions.
- Revenue/audit retention.

The core decision is which record or workflow is authoritative for paid entitlement, capacity, refundability, and user-facing promises.

## 7. Commerce Surface Inventory

| Surface | User expectation | Data/function domains affected | Owner | PP-01 evidence dependency | PP-02 copy dependency | PP-03 retention dependency | Current status | Recommended PP-04 decision need |
|---|---|---|---|---|---|---|---|---|
| Buy ticket | Payment results in valid ticket. | Orders, tickets, products, provider, capacity. | Product + commerce | Purchase RPC/table/provider evidence | Checkout/refund copy | Commerce retention | Unknown / Needs verification | Decide canonical paid flow. |
| Free ticket / request ticket | User gets or requests entitlement without payment. | `request_ticket_v2`, tickets, capacity. | Product | RPC body/grants | Ticket copy | Ticket retention | Unknown / Needs verification | Decide beta/free scope. |
| Create order | Checkout creates pending order. | `commerce_orders`, products, capacity. | Commerce | Order schema/RLS/RPC | Checkout copy | Order retention | Unknown / Needs verification | Decide order authority and expiry. |
| Mark/confirm order paid | Payment state becomes paid. | Provider/webhook/manual functions. | Commerce + provider | Function/provider evidence | Payment copy | Provider retention | Unknown / Needs verification | Decide payment authority. |
| Issue ticket from order | Paid order creates entitlement. | Tickets, order helpers, products. | Commerce | Issuance helper evidence | Ticket/wallet copy | Ticket retention | Unknown / Needs verification | Decide entitlement issuance moment. |
| Expire stale order | Abandoned checkout releases capacity. | Orders, capacity, products. | Commerce | Expiry function evidence | Checkout availability copy | Order retention | Unknown / Needs verification | Decide hold/release model. |
| Cancel order | Pending or paid order cancelled. | Orders, tickets, capacity, notifications. | Commerce + support | Cancel evidence | Cancellation copy | Retention exception | Unknown / Needs verification | Separate cancellation from refund. |
| Refund ticket/order | Money returned or requested. | Provider, orders, tickets, audit. | Legal + commerce + support | Provider/refund evidence | Refund copy | Commerce retention | Not confirmed | Decide refund policy and authority. |
| Dispute/chargeback | Provider/user challenges charge. | Provider events, tickets, support. | Legal + commerce | Provider/dispute evidence | Dispute copy | Evidence retention | Not confirmed | Decide dispute/chargeback stance. |
| Create reservation | User requests/holds venue/event spot. | Reservations, venue reservations, capacity. | Product + venue | Reservation RPC/table evidence | Reservation copy | Reservation retention | Unknown / Needs verification | Decide paid/unpaid boundary. |
| Cancel reservation | Reservation released/cancelled. | Reservations, capacity, notifications. | Product + venue | Reservation mutation evidence | Cancellation copy | Retention | Unknown / Needs verification | Decide effect on payment if any. |
| Claim ticket | Recipient accepts claim. | Claims, tickets, share routes. | Product + commerce | Claim RPC/table evidence | Claim/share copy | Claim retention | Unknown / Needs verification | Decide entitlement transfer semantics. |
| Gift/transfer ticket | Sender transfers entitlement. | Tickets, claims, transfers, audit. | Product + commerce | Transfer RPC evidence | Gift/transfer copy | Payment history retention | Unknown / Needs verification | Decide financial owner vs holder. |
| Check-in ticket | Staff verifies entitlement. | Tickets, proof, staff, public verification. | Staff/host + commerce | Check-in/proof RPC evidence | QR/wallet copy | Proof retention | Unknown / Needs verification | Decide check-in effects on refund/transfer. |
| Public QR/ticket verification | Public-safe proof verifies ticket state. | Public routes, proof, tickets. | Product + public web | Public dependency evidence | Verification copy | Proof retention | Unknown / Needs verification | Define public-safe fields. |
| Wallet display | User sees current entitlement. | Tickets, claims, status, QR. | Product | Ticket state evidence | Wallet copy | Ticket retention | Unknown / Needs verification | Ensure wallet is not financial authority. |
| Host sales visibility | Host sees sales/reservations. | Dashboard, tickets, orders, reservations. | Host ops + commerce | RLS/direct access evidence | Host policy copy | Retention | Unknown / Needs verification | Separate host visibility from refund authority. |
| Support/admin override | Support changes revenue state. | Admin RPCs, audit logs, orders/tickets. | Support + ops + legal | Admin functions/audit evidence | Support copy | Audit retention | Unknown / Needs verification | Decide powers and audit requirements. |
| Provider webhook | Provider reports payment state. | Provider events, orders, issuance. | Commerce + provider | Deployment/signature evidence | Payment/provider copy | Provider retention | Not confirmed | Verify active provider before claims. |
| Receipt/invoice/confirmation notification | User receives proof/confirmation. | Notifications, email if any, orders/tickets. | Product + legal + commerce | Notification/provider evidence | Receipt/invoice copy | Record retention | Unknown / Needs verification | Decide whether notifications are receipts. |

## 8. Current Policy / Copy Promise Inventory

| Policy/copy signal | Source audit signal | Engineering evidence status | Risk | Decision needed |
|---|---|---|---|---|
| All-sales-final checkout copy | Checkout reportedly says all ticket sales final/no refunds/exchanges. | Refund/provider behavior not confirmed. | Revenue-sensitive, legal/compliance-sensitive | Decide canonical refund policy. |
| Host refund policy terms signal | Terms reportedly say ticket purchases subject to host refund policy. | Host refund enforcement not confirmed. | Revenue-sensitive | Decide whether host refund policy exists and who enforces it. |
| Payment provider disabled/unknown signal | Prior payments context noted `COMMERCE_PAYMENT_ENABLED=false`; provider active state unknown. | Provider/webhook not verified. | Revenue-sensitive | Decide paid-commerce launch scope after PP-01. |
| Refund/dispute/chargeback absence | Refund/dispute/chargeback implementation not confirmed. | Not confirmed. | Revenue-sensitive | Decide whether these are supported, manual, or deferred. |
| Ticket as entitlement | Ticket/wallet/QR copy implies entry entitlement. | Entitlement issuance path incomplete. | Product correctness, revenue-sensitive | Decide when entitlement is created. |
| Reservation as request/booking | Reservation semantics vary by venue/event context. | Paid/unpaid boundary incomplete. | Product correctness | Decide reservation commerce boundary. |
| Instant ticket / QR / wallet copy | Wallet/QR may imply immediate validity. | Pending payment/order boundary incomplete. | Revenue-sensitive | Ensure copy matches entitlement authority. |
| Event cancellation copy if any | Event lifecycle affects commerce and notifications. | Lifecycle/refund coupling incomplete. | Revenue-sensitive | Decide cancellation/refund side effects. |
| Host/venue responsibility copy | Host/venue obligations intersect refunds and reservations. | Authority/process incomplete. | Operational-sensitive | Decide host/venue responsibility model. |
| Support refund/dispute copy if any | Support promises are process-sensitive. | Support process not confirmed. | Operational/admin-sensitive | Define support process before copy promises. |
| Receipt/invoice/confirmation copy if any | Notifications may be confirmation but not formal receipt. | Receipt/invoice evidence incomplete. | Revenue-sensitive, legal/compliance-sensitive | Decide receipt/invoice semantics. |

## 9. Canonical Commerce Flow Options

| Option | Pros | Risks | Required evidence | Product decision need | Legal review need | Implementation complexity | Copy impact |
|---|---|---|---|---|---|---|---|
| Option A: free/request-ticket only for beta; paid commerce disabled | Reduces payment/refund/provider risk. | Limits monetization; paid UI/copy must be suppressed or scoped. | Verify paid paths disabled and free/request path authority. | High | Medium | Medium | Copy must avoid paid/refund/payment claims. |
| Option B: paid ticket order flow with provider webhook as payment authority | Standard paid commerce model; provider is payment source. | Requires webhook auth, idempotency, provider retention, refund/dispute decisions. | Provider/webhook/payment/order/ticket evidence. | High | High | High | Checkout/refund/provider copy must be precise. |
| Option C: manual/support-mediated payment confirmation | Allows controlled launch without full automation. | High ops burden; audit and fraud risk; support powers must be explicit. | Admin/support functions, audit logs, provider/manual process evidence. | High | High | Medium/High | Copy must state manual processing constraints if user-facing. |
| Option D: host-managed external payments with JoinFolk entitlement only | Avoids direct provider handling. | User confusion; refund/support responsibility boundaries become critical. | Host responsibility, entitlement, and support process evidence. | High | High | Medium | Copy must make payment responsibility clear. |
| Option E: reservations only / no paid ticketing for initial launch | Simplifies revenue operations. | Ticketing/payment features may need hiding or scoping. | Reservation authority and capacity evidence. | High | Medium | Medium | Copy must avoid paid-ticket language. |

No option is selected by this pack. Decision required.

## 10. Ticket Entitlement Decision Model

Decisions needed:

- What is a ticket product.
- Which user owns or holds a ticket.
- Which ticket statuses are active, pending, cancelled, refunded, transferred, claimed, expired, checked-in, or revoked.
- When QR/check-in status becomes available.
- How claim/gift/transfer state affects entitlement.
- Whether refund/cancel revokes entitlement.
- Whether unpaid or pending orders can create entitlement.
- Whether transfer preserves payment history and original purchaser metadata.

Ticket entitlement must be backend/RPC/RLS-authoritative. Wallet or UI display is not financial authority.

## 11. Order / Payment Attempt / Provider Event Decision Model

Decision areas:

- `commerce_orders` role and state machine.
- Payment attempt role and retry behavior, if present.
- Provider event role, if present.
- Order creation authority.
- Paid-state authority.
- Provider payment confirmation authority.
- Ticket issuance from order.
- Idempotency model.
- Stale order expiry.
- Provider payload minimization.

Core decision: which state is authoritative for paid entitlement: provider event, confirmed order, issued ticket, or another accepted source.

## 12. Refund / Cancellation / Dispute / Chargeback Decision Model

These are separate concepts:

- Refund: money return decision.
- Cancellation: product/event/order/ticket/reservation state change.
- Event cancellation: lifecycle event that may trigger commerce side effects.
- Reservation cancellation: availability/request state change.
- Dispute: provider/user challenge process.
- Chargeback: provider/network reversal process.
- No-show: attendance outcome, not automatically refund state.
- Host refund policy: product/legal responsibility question.
- Platform/admin override: operational authority question.

Decisions needed:

- Canonical refund policy.
- Who can initiate refund requests.
- Who can approve refunds.
- Whether host refunds exist.
- Whether support/admin can override.
- Whether refunded tickets are revoked.
- Whether check-in blocks or affects refund.
- Required audit and notification behavior.

No legal wording is provided by this pack.

## 13. Reservation / Venue Reservation Decision Model

Reservation decisions must cover:

- Event reservation.
- Venue/table/booth reservation.
- Reservation request vs confirmed booking.
- Paid vs unpaid reservation boundary.
- Cancellation and release behavior.
- Capacity and expiry.
- Host/venue authority.
- Support correction process.

Decision required: reservations are either commerce records, availability requests, or a separate module with distinct payment and cancellation semantics.

## 14. Claim / Gift / Transfer Decision Model

Claim/gift/transfer decisions must cover:

- `create_ticket_claim_v1`.
- `claim_ticket_v1`.
- `transfer_gift_ticket_v1`.
- Sender and recipient authority.
- Pending, accepted, rejected, cancelled, and expired states.
- Entitlement ownership after transfer.
- Payment history ownership and retention.
- Duplicate entitlement prevention.

Decision required: a transfer may change entitlement holder, financial owner, both, or neither for payment-history purposes.

## 15. Check-In / QR / Proof / Public Verification Decision Model

Check-in decisions must cover:

- QR code semantics.
- Check-in authority.
- Checked-in status.
- Public verification route and public-safe fields.
- Check-in proof helper functions.
- Staff/scanner role.
- Refund-after-check-in policy.
- Transfer-after-check-in policy.
- Proof retention.

Check-in verifies entitlement. It is not payment processing and does not itself grant refund, dispute, or admin authority.

## 16. Capacity / Inventory / Expiry / Release Decision Model

Capacity decisions must cover:

- Ticket product capacity.
- Venue/session capacity.
- Exact seat, standing, section, table, or booth capacity.
- Reservation holds.
- Pending order holds.
- Abandoned checkout.
- Stale/expired order expiry.
- Refunded or cancelled tickets.
- Pending claim/gift/transfer state.

Decision required: capacity may be held by order, ticket, reservation, session, or another accepted entity. Release behavior must be backend-authoritative.

## 17. Host / Venue / Organizer Responsibility Decision Model

Host/venue decisions must cover:

- Host refund responsibility.
- Host event cancellation obligations.
- Venue reservation accuracy.
- Organizer dashboard sales visibility.
- Official business or venue claims.
- Host authority to cancel, refund, view, or correct records.
- Support escalation boundaries.

Host sales visibility is not refund/dispute authority. Venue ownership is not platform payment authority.

## 18. Support / Ops / Admin Commerce Process Decision Model

Support/admin commerce decisions must cover:

- Refund and dispute intake.
- Admin overrides.
- Manual paid confirmation, if any.
- Ticket correction.
- Reservation correction.
- Claim/transfer fixes.
- Fraud or safety review.
- Audit logs.
- Support SLA and escalation.

Revenue-sensitive support/admin actions must be explicit, backend-authoritative, and auditable before any process copy or implementation is accepted.

## 19. Revenue / Audit / Retention Decision Model

Revenue retention decisions must cover:

- Commerce record retention.
- Provider payload retention and minimization.
- Receipts, invoices, and confirmations.
- Payment attempts.
- Refunds and disputes.
- Ticket entitlement history.
- Reservation history.
- Support/admin actions.
- Audit logs.

PP-03 constraints apply: account deletion must not blindly delete commerce/payment/ticket records until retention exceptions are accepted.

## 20. Public Web / Share / Checkout / Wallet Copy Contract

Commerce copy must match the accepted contract for:

- Public event price display.
- Checkout refund copy.
- Wallet ticket status.
- QR/check-in text.
- Claim/share links.
- Public ticket verification.
- Confirmation copy.
- Cancellation/refund support copy.
- Host/venue responsibility copy.

Checkout copy is high-salience purchase-moment copy. It must not conflict with terms, support copy, host policy, or backend behavior.

## 21. Backend / RPC / RLS Verification Dependencies

Function families requiring PP-01 verification:

- `purchase_event_ticket_v3/v4/v5`.
- `create_commerce_order_v1`.
- `create_ticket_order_v1`.
- `mark_order_paid_v1`.
- `confirm_order_payment_v1`.
- `_issue_tickets_from_order_v1`.
- `expire_stale_orders_v1`.
- `request_ticket_v2`.
- `create_reservation_v1/v2`.
- `create_ticket_claim_v1`.
- `claim_ticket_v1`.
- `transfer_gift_ticket_v1`.
- `checkin_ticket_by_id_v2`.
- `staff_checkin_ticket_v1`.
- `ensure_ticket_checkin_proof_v1`.
- `record_checkin_proof_v1`.
- `remove_ticket_checkin_proof_v1`.

Table groups requiring PP-01 verification:

- `tickets`.
- `reservations`.
- `venue_reservations`.
- `event_ticket_claims_v1`.
- `commerce_orders`.
- `payment_attempts`.
- `provider_event_log`.
- `event_ticket_products_v1`.
- `event_sessions_v1`.
- `event_staff_assignments`.
- `events`.
- `venues`.

Verification must cover existence, active/legacy status, signatures, bodies, grants, security mode, `search_path`, RLS, direct policies, caller model, and denial paths. No SQL is included here.

## 22. Payment Provider / Webhook Verification Dependencies

Facts needed before provider/payment claims:

- Active provider yes/no.
- Provider environment/sandbox/production status.
- Webhook deployed yes/no.
- Webhook signature verification yes/no.
- Provider payload retention and minimization.
- Refund API available yes/no.
- Dispute/chargeback signal handling.
- Manual confirmation path.
- Receipt/invoice/confirmation path.
- Service-role usage boundaries without exposing secrets.

PP-04 does not verify the provider and does not inspect secrets.

## 23. Commerce Data Domain Inventory Matrix

| Domain | Example data | User expectation | Contract decision needed | Legal/product review need | PP-01 evidence need | PP-03 retention dependency | Later pack dependency |
|---|---|---|---|---|---|---|---|
| tickets | Holder, status, QR/check-in | Valid entry entitlement | Entitlement creation/revocation | High | Ticket RLS/RPC | Retention/redaction | PP-05 / PP-08 |
| ticket products | Price, capacity, section | Accurate purchasable product | Product/capacity authority | High | Product/session evidence | Product retention | PP-05 |
| sessions | Event sessions/capacity | Correct availability | Session inventory model | Medium/High | Session table/RPC evidence | Event retention | PP-05 |
| reservations | Event reservation state | Booking/request status | Paid vs unpaid semantics | High | Reservation evidence | Reservation retention | PP-04 / PP-05 |
| venue reservations | Table/booth/venue booking | Venue booking clarity | Venue commerce boundary | High | Venue reservation evidence | Venue retention | PP-04 |
| claims | Pending claim tokens | Claim ticket safely | Claim entitlement model | High | Claim RPC/table evidence | Claim retention | PP-05 |
| gift transfers | Sender/recipient transfer | Transfer valid ticket | Financial vs holder owner | High | Transfer evidence | Transfer retention | PP-10 if messaging used |
| commerce_orders | Order amount/status | Payment state reliable | Order authority | Very high | Order RLS/RPC | Commerce retention | PP-04 |
| payment_attempts | Provider attempt/ref | Payment processing trace | Attempt state and retry | High | Table/provider evidence | Provider retention | PP-04 |
| provider_event_log | Provider callback payload | Accurate payment updates | Provider authority/minimization | Very high | Provider/webhook evidence | Provider payload retention | PP-04 |
| receipts/invoices/notifications | Confirmation records | Proof of purchase | Receipt semantics | High | Notification/provider evidence | Record retention | PP-06 |
| check-in proof | Proof records/status | Entry verified | Proof authority/retention | High | Check-in proof evidence | Proof retention | PP-08 |
| event_staff_assignments | Scanner/manager authority | Staff can verify entry | Staff scope | High | Staff RLS/RPC | Staff retention | PP-08 |
| host sales dashboard data | Sales/ticket stats | Host sees event sales | Visibility scope only | Medium/High | Direct/RLS evidence | Host data retention | PP-08 |
| support/admin commerce logs | Overrides, refunds, disputes | Auditable support actions | Admin authority/audit | Very high | Admin/log evidence | Audit retention | PP-08 |
| public claim/share/ticket verification pages | Public-safe status | Link works without private leakage | Public field contract | High | Public route evidence | Public suppression | PP-05 |

## 24. Policy-to-Contract Mismatch Register

| Copy/policy signal | Missing commerce contract decision | Risk | Owner | Recommended next action |
|---|---|---|---|---|
| All-sales-final vs host refund policy | Canonical refund policy and host role. | Revenue-sensitive, legal/compliance-sensitive | Legal + commerce + product | Decide single accepted refund contract before final copy. |
| Payment provider disabled/unknown vs paid-ticket UI | Paid commerce launch scope. | Revenue-sensitive | Product + commerce | Verify provider state and decide launch scope. |
| Instant ticket vs pending payment/order state | Entitlement issuance moment. | Revenue-sensitive | Commerce + product | Decide whether tickets issue only after paid confirmation. |
| Reservation wording vs paid booking ambiguity | Reservation commerce boundary. | Product correctness | Product + venue + legal | Decide request vs paid booking language. |
| Refund after check-in unknown | Check-in/refund interaction. | Revenue-sensitive | Commerce + legal | Decide checked-in refund/cancel policy. |
| Transfer/gift payment history unknown | Financial owner vs entitlement holder. | Revenue-sensitive | Commerce + product | Decide transfer retention and refund owner. |
| Support refund/dispute process unknown | Support authority and SLA. | Operational/admin-sensitive | Support + legal + ops | Define support process before copy promises. |
| Receipt/invoice semantics unknown | Whether confirmations are legal/financial receipts. | Legal/compliance-sensitive | Legal + commerce | Decide receipt/invoice wording after provider evidence. |

## 25. Implementation-without-Commerce-Contract Register

| Existing technical/product surface | Missing commerce/refund/payment contract | Risk | Owner | Recommended next action |
|---|---|---|---|---|
| Purchase RPC versions | Canonical active purchase path. | Revenue-sensitive | Commerce | Verify and select canonical path. |
| Commerce order functions | Order state machine and authority. | Revenue-sensitive | Commerce | Define order contract. |
| Mark/confirm payment functions | Payment authority and caller model. | Revenue-sensitive, security-sensitive | Commerce + provider | Verify provider/manual authority. |
| Issue tickets helper | Entitlement issuance conditions. | Revenue-sensitive | Commerce | Define paid/free issuance moment. |
| Stale order expiry | Capacity release timing. | Product correctness, revenue-sensitive | Commerce | Define expiry/release model. |
| Claims/gifts/transfers | Entitlement and financial history semantics. | Revenue-sensitive | Product + commerce | Define transfer contract. |
| Check-in proof helpers | Proof retention and refund interaction. | Revenue-sensitive | Staff/host + commerce | Define check-in contract. |
| Reservations | Paid/unpaid boundary and cancellation. | Product correctness | Product + venue | Define reservation contract. |
| Provider event log | Provider authority and payload retention. | Revenue-sensitive, privacy-sensitive | Commerce + legal | Verify provider behavior and retention. |
| Admin/support transfer tools | Revenue-sensitive override authority. | Operational/admin-sensitive | Ops + support | Define audit and authorization. |
| Dashboard sales visibility | Host visibility vs mutation authority. | Privacy/revenue-sensitive | Dashboard/product | Verify scope and avoid refund authority drift. |

## 26. PP-01 Evidence Dependencies

PP-04 requires PP-01 evidence for:

- Production RPC existence, signatures, bodies, grants, security mode, and `search_path`.
- RLS and direct policies for commerce tables.
- Provider/webhook deployment state.
- Payment, refund, dispute, and chargeback actual state.
- Order/ticket issuance authority.
- Capacity hold/release behavior.
- Public claim/share/check-in route backend dependencies.
- Support/admin commerce mutation functions.
- Diagnostics/audit logs for commerce events.
- Production migration provenance for commerce-related functions and tables.

## 27. PP-02 Policy Copy Dependencies

PP-04 must respect PP-02 constraints:

- Checkout copy must match terms and refund policy.
- Host refund policy text requires accepted process and authority.
- Support copy must not overpromise refunds, disputes, chargebacks, or escalation.
- Public price, ticket, reservation, wallet, claim, and verification copy must match product behavior.
- Legal review is needed for payment/refund/dispute wording.
- No final legal payment copy should be treated as approved until legal owner approval.

## 28. PP-03 Retention / Data Request Dependencies

PP-04 must respect PP-03 constraints:

- Commerce, order, payment, ticket, reservation, claim, and provider records may require retention exceptions.
- Provider payloads should be minimized and redacted where appropriate.
- Account deletion must not blindly delete commerce records.
- Data export scope for tickets, orders, payments, claims, and reservations must be decided.
- Support/admin audit logs may require retention.
- Refund/dispute evidence may require retention.

## 29. Product Decision Dependency Checklist

- Paid commerce launch scope.
- Free/request-ticket launch scope.
- Canonical purchase/order/payment path.
- Entitlement issuance moment.
- Refund policy.
- Dispute/chargeback stance.
- Event cancellation behavior.
- Reservation cancellation behavior.
- Capacity hold/release model.
- Claim/gift/transfer semantics.
- Check-in/refund interaction.
- Host refund responsibility.
- Support/admin override powers.
- Beta vs public launch scope.

## 30. Legal Review Dependency Checklist

- Refund, cancellation, and dispute policy.
- Chargeback handling.
- Consumer purchase copy.
- Payment provider terms.
- Receipt/invoice wording.
- Host refund responsibility.
- Event cancellation obligations.
- Retention exceptions for commerce/payment records.
- Minors/guardian purchase implications if applicable.
- Support escalation/contact copy.
- Public marketing claims around payments and tickets.

## 31. Risk Priority Matrix

| Priority candidate | Items | Rationale |
|---|---|---|
| Candidate P0 | None assigned by this pack. | Current handbook evidence does not support P0 without production/provider/legal verification. |
| Candidate P1 | Refund copy contradiction; canonical paid-commerce path; provider/webhook unknown; entitlement issuance authority; commerce RLS/RPC verification; support/admin override auditability. | These affect revenue, public purchase promises, and entitlement correctness. |
| Candidate P2 | Disputes/chargebacks; receipts/invoices; claim/gift/transfer semantics; stale order expiry/capacity release; reservation paid/unpaid boundary. | Important beta/pre-scale contract decisions. |
| Candidate P3 | Copy polish and documentation after decisions. | Lower-risk once P1/P2 decisions are accepted. |
| Unknown / Needs verification | Active provider, webhook, refund/dispute support, paid-commerce launch scope, production RPC bodies, table policies. | Do not convert to patch work before PP-01 and owner decisions. |

## 32. Recommended Decision Records

- Commerce Launch Scope Decision.
- Canonical Purchase / Order / Payment Path Decision.
- Ticket Entitlement Issuance Decision.
- Refund / Cancellation / Dispute Policy Decision.
- Reservation Commerce Boundary Decision.
- Claim / Gift / Transfer Entitlement Decision.
- Check-In / Refund Interaction Decision.
- Commerce Retention Exception Decision.
- Support/Admin Commerce Authority Decision.

## 33. Dependency Map to Later Patch Plan Groups

PP-04 depends on PP-01, PP-02, and PP-03.

| Later pack | PP-04 dependency |
|---|---|
| PP-05 Public Visibility Suppression Pack | Public claim/share/check-in/ticket visibility, cancelled/refunded/private event exposure. |
| PP-06 Notification/Diagnostics Privacy Pack | Purchase, cancellation, refund, claim, reminder, receipt/confirmation notification semantics and commerce diagnostics. |
| PP-07 Abuse/Moderation Workflow Pack | Fraud/safety commerce review, report-driven takedown/refund implications. |
| PP-08 Ops/Admin Support Auditability Pack | Support/admin refund, dispute, correction, override, and audit authority. |
| PP-09 Media Storage Lifecycle Pack | Commerce proof/media receipt artifacts if any exist. |
| PP-10 Messaging Privacy Lifecycle Pack | Commerce notifications/deep links or support messages touching DMs, if adopted. |

## 34. PP-04 Output Artifacts

Recommended artifacts after execution, not created now:

- `CommerceLaunchScopeDecision.md`
- `CanonicalCommerceContractDecision.md`
- `RefundCancellationDisputePolicyDecision.md`
- `PaymentProviderVerificationReport.md`
- `CommerceRpcRlsVerificationReport.md`
- `TicketEntitlementLifecycleDecision.md`
- `ReservationCommerceBoundaryDecision.md`
- `CommerceRetentionExceptionDecision.md`
- `CommerceImplementationReadinessChecklist.md`

## 35. Execution Preconditions

Before executing PP-04:

- Product owner assigned.
- Legal owner assigned.
- Commerce/payment owner assigned.
- Support/admin owner assigned.
- PP-01 production evidence available where needed.
- PP-02 copy constraints available.
- PP-03 retention/data-request constraints available.
- Launch scope defined.
- No production changes planned as part of decision work.
- No payment/refund execution.
- No provider secrets exposed.
- No final legal claims made.
- Sanitized evidence rules accepted.

## 36. Explicitly Blocked Actions

PP-04 blocks:

- Payment execution.
- Refund execution.
- Dispute or chargeback action.
- Provider activation.
- Webhook deployment.
- Ticket issuance.
- Reservation mutation.
- Production access.
- SQL or Supabase CLI.
- Migrations.
- Source code changes.
- Provider secret exposure.
- Policy publication.
- Legal compliance claims.
- Support refund SLA commitments.
- Immediate patch authorization.

## 37. Unknown / Needs Verification Items

- Whether paid commerce is in launch scope.
- Whether `COMMERCE_PAYMENT_ENABLED=false` reflects current deployed behavior.
- Which provider, if any, is active.
- Whether any provider webhook is deployed.
- Whether provider signature verification exists.
- Whether refunds, disputes, or chargebacks are supported.
- Whether receipt/invoice behavior exists.
- Which purchase/order/payment RPC path is canonical.
- Whether pending/unpaid orders can create tickets.
- How stale orders release capacity.
- How reservation cancellation interacts with money.
- How claim/gift/transfer affects payment history.
- Whether checked-in tickets can be refunded/cancelled/transferred.
- Who owns support/admin commerce requests.

## 38. Acceptance Criteria for PP-04 Completion

PP-04 is complete only when:

- Commerce surface inventory is confirmed.
- Canonical commerce flow option is accepted or explicitly deferred.
- Paid commerce launch scope is accepted or explicitly deferred.
- Refund/cancellation/dispute stance is accepted or explicitly deferred.
- Ticket entitlement issuance model is accepted or explicitly deferred.
- Reservation/claim/transfer semantics are accepted or explicitly deferred.
- Capacity hold/release decision is accepted or explicitly deferred.
- PP-01 evidence dependencies are linked.
- PP-02 copy constraints are linked.
- PP-03 retention constraints are linked.
- Product owner decisions are assigned.
- Legal review dependencies are assigned.
- Commerce/payment owner is assigned.
- Support/admin process owner is assigned.
- Follow-up PP-05 through PP-10 groups are updated or explicitly marked unchanged based on commerce/refund/payment decisions.
- No final legal/payment/refund text is treated as approved unless the legal owner confirms it.

## 39. Recommended Follow-Up Reports

Recommended follow-up reports after execution:

- Commerce Launch Scope Decision.
- Canonical Commerce Contract Decision.
- Payment Provider Verification Report.
- Commerce RPC/RLS Verification Report.
- Refund / Cancellation / Dispute Policy Decision.
- Ticket Entitlement Lifecycle Decision.
- Reservation Commerce Boundary Decision.
- Commerce Retention Exception Decision.
- Support/Admin Commerce Authority Review.
- Commerce Implementation Readiness Checklist.

## 40. Non-Goals

- No code changes.
- No SQL or migrations.
- No production execution.
- No payment execution.
- No refund execution.
- No dispute or chargeback action.
- No provider activation.
- No webhook deployment.
- No ticket or reservation mutation.
- No legal advice.
- No compliance claim.
- No launch readiness claim.
- No final legal/refund/payment copy.
- No immediate patch authorization.
- No source-code re-audit.

## 41. Open Questions

- Is paid commerce in launch scope?
- Is free/request-ticket only acceptable for beta?
- Which purchase/order/payment RPC path is canonical?
- When is a ticket entitlement created?
- Can unpaid or pending orders create entitlement?
- What is the canonical refund policy?
- Are host refunds allowed?
- Who can approve refunds?
- What happens after event cancellation?
- What happens after check-in?
- Are disputes or chargebacks handled?
- Are receipts or invoices required?
- Are reservations paid bookings or requests?
- How is capacity held and released?
- How are gift, claim, and transfer payment histories handled?
- Which provider or webhook is active?
- Who owns support/admin commerce requests?

## 42. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No payment/refund/dispute/provider action was executed.
- No files were staged or committed.
- Only `08_PatchPlans/PP04CommerceRefundPaymentContractPack.md` was created/modified.

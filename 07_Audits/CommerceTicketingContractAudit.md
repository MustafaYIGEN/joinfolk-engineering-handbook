# Commerce + Ticketing Contract Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false

## 2. Purpose

This document is a commerce and ticketing contract audit for JoinFolk. It maps observed ticketing, order, reservation, claim, transfer, buyer, and participation flows against the expected product contract and authority boundary.

This is not a patch plan, cleanup plan, migration plan, or implementation plan. It does not authorize adding, removing, or changing features. It does not authorize modifying backend/RPC/RLS/storage/auth, any Supabase tree, or application code.

## 3. Audit Scope

Read-only evidence was drawn from the engineering handbook and local source trees for the handbook, platform/backend, web/dashboard, and mobile app. The audit focused on commerce and participation surfaces:

- Event commerce mode: none, ticket sales, reservation, and conflict/illegal dual mode.
- Ticket products, orders, checkout, wallet, gift claims, transfers, reservations, venue reservations, and check-in participation dependencies.
- Backend Database Functions / RPCs, table/RLS reliance, direct table access, and UI/helper gates.
- Production SQL/RPC evidence from prior operator-supplied verification reports.

Current system context:

- Future accepted Supabase migration target: `C:\dev\hostos\supabase\migrations`.
- This target is not proof of historical sole canonical source.
- Split-source migration history remains unresolved.
- Production SQL/RPC evidence remains stronger than local source assumptions.
- Database Functions / RPC evidence is separate from Edge Function deployment evidence.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- No backend patch or migration is authorized by this audit.

## 4. Commerce Contract Summary

JoinFolk commerce appears to support three product-level participation modes:

- No commerce: event participation, discovery, social sharing, and non-paid attendance behavior.
- Ticket sales: host-configured ticket products, buyer purchase flow, ticket wallet, gift/claim/transfer support, and check-in eligibility.
- Reservations: event reservation requests/approvals and venue/service reservations, distinct from ticket wallet/check-in semantics.

The clean commerce contract should be backend-authoritative for revenue, entitlement, capacity, order/payment, reservation, wallet ownership, claim/transfer, and check-in eligibility decisions. UI helpers should guide setup and display state, but they should not be the authority for product legality, capacity, max-per-buyer, entitlement conflict prevention, seat/standing availability, or mutation permission.

Observed implementation reality is mostly backend-oriented, but not fully deterministic. The strongest positive signals are production RLS enabled on target sensitive tables, RPC-first mobile ticket/wallet paths, production entitlement guard architecture, and current scanner RPC positive controls. The main uncertainty is source-of-truth clarity across versioned and overloaded RPCs, split commerce-mode vocabulary, direct table access in some UI paths, and incomplete proof that every acquisition path uses the same entitlement conflict guards.

## 5. Active Commerce Flow Map

| Flow | Observed surfaces | Backend/RPC candidates | Contract status | Notes |
| --- | --- | --- | --- | --- |
| Commerce mode setup | Dashboard module/product setup; mobile commerce mode helper | `get_event_modules_v1`, `set_event_modules_v1`, `clear_event_module_v1`, local `_event_commerce_mode_v1` references | Split / Needs verification | Mobile names dual mode `illegal_dual_mode`; dashboard names it `conflict`. |
| Ticket product management | Dashboard Products page; mobile ticket product wrappers | `get_event_ticket_products_v1`, `get_event_ticket_product_v1`, `upsert_event_ticket_product_v1`, `upsert_event_ticket_product_v2`, `set_event_ticket_product_active_v1` | Mostly deterministic | Product fields include price, currency, capacity, max per buyer, giftability, transferability, category, section keys, and standing/seat mapping signals. |
| Ticket purchase | Mobile buyer and ticket-sales APIs | `purchase_event_ticket_v2`, `purchase_event_ticket_v3`, `purchase_event_ticket_v4`, `purchase_event_ticket_v5`, `create_commerce_order_v1`, `create_ticket_order_v1` | Split / duplicated | v5 is described locally as replacing overloaded v4 to avoid ambiguous function resolution, but production active path needs confirmation. |
| Order/payment lifecycle | Commerce/order RPC surface from backend audits | `create_commerce_order_v1`, `mark_order_paid_v1`, `confirm_order_payment_v1`, `_issue_tickets_from_order_v1`, `expire_stale_orders_v1` | Backend authority unclear | Revenue-sensitive; active orchestration and payment authority need focused review. |
| Event reservations | Mobile reservation API; dashboard reservation pages | `create_reservation_v1`, `create_reservation_v2`, `cancel_reservation_v1`, `get_my_reservations_v1`, `get_event_reservations_v1`, `update_reservation_status_v1` | Mostly deterministic | Mobile source explicitly treats reservations as separate from tickets, QR scanning, and check-in proof. |
| Venue reservations | Mobile venue reservation v2 API; dashboard venue reservation status paths | `get_venue_availability_v2`, `create_venue_reservation_v1`, `create_venue_reservation_v2`, `decide_venue_reservation_v2`, `update_venue_reservation_status_v1`, `list_my_venue_reservations_v1`, `get_venue_reservations_v1` | Split / Needs verification | Event reservations and venue/service reservations need an explicit product contract boundary. |
| Gift claims and transfers | Mobile claim/transfer API and wallet handoff | `create_ticket_claim_v1`, `claim_ticket_v1`, `get_my_ticket_claims_v1`, `get_my_pending_gift_claims_v1`, `get_claim_preview_v1`, `transfer_gift_ticket_v1`, `revoke_ticket_claim_v1` | Mostly deterministic | Production guard helpers include pending claim and gift entitlement checks, but coverage across all paths remains incomplete. |
| Wallet and ownership truth | Mobile wallet/ticket detail APIs | `get_my_tickets_v2`, `get_my_event_ticket_v1`, `get_my_ticket_by_id_v1`, `get_ticket_by_id_v2`, `has_event_entitlement_v1`, `user_has_ticket` | Mostly deterministic | Local mobile wrapper explicitly avoids direct ticket fallback for ticket detail and uses RPC as canonical path. |
| Seat/section/standing buyer flow | Mobile seating API; dashboard product-section mapping; venue layout editor dependency | `get_event_seat_availability_v1`, `get_event_product_section_usage_v1`, `_validate_exact_seat_purchase_v1`, `get_buyer_venue_zones_v1`, `update_event_seating_config_v1`, `upsert_event_ticket_product_v2` | Split / UI-heavy | UI does significant validation; backend parity for all constraints needs verification. |
| Check-in participation dependency | Mobile scanner and ticket APIs; dashboard queue/check-in | `checkin_ticket_by_id_v2`, `staff_checkin_ticket_v1`, `checkin_ticket_v2`, `check_in_ticket`, `get_event_ticket_queue_v2` | Mostly deterministic for current scanner; legacy unclear | Current by-id scanner RPC has positive controls; legacy/proof paths need separate review. |

## 6. Commerce Authority Matrix

| Domain | Flow / action | Current observed surface | Expected authority owner | Active RPC / data path candidates | Determinism status | Risk class | Evidence source | Recommendation |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Commerce mode | Decide ticket sales vs reservation vs none | Dashboard and mobile helpers mirror module state with different conflict names | Backend/RPC plus UI mirror | Module RPCs; local `_event_commerce_mode_v1` references | Split / duplicated | Revenue-sensitive, product correctness | Mobile/dashboard helpers; Product Contract Audit | Document canonical contract |
| Ticket products | Create/update active products, pricing, capacity, category, giftable/transferable flags | Dashboard product manager; mobile wrappers | Backend/RPC/RLS/auth | `upsert_event_ticket_product_v2`, product read RPCs | Mostly deterministic | Revenue-sensitive | Dashboard/mobile source; backend provenance | Preserve, then reconcile versions |
| Ticket capacity | Prevent oversell and capacity below assigned/sold count | UI validation plus backend guard migrations | Backend/RPC | Purchase RPCs, `_issue_tickets_from_order_v1`, exact-seat guard | Backend authority unclear | Revenue-sensitive | Dashboard Products page; backend audit summaries | Harden later after coverage review |
| Purchase | Buyer purchases one or more products/seats/areas | Mobile buyer/ticket-sales APIs | Backend/RPC/payment authority | `purchase_event_ticket_v5`, older purchase RPCs, order RPCs | Split / duplicated | Revenue-sensitive | Mobile source; production parity reports | Reconcile active purchase path |
| Orders/payment | Create order, mark/confirm paid, issue tickets, expire stale orders | Backend RPC inventory; partial app source evidence | Backend/RPC and payment provider authority | `create_commerce_order_v1`, `mark_order_paid_v1`, `confirm_order_payment_v1`, `_issue_tickets_from_order_v1`, `expire_stale_orders_v1` | Unknown / Needs verification | Revenue-sensitive, security-sensitive | Handbook/backend audits | Focused audit needed |
| Event reservations | Create/cancel/update event reservation | Mobile and dashboard RPC calls | Backend/RPC/RLS/auth | `create_reservation_v2`, `cancel_reservation_v1`, `update_reservation_status_v1` | Mostly deterministic | Product correctness, privacy-sensitive | Mobile/dashboard source; production RLS evidence | Preserve, document contract |
| Venue reservations | Book/approve venue offerings | Mobile v2 offering-aware API and dashboard v1 status paths | Backend/RPC/RLS/auth | `create_venue_reservation_v2`, `decide_venue_reservation_v2`, `update_venue_reservation_status_v1` | Split / Needs verification | Revenue-sensitive, operational-sensitive | Mobile/dashboard source | Reconcile v1/v2 contract |
| Gift claims/transfers | Create, preview, claim, revoke, transfer gift ticket | Mobile claim/transfer API; backend guards | Backend/RPC/RLS/auth | Claim/transfer RPC family and entitlement helpers | Mostly deterministic | Revenue-sensitive, privacy-sensitive | Mobile source; focused backend report | Verify guard coverage |
| Entitlement conflicts | Prevent ticket/reservation/gift/pending claim overlap | Production helper architecture present | Backend/RPC/RLS/auth | `_assert_narrow_ticket_acquisition_conflict_v1` and helper family | Mostly deterministic but incomplete | Revenue-sensitive | Focused backend report | Focused body/overload review |
| Wallet ownership | Show and validate owned tickets | Mobile wallet RPCs | Backend/RPC/RLS/auth | `get_my_tickets_v2`, `get_ticket_by_id_v2`, ticket ownership helpers | Mostly deterministic | Privacy-sensitive, product correctness | Mobile source; production RLS evidence | Preserve RPC-first contract |
| Seat/standing availability | Reserve exact seats or standing sections | Mobile seating APIs; dashboard layout/product mapping | Backend/RPC plus UI mirror | Seat availability, product-section usage, exact-seat validation RPCs | Split / UI-heavy | Revenue-sensitive | Mobile/dashboard source; provenance report | Venue buyer audit |
| Check-in eligibility | Validate ticket, event, status, code, staff/host authority | Current scanner RPC; legacy paths exist | Backend/RPC/RLS/auth | `checkin_ticket_by_id_v2`, `staff_checkin_ticket_v1`, legacy check-in RPCs | Mostly deterministic for current path | Security-sensitive, product correctness | Focused backend report; mobile source | Preserve current path, review legacy |
| Direct ticket mutation | Approve/reject/remove ticket status in dashboard local API | Dashboard direct `tickets` updates observed | Backend/RPC/RLS/auth | Direct `tickets` data path plus v2 approval RPCs | Backend authority unclear | Security-sensitive, revenue-sensitive | Dashboard source; production RLS evidence | Reconcile to canonical authority |

## 7. Ticket Product Contract Assessment

Expected clean contract:

- Hosts can define active/inactive ticket products with title, price, currency, capacity, max per buyer, giftability, transferability, category, and optional section/standing constraints.
- Product state must be validated by backend authority before it can affect checkout, buyer availability, or wallet issuance.
- UI may provide previews and setup validation, but backend must enforce final product legality and capacity.

Observed evidence:

- Mobile ticket product wrappers expose v1/v2 product reads and mutations.
- Dashboard Products page manages product definitions, categories, buyer setup, section keys, price overrides, capacity, and active state.
- Dashboard product setup includes UI-level validation for stale/orphan section keys and capacity below allocated/checked-in usage.
- Backend migration provenance identifies hostos as strongest commerce/entitlement source, including ticket product, exact-seat, eligibility, and standing-ticket migrations.

Assessment:

- Product status: Product-critical.
- Determinism status: Mostly deterministic, with split UI/backend validation and versioned RPCs.
- Authority owner: Backend/RPC/RLS/auth should be authoritative; dashboard/mobile are mirrors and editing surfaces.
- Recommendation: Preserve the product model, document the canonical product contract, and verify that backend product mutation/purchase guards enforce the same invariants as dashboard validation.

## 8. Ticket Purchase / Order Contract Assessment

Expected clean contract:

- Purchase and order creation must be backend-authoritative.
- Price, currency, capacity, active state, max-per-buyer, section/seat/standing availability, entitlement conflict, and ticket issuance must not be trusted from the client.
- Payment confirmation and ticket issuance must have a single accepted source-of-truth path.

Observed evidence:

- Mobile exposes `purchase_event_ticket_v2`, `purchase_event_ticket_v3`, and `purchase_event_ticket_v5`.
- Prior backend reports identified `purchase_event_ticket_v4` overloads and order functions including `create_commerce_order_v1`, `create_ticket_order_v1`, `mark_order_paid_v1`, `confirm_order_payment_v1`, `_issue_tickets_from_order_v1`, and `expire_stale_orders_v1`.
- Local mobile source describes v5 as a basket purchase path intended to replace overloaded v4 and avoid ambiguous function resolution.
- Production parity evidence showed broad grants on some commerce/reservation RPCs and incomplete body review for entitlement guard coverage.

Assessment:

- Commerce status: Active core path / Product-critical.
- Determinism status: Split / duplicated.
- Authority owner: Backend/RPC/RLS/auth.
- Risk class: Revenue-sensitive and security-sensitive.
- Recommendation: Reconcile active purchase/order source of truth before any patching. Do not assume v5 is the only production-active path until production RPC usage and overloads are confirmed.

## 9. Reservation Contract Assessment

Expected clean contract:

- Event reservations are distinct from paid ticket wallet ownership and should not imply ticket QR/check-in proof unless explicitly converted by backend authority.
- Reservation creation, cancellation, host status changes, capacity, and cross-entitlement conflicts must be backend-authoritative.
- Venue/service reservations should have a separate contract from event attendance reservations.

Observed evidence:

- Mobile `reservations.v1` explicitly states reservations are not tickets and do not carry QR/scanning/check-in proof.
- Event reservation RPCs include v1/v2 create paths, cancellation, host update, host list, and user list.
- Venue reservation v2 API is offering-aware and distinguishes instant vs request booking behavior.
- Dashboard has event reservation and venue reservation status surfaces.
- Production RLS evidence confirms `reservations` exists and RLS is enabled, with policy surface present.

Assessment:

- Commerce status: Product-critical.
- Determinism status: Mostly deterministic for event reservations; split for venue reservation v1/v2 and dashboard/mobile semantics.
- Authority owner: Backend/RPC/RLS/auth.
- Risk class: Product correctness, privacy-sensitive, and revenue-sensitive where offerings/payment are involved.
- Recommendation: Preserve reservation as a separate contract, document event vs venue reservation boundaries, and verify cross-entitlement guard coverage.

## 10. Gift Claim / Transfer Contract Assessment

Expected clean contract:

- Gift claims and transfers must preserve single-owner wallet truth.
- Pending claim, recipient entitlement, sender ownership, expiration, revoke, and transfer limits must be backend-authoritative.
- Caller-provided recipient/user identifiers cannot authorize ownership transfer by themselves.

Observed evidence:

- Mobile claim API exposes create, claim, preview, pending gifts, revoke, and transfer RPCs.
- Claim result types include gift limit, capacity, sold, pending gift, and remaining count fields.
- Focused backend report confirmed helper architecture for active reservation, gift entitlement, and pending recipient claim checks.
- Production evidence showed `event_ticket_claims_v1` exists with RLS enabled and zero direct policies, making RPC internal guards critical.

Assessment:

- Commerce status: Product-critical.
- Determinism status: Mostly deterministic, but full coverage across all claim/transfer/acquisition paths remains Unknown / Needs verification.
- Authority owner: Backend/RPC/RLS/auth.
- Risk class: Revenue-sensitive, privacy-sensitive, product correctness.
- Recommendation: Preserve gift/claim contract and perform focused guard coverage review before accepting any patch plan.

## 11. Entitlement Conflict Guard Assessment

Expected clean contract:

- A user should not be able to acquire incompatible attendance entitlements through alternate paths such as purchase, reservation, gift claim, pending claim, transfer, request/approval, or order issuance.
- The same backend guard policy should apply across active acquisition and ownership paths.

Observed evidence:

- Production focused verification confirmed live helper functions with search path configuration for core entitlement checks.
- `_assert_narrow_ticket_acquisition_conflict_v1` checks reservation, gift entitlement, and pending recipient claim helpers.
- `create_commerce_order_v1` appears to call the narrow acquisition guard for self or `p_for_user_id`.
- `create_ticket_claim_v1` references active entitlement and gift/reservation helper checks.
- `purchase_event_ticket_v4` had multiple overloads; one observed overload mentioned the guard and another did not.
- `purchase_event_ticket_v5` mentions the narrow guard.
- `request_ticket_v2` evidence was partial/truncated.

Assessment:

- Commerce status: Product-critical.
- Determinism status: Mostly deterministic architecture, incomplete path coverage.
- Authority owner: Backend/RPC/RLS/auth.
- Risk class: Revenue-sensitive and product correctness.
- Recommendation: Treat as Candidate P1 / Needs verification until every active acquisition path is mapped to guard coverage.

## 12. Seat / Section / Standing-Area Buyer Contract Assessment

Expected clean contract:

- Event seating mode, layout binding, ticket product section keys, standing areas, exact seat IDs, capacity, and availability must be enforced by backend authority.
- Dashboard/mobile buyer previews may help hosts configure products and buyers select seats, but cannot be the source of final availability truth.

Observed evidence:

- Mobile seating source models event-level seat selection, venue layout sections, section types, and availability states including available, sold, checked-in, gift-reserved, and blocked.
- Dashboard Products page maps products to sections, excludes non-mappable sections, sanitizes stale keys, and checks allocation/checked-in usage before capacity or section changes.
- Backend provenance identifies hostos commerce/venue migrations including exact-seat purchase guard, product section usage, ticket section context, and standing-ticket support.
- Product Contract and Authority Boundary audits flagged venue buyer flow as split and UI-heavy.

Assessment:

- Commerce status: Product-critical for ticketed venues; optional for simple general-admission events.
- Determinism status: Split / UI-heavy.
- Authority owner: Backend/RPC/RLS/auth for final availability; dashboard/mobile for editing and preview.
- Risk class: Revenue-sensitive and product correctness.
- Recommendation: Run a dedicated Venue Buyer Flow Contract Audit before changing this area.

## 13. Wallet / Ownership / Participation Truth Assessment

Expected clean contract:

- Wallet truth should come from backend-authoritative ticket ownership and status, not frontend reconstruction.
- Check-in eligibility should depend on backend-owned ticket status, event status, event scope, staff/host authority, and accepted scanner path.
- Public/share/claim handoff should not expose wallet or ownership details beyond product-approved views.

Observed evidence:

- Mobile uses wallet RPCs including `get_my_tickets_v2`, `get_my_event_ticket_v1`, and `get_ticket_by_id_v2`.
- Local mobile wrapper explicitly says ticket detail must not fall back to direct table query and should use the security-definer RPC as canonical path.
- Production evidence confirms `tickets` has RLS enabled and zero direct policies, implying direct table access is likely default deny while RPC internal guards are critical.
- Current scanner RPC `checkin_ticket_by_id_v2` has positive controls, including auth, host/status/ticket scope/code/state checks.

Assessment:

- Commerce status: Core MVP / Product-critical.
- Determinism status: Mostly deterministic for current mobile wallet and scanner paths; legacy/direct paths unclear.
- Authority owner: Backend/RPC/RLS/auth.
- Risk class: Security-sensitive, privacy-sensitive, and product correctness.
- Recommendation: Preserve RPC-first wallet and scanner contract, reconcile direct/legacy ticket paths.

## 14. Frontend UI Gate and Helper Map

| UI/helper surface | Observed role | Acceptable as UI guidance? | Risk if treated as authority | Recommendation |
| --- | --- | --- | --- | --- |
| Mobile commerce mode resolver | Mirrors module state and names dual mode `illegal_dual_mode` | Yes | Split vocabulary can hide backend/UI mismatch | Document canonical commerce mode vocabulary. |
| Dashboard commerce mode resolver | Mirrors module state and names dual mode `conflict` | Yes | Same logical state has different name from mobile | Reconcile naming. |
| Dashboard Products page validation | Validates product capacity, section keys, price overrides, buyer setup | Yes | Capacity/section correctness cannot rely on UI only | Verify backend parity. |
| Dashboard admission/readiness model | Guides host setup for general admission vs reserved layout | Yes | UI-derived model could diverge from backend publish/purchase readiness | Document as advisory unless backend confirms. |
| Mobile purchase wrappers | Call versioned purchase and product RPCs | Yes | Multiple versions can route users to non-canonical behavior | Reconcile active purchase path. |
| Mobile reservation wrappers | Encapsulate event reservation semantics | Yes | v1/v2 behavior could diverge if both remain active | Document active path. |
| Dashboard direct ticket status updates | Mutates ticket status via direct table data path in local source | No, unless RLS and product contract explicitly support it | May bypass canonical RPC semantics or fail under default-deny RLS | Reconcile to backend-authoritative path. |
| Public web claim/share handoff | Presents public/share and claim entry points | Yes for handoff | Validation must remain backend/app authoritative | Keep as public handoff, not authority. |

## 15. Backend RPC / RLS Authority Evidence Map

Production-backed evidence:

- Target sensitive tables including `tickets`, `reservations`, `commerce_orders`, `event_ticket_claims_v1`, `events`, and `event_staff_assignments` existed in production with RLS enabled.
- `commerce_orders` had a deny-all style authenticated policy in supplied production output.
- `tickets` and `event_ticket_claims_v1` had zero policies in focused output; with RLS enabled, direct table access is likely default deny, and RPC internal guards become critical.
- Entitlement guard helper architecture exists in production, but full active-path coverage is not confirmed.
- Multiple `create_reservation_v2` overloads and purchase/order variants support the source-of-truth clarity concern.
- Some commerce/reservation/ticket RPCs have broad execute grants; broad grants are not automatically exploitable if internal guards are correct, but they warrant review.

Local-source-only evidence:

- Mobile and dashboard clients call many RPCs by name and also include some direct table reads/writes.
- Hostos migrations are the strongest local source candidate for commerce/entitlement and standing-ticket provenance.
- Mobile migration history strongly implicates proof-specific helper provenance and some transfer/visual history.
- Joinfolk-web migration history strongly implicates host identity transfer / ops provenance.

Unknown / Needs verification:

- Which purchase/order path is production-current.
- Whether every acquisition path uses the same entitlement conflict guards.
- Whether dashboard direct ticket updates are active, reachable, or blocked by production RLS.
- Whether payment confirmation and ticket issuance are externally reachable, internal-only, or payment-provider-gated.
- Whether seat/standing/product-section backend validation fully mirrors UI validation.

## 16. Direct Data Access / RLS Reliance Map

| Data surface | Observed direct access relevance | Prior production authority evidence | Current classification | Recommendation |
| --- | --- | --- | --- | --- |
| `tickets` | Mobile reads in some areas; dashboard local API has direct status mutation paths; wallet uses RPCs | RLS enabled; zero direct policies in focused output | RLS authority likely default-deny; RPC guards critical | Reconcile direct access and prefer canonical RPC paths. |
| `event_ticket_claims_v1` | Claim/transfer backend surface | RLS enabled; zero direct policies | RPC-only reliance likely | Verify all claim/transfer guards. |
| `commerce_orders` | Order/payment lifecycle | RLS enabled; deny-all style policy | RPC-only reliance likely | Audit order/payment RPC authority. |
| `reservations` | Event reservations via RPC; possible direct list/display reads | RLS enabled; policy surface exists | RLS/storage authority likely | Review policy correctness and v1/v2 path ownership. |
| `venue_reservations` | Venue/service bookings | Production parity not fully summarized in target sensitive table set | Unknown / Needs verification | Dedicated venue reservation review. |
| `event_ticket_products_v1` | Product setup and buyer reads | Production RLS evidence not fully summarized | Unknown / Needs verification | Verify product read/write authority. |
| `events` | Commerce modules, lifecycle, seat selection mode, max buyer settings | RLS enabled; public/auth/host policies exist | RLS authority likely but policy correctness separate | Ensure revenue-relevant fields are backend-gated. |
| `venue_layouts` / layout snapshots | Dashboard product-section mapping and buyer layout dependencies | Not covered by supplied production RLS summary | Unknown / Needs verification | Venue Buyer Flow Contract Audit. |
| `event_sessions_v1` / seat/session metadata | Mobile buyer/seating flows may read session/availability metadata | Not covered by supplied production RLS summary | Unknown / Needs verification | Verify read/write authority before patching. |

## 17. Duplicated / Legacy / Overloaded RPC Surface

| RPC / surface | Observed role | Current/legacy/unknown | Risk if still callable or used | Evidence type | Recommendation |
| --- | --- | --- | --- | --- | --- |
| `purchase_event_ticket_v2` | Older mobile purchase path | Legacy / Unknown | May bypass newer guard or product-section behavior | Local source | Review active usage. |
| `purchase_event_ticket_v3` | Older multi-product/seat-aware purchase path | Legacy / Unknown | May differ from v5 semantics | Local source | Review active usage. |
| `purchase_event_ticket_v4` | Production-observed overloaded purchase path | Unknown / overloaded | One overload may lack visible narrow guard; ambiguous active body | Production focused evidence | Candidate P1 source-of-truth review. |
| `purchase_event_ticket_v5` | Mobile-described basket purchase replacement for v4 | Active candidate | Must be confirmed as production-current before relying on it | Local source + focused evidence | Preserve candidate, verify production usage. |
| `create_ticket_order_v1` | Ticket order creation | Unknown | Competes with commerce order path | Backend inventory | Reconcile order contract. |
| `create_commerce_order_v1` | Commerce order creation | Active candidate | Broad grants and guard coverage need review | Production evidence | Verify authority and current path. |
| `mark_order_paid_v1` / `confirm_order_payment_v1` | Payment confirmation | Unknown | Revenue-sensitive if callable without accepted payment authority | Backend inventory | Focused payment/order audit. |
| `_issue_tickets_from_order_v1` | Ticket issuance from order | Unknown/internal candidate | Revenue-sensitive if externally executable or weakly gated | Backend inventory | Verify execute grants and caller path. |
| `expire_stale_orders_v1` | Order cleanup | Unknown | Product correctness and inventory release risk | Backend inventory | Verify lifecycle contract. |
| `create_reservation_v1` | Legacy reservation creation | Legacy / Unknown | May lack v2 guest/session/guard behavior | Local source | Prefer v2 if accepted. |
| `create_reservation_v2` | Event reservation creation | Active candidate; multiple overloads | Source-of-truth ambiguity | Production evidence | Reconcile overloads. |
| `create_venue_reservation_v1` / `create_venue_reservation_v2` | Venue booking | Split v1/v2 | Event/venue reservation semantics can diverge | Local source | Document active path. |
| `approve_ticket_v2` / `approve_ticket_by_id_v2` | Ticket approval | Duplicated | Host queue semantics can diverge | Backend/app source | Reconcile approval contract. |
| `reject_ticket_v2` / `reject_ticket_by_id_v2` | Ticket rejection | Duplicated | Host queue semantics can diverge | Backend/app source | Reconcile rejection contract. |
| Dashboard direct `tickets` update | Ticket status mutation | Unknown / concerning | Could bypass RPC semantics or be blocked by RLS | Local source + production RLS evidence | Needs authority decision. |
| `upsert_event_ticket_product_v1` / `upsert_event_ticket_product_v2` | Product mutation | v2 active candidate | Product validation may diverge | Local source | Reconcile to one contract. |
| `check_in_ticket`, `checkin_ticket_v2`, `checkin_ticket_by_id_v2` | Check-in | Current plus legacy | Legacy paths may differ from current scanner authority | Focused backend report | Preserve by-id path, review legacy. |

## 18. Revenue-Sensitive Invariants

The following invariants should be backend-authoritative:

- An event cannot be in ticket-sales and reservation-only commerce modes at the same time unless the backend explicitly defines a conflict state that blocks purchase/reservation mutation.
- Ticket products cannot be purchased if inactive, outside allowed sale windows, over capacity, over max-per-buyer, mismatched to section/standing/seat constraints, or incompatible with event lifecycle.
- Price, currency, product ID, section key, seat ID, quantity, session ID, user ID, and buyer/recipient identity cannot be trusted from client input without backend validation.
- Payment confirmation cannot issue tickets unless an accepted payment/order authority has verified the order state.
- A user cannot hold incompatible entitlements through purchase, reservation, gift claim, pending claim, transfer, request, or approval paths.
- A ticket cannot be checked in unless event, ticket, owner/status, code, scanner/host/staff authority, and state transition rules are satisfied.
- Direct table access must not be the authority for tickets, claims, orders, reservations, or product mutations unless RLS and policies explicitly encode the same contract.
- Seat and standing availability must be computed from backend-owned ticket/order/claim/check-in state, not from UI snapshots alone.

## 19. Missing / Incomplete Commerce Feature Candidates

These are not accepted product gaps or approved feature work. They are candidates requiring product and authority decisions:

- Accepted commerce-mode vocabulary and state machine across mobile, dashboard, backend, and public surfaces.
- Single accepted purchase/order path, including whether v5 is current and whether v4/v3/v2 remain supported.
- Payment/order lifecycle contract for create, confirm, issue, expire, refund/cancel if present.
- Event reservation vs venue reservation boundary and how each interacts with entitlement/wallet/check-in.
- Gift claim and transfer contract for pending claims, sender/recipient ownership, expiration, revoke, and conflict prevention.
- Venue buyer flow contract for exact seats, standing areas, section mappings, and product-section usage.
- Direct dashboard ticket status mutation policy: accepted direct RLS path or deprecated in favor of RPC.
- Legacy ticket request/approval and check-in path classification.

## 20. Commerce Gaps / Risk Register Seeds

### CTC-GAP-001

- Domain: Commerce mode
- Current issue: Mobile and dashboard represent dual ticket/reservation enablement with different names: `illegal_dual_mode` vs `conflict`.
- Expected clean commerce contract: One backend-defined commerce mode vocabulary used consistently by UI mirrors.
- Risk: Product correctness and revenue-sensitive setup drift.
- Priority candidate: Candidate P1.
- Blocked by: Accepted commerce mode contract and backend parity check.
- Recommended next action: Document canonical commerce mode state machine.

### CTC-GAP-002

- Domain: Ticket purchase / order
- Current issue: Multiple purchase/order RPCs and overloads exist or are referenced across local and production evidence.
- Expected clean commerce contract: One accepted purchase/order authority path, with legacy paths classified.
- Risk: Revenue-sensitive path divergence and unclear guard coverage.
- Priority candidate: Candidate P1.
- Blocked by: Production RPC body/grant/usage review.
- Recommended next action: Commerce purchase source-of-truth review.

### CTC-GAP-003

- Domain: Entitlement conflicts
- Current issue: Guard architecture is present, but coverage across purchase, reservation, claim, transfer, request, approval, and order issuance remains incomplete.
- Expected clean commerce contract: Every acquisition and ownership path checks the same backend entitlement conflict policy.
- Risk: Duplicate or incompatible entitlements.
- Priority candidate: Candidate P1.
- Blocked by: Full active RPC path inventory and body review.
- Recommended next action: Entitlement coverage matrix.

### CTC-GAP-004

- Domain: Direct ticket access
- Current issue: Dashboard local source includes direct `tickets` status mutation paths while production evidence shows `tickets` RLS enabled with zero direct policies.
- Expected clean commerce contract: Ticket status mutation goes through accepted backend authority or an explicitly documented RLS policy.
- Risk: Security-sensitive/revenue-sensitive ambiguity; direct path may be stale, blocked, or divergent.
- Priority candidate: Candidate P1 / Unknown.
- Blocked by: Confirming active dashboard path and production RLS behavior.
- Recommended next action: Direct data access / RLS reliance audit.

### CTC-GAP-005

- Domain: Ticket product and buyer layout
- Current issue: UI performs significant product-section, capacity, and layout validation; backend parity is not fully proven.
- Expected clean commerce contract: Backend validates final product-section, seat, standing, capacity, and availability rules.
- Risk: Revenue-sensitive oversell or incorrect buyer availability if UI and backend diverge.
- Priority candidate: Candidate P1.
- Blocked by: Venue buyer flow RPC/body review.
- Recommended next action: Venue Buyer Flow Contract Audit.

### CTC-GAP-006

- Domain: Reservations vs tickets
- Current issue: Reservation and ticket sales are intended to be mutually exclusive for event commerce, but state naming and guard coverage are split.
- Expected clean commerce contract: Backend blocks illegal dual-mode mutations and cross-entitlement conflicts.
- Risk: Product correctness and entitlement ambiguity.
- Priority candidate: Candidate P1.
- Blocked by: Commerce mode and reservation/ticket guard review.
- Recommended next action: Event commerce mode audit.

### CTC-GAP-007

- Domain: Venue reservations
- Current issue: Venue reservation v1/v2 and event reservation surfaces coexist with different semantics.
- Expected clean commerce contract: Event reservations and venue/service bookings have distinct documented state machines and authority owners.
- Risk: Operational and product correctness ambiguity.
- Priority candidate: Candidate P2.
- Blocked by: Venue reservation product decision.
- Recommended next action: Venue reservation contract review.

### CTC-GAP-008

- Domain: Wallet ownership
- Current issue: Wallet truth is mostly RPC-first, but multiple ticket detail/ownership helpers and legacy paths exist.
- Expected clean commerce contract: One accepted wallet ownership read model and one accepted ticket detail authority.
- Risk: Privacy-sensitive and product correctness divergence.
- Priority candidate: Candidate P2.
- Blocked by: Active usage inventory.
- Recommended next action: Wallet/read-model source-of-truth review.

### CTC-GAP-009

- Domain: Gift claims and transfers
- Current issue: Pending claim, gift entitlement, and recipient checks exist, but transfer/claim coverage across all paths is not fully verified.
- Expected clean commerce contract: Claim and transfer operations enforce sender ownership, recipient eligibility, pending claim uniqueness, expiration, and capacity.
- Risk: Revenue-sensitive ownership inconsistency.
- Priority candidate: Candidate P1 / Unknown.
- Blocked by: Function body and grant review.
- Recommended next action: Gift claim/transfer authority audit.

### CTC-GAP-010

- Domain: Check-in participation
- Current issue: Current scanner RPC has positive controls, but legacy check-in/proof paths remain in the surface.
- Expected clean commerce contract: Check-in eligibility and proof mutation derive from accepted ticket ownership/status and scanner authority.
- Risk: Product correctness and security-sensitive participation ambiguity.
- Priority candidate: Candidate P1 / Unknown.
- Blocked by: Legacy path reachability and proof RPC review.
- Recommended next action: Keep proof check-in hardening plan blocked until explicit patch approval and reachability review.

### CTC-GAP-011

- Domain: Payment/order lifecycle
- Current issue: Order create, paid, confirm, issue, and expire RPCs are known by name but active authority and caller model are not established in this audit.
- Expected clean commerce contract: Payment and ticket issuance lifecycle has one accepted backend authority and payment verification boundary.
- Risk: Revenue-sensitive if callable without accepted authority.
- Priority candidate: Unknown / Needs verification.
- Blocked by: Payment integration and production RPC verification.
- Recommended next action: Payment/order lifecycle audit.

### CTC-GAP-012

- Domain: Dashboard commerce setup
- Current issue: Dashboard directly edits event-level commerce-adjacent fields such as seat selection mode and max-per-buyer in local source.
- Expected clean commerce contract: Revenue-relevant event configuration is backend-authoritative and consistent with publish/readiness guards.
- Risk: Product correctness and revenue-sensitive setup drift.
- Priority candidate: Candidate P2.
- Blocked by: Policy/RPC authority review for event configuration fields.
- Recommended next action: Event Lifecycle Contract Audit.

### CTC-GAP-013

- Domain: Legacy/public web surfaces
- Current issue: Duplicate web/public app surfaces appear to reference older ticket/check-in/share flows.
- Expected clean commerce contract: Public/share/claim handoff is clearly separated from authenticated ticket ownership and mutation authority.
- Risk: Unknown / Needs verification.
- Priority candidate: Unknown.
- Blocked by: Active deployment/source-path confirmation for public web surfaces.
- Recommended next action: Public Web / Share Surface Audit.

## 21. Product Decisions Required

- What is the canonical commerce mode vocabulary and state machine?
- Is ticket sales vs event reservation mutual exclusion always required, or are mixed modes allowed for any product case?
- Which purchase/order RPC is accepted as the current buyer path?
- Which payment confirmation and ticket issuance path is accepted?
- Which reservation flow is event attendance, and which is venue/service booking?
- Are gift transfer and claim flows Core MVP, Product-critical, or optional?
- Should dashboard direct ticket status mutation remain supported, or should ticket status mutation be RPC-only?
- Which wallet read model is canonical for mobile, dashboard, and public handoff?
- What seat/standing/section constraints are product-required vs host setup guidance?
- Which legacy ticket/check-in/order RPCs should remain callable, and which should be deprecated later?

## 22. Recommended Next Audits

1. Venue Buyer Flow Contract Audit

   Focus on seat/standing/section mappings, exact-seat purchase guards, product-section usage, buyer preview, and layout authority.

2. Event Lifecycle Contract Audit

   Focus on commerce mode legality, event status, publish readiness, ticket/reservation availability, check-in timing, and lifecycle-dependent buyer behavior.

3. Direct Data Access / RLS Reliance Audit

   Focus on direct table access for tickets, reservations, products, events, venue layouts, orders, and claims.
## 23. Non-Goals

- No implementation guidance is provided.
- No SQL, migration, or patch content is provided.
- No production connection was made.
- No Supabase CLI, builds, tests, installs, or deployment commands were run.
- No feature removal, cleanup, migration, or hardening work is authorized.
- No final exploitability or production vulnerability claim is made.
- No claim is made that any legacy surface can be safely removed.

## 24. Open Questions

- Which ticket purchase/order path is production-current?
- Are `purchase_event_ticket_v2`, v3, and v4 still used by any deployed client?
- Are `create_ticket_order_v1` and `create_commerce_order_v1` separate product flows or historical alternatives?
- Which RPC owns payment confirmation and ticket issuance?
- Do all ticket/reservation/gift/claim/transfer acquisition paths call the same entitlement guard?
- Are dashboard direct ticket status updates currently reachable, and if so, are they intentionally authorized by RLS?
- Which backend object owns max-per-buyer: event-level field, product-level field, or both?
- Is venue reservation part of the same commerce contract as event attendance, or a separate business-tools contract?
- What public/share claim behavior is required before authentication?
- Which legacy ticket/check-in paths are still needed for backward compatibility?

## 25. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `07_Audits/CommerceTicketingContractAudit.md` was created/modified.


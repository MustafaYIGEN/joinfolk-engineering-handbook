# Venue Buyer Flow Contract Audit

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Handbook docs + read-only local source inspection
- canonical: false

## 2. Purpose

This document is a venue buyer flow contract audit for JoinFolk. It maps observed dashboard venue layout/editor behavior, mobile buyer behavior, and backend/RPC authority candidates into one venue buyer contract view.

This is not an implementation plan, patch plan, cleanup plan, migration plan, or authorization to add, remove, or change features. It does not authorize modifying backend/RPC/RLS/storage/auth, any Supabase tree, dashboard code, mobile code, web code, or application code.

## 3. Audit Scope

Read-only evidence was drawn from:

- Handbook repository: `C:\dev\joinfolk-engineering-handbook`
- Platform/backend candidate repository: `C:\dev\hostos`
- Web/dashboard repository: `C:\dev\joinfolk-web`
- Mobile repository: `C:\dev\hostos\apps\mobile`

The audit focused on:

- Dashboard venue layout editor, visual venue editor, template adapters, blueprint/template behavior, buyer preview, split-section behavior, product-section mapping, and ticket product setup validation.
- Mobile venue buyer routes and helpers: area select, buy ticket, seat picker, session picker, buyer map tap resolver, overview map, buyer snapshot adapter, presentation scene builder, standing-area logic, split-section signals, and hit testing.
- Backend/RPC candidates for venue layouts, buyer zones, seat availability, event product-section usage, event seating config, exact-seat purchase validation, standing tickets, ticket products, and purchase/order authority.
- Prior production SQL/RPC evidence from handbook audits.

Current system context preserved:

- Future accepted Supabase migration target: `C:\dev\hostos\supabase\migrations`.
- This target is not proof of historical sole canonical source.
- Split-source migration history remains unresolved.
- Production SQL/RPC evidence remains stronger than local source assumptions.
- Database Functions / RPC evidence is separate from Edge Function deployment evidence.
- No deployed Supabase Edge Functions were visible in Dashboard based on manual confirmation.
- No backend patch or migration is authorized by this audit.
- Commerce + Ticketing Contract Audit identified venue buyer flow as a revenue-sensitive follow-up area.

## 4. Venue Buyer Contract Summary

JoinFolk appears to support a venue buyer flow where hosts configure venue geometry and ticket product section access in dashboard, while buyers use mobile flows to select products, areas, exact seats, standing areas, and sessions.

The desired clean contract is:

- Dashboard may own host editing, visual geometry authoring, buyer preview, and setup validation.
- Mobile may own buyer rendering, hit testing, product selection, area selection, seat selection, and purchase UX.
- Backend/RPC/RLS/auth must own final venue/layout mutation authority, product mapping authority, availability, capacity, seat/standing allocation, product eligibility, order/purchase, and ticket issuance.
- UI layout and preview can be useful, but final seat/standing/product availability must be backend-authoritative.

Observed reality is partially aligned but not fully deterministic. Strong positive signals include a dashboard-generated buyer snapshot contract, a mobile buyer snapshot bridge, mobile comments that raw availability remains purchase truth, and backend provenance for exact-seat/standing-ticket/product-section migrations. The main risks are split source history, UI-heavy geometry and eligibility logic, direct event/layout/table access, multiple purchase/order paths, inconsistent section-key semantics between mobile flows, and unknown production parity for several layout/product/session tables.

## 5. Active Venue Buyer Flow Map

| Flow | Observed current surface | Backend/RPC candidates | Contract status | Notes |
| --- | --- | --- | --- | --- |
| Host creates venue/layout | Dashboard venue pages and visual editor; hostos dashboard venue pages | `create_venue_layout_v1`, `create_visual_venue_layout_v1`, `save_venue_layout_v1`, `save_layout_as_blueprint_v1`, `update_venue_v2` | Split / Needs verification | Dashboard has rich visual editor; production RLS for `venues` confirmed, layout RLS not confirmed in supplied evidence. |
| Host edits visual geometry | Dashboard VisualVenueEditor, template adapters, split engines, readiness panel | Layout save RPCs and possible direct layout data paths | UI-heavy | Geometry is authored in dashboard and serialized into layout/section metadata. |
| Host publishes buyer snapshot | Dashboard buyer snapshot types and preview panel | `save_venue_layout_v1` / layout persistence candidate | Mostly deterministic locally | Snapshot comments say publish/apply-to-live writes buyer snapshot atomically; production path needs verification. |
| Host maps products to sections | Dashboard Products page and SectionAccessPicker | `upsert_event_ticket_product_v2`, `get_event_product_section_usage_v1`, `get_event_ticket_products_v1` | Mostly deterministic but UI-heavy | UI sanitizes stale keys and handles `SECTION_BINDING_CONFLICT`; backend parity must be confirmed. |
| Buyer selects products | Mobile buy-ticket route | `get_event_ticket_products_v1`, `create_commerce_order_v1`, legacy purchase RPCs | Split / duplicated | Mobile can route to area-select when visual layout and product restrictions require area choice. |
| Buyer selects area | Mobile area-select route and VenueOverviewMap | `get_event_seat_availability_v1`, `get_event_ticket_products_v1`, buyer snapshot in `venue_frame_def` | UI-heavy / Backend authority unclear | Mobile computes eligible section keys and routes seated areas to seat-picker, standing/table/booth to buy-ticket. |
| Buyer selects exact seats | Mobile seat-picker route and BuyerVenueRenderer | `get_event_seat_availability_v1`, `create_commerce_order_v1`, `purchase_event_ticket_v3` fallback | Split / duplicated | UI prevalidates seat/product assignment; backend must revalidate purchase. |
| Buyer selects standing/table area | Mobile area-select plus buy-ticket or seat-picker standing allocation | `create_commerce_order_v1`, `purchase_event_ticket_v5`, `purchase_event_ticket_v4`, standing-ticket migration candidates | Backend authority unclear | Standing support exists locally and in hostos provenance; final production path needs review. |
| Buyer selects session | Mobile session-picker direct reads | `event_sessions_v1`, `tickets`, purchase/order RPCs with `p_session_id` | UI-derived / RLS unclear | Session availability is computed in UI from direct reads; backend purchase must enforce final capacity. |
| Buyer purchase/order | Mobile commerce boundary and seat-picker fallback | `create_commerce_order_v1`, `purchase_event_ticket_v5`, `purchase_event_ticket_v4`, `purchase_event_ticket_v3` | Split / duplicated | Commerce audit already tracks purchase/order source-of-truth as Candidate P1 / Needs verification. |

## 6. Host Configuration Flow Map

Dashboard host/editor behavior observed:

- Venue visual editor supports templates, blueprints, stadium/arena/rectilinear/radial adapters, visual cells, shape splitting, merge behavior, custom geometry, focal/context areas, frame overlays, and readiness checks.
- Buyer snapshot generation defines buyer-facing `buyer_behavior` values: `selectable`, `visual_only`, `disabled`, `parent_group`, `circulation`, and `none`.
- Buyer snapshot comments state that publish/apply-to-live generates and writes the snapshot atomically, mobile consumes `buyer_behavior` when present, and `buyer_selectable` is always explicit.
- Buyer preview renders snapshot sections in layers: frame, circulation, facility/visual-only, and sellable/selectable.
- Product setup maps products to `allowed_section_keys` and `section_price_overrides`.
- Product setup validates capacity, stale/orphan section keys, locked section references, and backend `SECTION_BINDING_CONFLICT` errors.
- Product setup directly reads and writes event fields including `seat_selection_mode`, `venue_layout_id`, and `max_tickets_per_buyer` in local source.

Contract assessment:

- Host configuration is product-critical and revenue-sensitive.
- Dashboard UI is a valid editing and preview surface.
- Final authority for layout mutation, product-section mapping, capacity, stale-key protection, and publish/readiness must be backend/RPC/RLS/auth.
- Direct event/layout writes and UI-only validation should be mapped to production RLS/RPC authority before patching.

## 7. Mobile Buyer Flow Map

Mobile buyer behavior observed:

- `buy-ticket` is the main product-selection and purchase stepper for non-seating flows and the entry into area selection for visual layouts.
- `area-select` loads seat availability, products, `venue_frame_def`, buyer snapshot data, and computes eligible section keys for the buyer map.
- `area-select` routes seated sections to `seat-picker`; non-seated sections such as standing/table/booth can route to `buy-ticket` with a selected section key.
- `seat-picker` loads availability, filters sections by basket product constraints, supports exact seats, has standing allocation state, and can create commerce orders or fall back to legacy single-product purchase.
- `session-picker` reads `event_sessions_v1` and direct `tickets` status counts to display session availability before routing into buy-ticket with `sessionId`.
- `buyerSnapshotAdapter` explicitly states synthetic snapshot sections are for map rendering only, while raw availability remains the source of truth for eligible keys, seat-picker routing, and purchase flow.
- `buildVenuePresentationScene` classifies areas into frame, context, sellable, and environment buckets, then marks selectability from eligible keys and buyer behavior.
- `buyerMapTapResolver` maps rendered screen taps to sellable section keys using forward transform and hit-test logic.

Contract assessment:

- Mobile buyer flow is active core / product-critical.
- Mobile rendering and tap resolution are UX authority only.
- Backend/RPC must revalidate product, section, seat, standing, session, capacity, price, and entitlement at purchase/order time.
- Mobile has several frontend-heavy eligibility rules that should be treated as preview/guidance unless production backend evidence confirms matching enforcement.

## 8. Venue Buyer Authority Matrix

| Domain | Flow / action | Current observed surface | Expected authority owner | Active RPC / data path candidates | Determinism status | Risk class | Evidence source | Recommendation |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Venue ownership | Create/update venue and venue details | Dashboard/mobile/hostos venue pages | Backend/RPC/RLS/auth | `update_venue_v2`, `get_public_venue_detail_v2`, venue table policies | Mostly deterministic | Security-sensitive, operational/admin-sensitive | Production parity report; source scan | Preserve, document contract |
| Layout creation | Create visual or canonical layout | Dashboard visual editor and hostos/mobile layout APIs | Backend/RPC/RLS/auth | `create_venue_layout_v1`, `create_visual_venue_layout_v1` | Split / Needs verification | Product correctness | Source scan; provenance report | Reconcile |
| Layout save/publish | Save draft/live layout and buyer snapshot | Dashboard VisualVenueEditor and buyer snapshot generator | Backend/RPC/RLS/auth | `save_venue_layout_v1`, `save_layout_as_blueprint_v1` | Backend authority unclear | Revenue-sensitive | Dashboard source; migration provenance | Verify live snapshot path |
| Templates/blueprints | Generate layout from templates and adapters | Dashboard template adapters, blueprints, shared package | Mixed; backend for persisted result | Blueprint/template migrations and shared package | Split / duplicated | Product correctness | Dashboard/shared source; provenance report | Document canonical template contract |
| Shape splitting | Split/merge sections and child areas | Dashboard split engines and preview; mobile snapshot/hit-test support | Backend for persisted result; UI for editing | `save_venue_layout_v1`, topology metadata | UI-heavy | Revenue-sensitive | Dashboard/mobile source | Reconcile split lineage contract |
| Buyer snapshot | Export buyer-facing layout | Dashboard buyerSnapshotTypes; mobile buyerSnapshotAdapter | Backend/RPC for persistence; mobile UI for rendering | `venue_frame_def.buyer_snapshot` via availability/layout RPCs | Mostly deterministic locally | Product correctness | Dashboard/mobile source | Preserve, verify production persistence |
| Area selectability | Decide selectable vs visual-only vs disabled | Dashboard buyer behavior; mobile eligibility filters | Backend/RPC at purchase; UI mirror for map | `get_event_seat_availability_v1`, buyer snapshot fields | Split / UI-heavy | Revenue-sensitive | Mobile/dashboard source | Document canonical selectability contract |
| Product-section mapping | Limit products to sections | Dashboard Products page; mobile basket/area/seat filters | Backend/RPC/RLS/auth | `upsert_event_ticket_product_v2`, `get_event_product_section_usage_v1`, `get_event_ticket_products_v1` | Mostly deterministic, parity unknown | Revenue-sensitive | Commerce audit; dashboard/mobile source | Verify backend parity |
| Exact seat selection | Pick specific available seats | Mobile seat-picker | Backend/RPC purchase authority | `get_event_seat_availability_v1`, `_validate_exact_seat_purchase_v1`, purchase/order RPCs | Split / duplicated | Revenue-sensitive | Mobile source; provenance report | Reconcile active purchase path |
| Standing-area selection | Pick quantity in standing/table/booth areas | Mobile area-select/buy-ticket/seat-picker; hostos standing-ticket migration | Backend/RPC purchase authority | `purchase_event_ticket_v5`, `create_commerce_order_v1`, standing-ticket migrations | Backend authority unclear | Revenue-sensitive | Mobile source; provenance report | Focused standing-ticket review |
| Session selection | Pick showtime/session | Mobile session-picker direct reads | Backend/RPC/RLS/auth | `event_sessions_v1`, `tickets`, purchase/order `sessionId` | UI-derived / RLS unclear | Revenue-sensitive | Mobile source | Verify session authority |
| Buyer hit testing | Resolve tap to section key | Mobile `buyerMapTapResolver` | Mobile UI only | Local geometry and presentation scene | UI-derived only | UX-only unless treated as authority | Mobile source | Preserve as UX helper only |
| Purchase/order | Convert selection to tickets/order | Mobile commerce boundary and seat-picker fallback | Backend/RPC/RLS/auth | `create_commerce_order_v1`, `purchase_event_ticket_v5`, `purchase_event_ticket_v4`, `purchase_event_ticket_v3` | Split / duplicated | Revenue-sensitive | Commerce audit; mobile source | Reconcile with commerce audit |
| Venue media context | Show venue photos/posters for buyer context | Mobile/dashboard venue media components | Storage/RLS for access, UI for display | `add_venue_media_v1`, `update_venue_media_v1`, `remove_venue_media_v1`, storage policies | Mostly deterministic | Privacy-sensitive | Production storage report; source scan | ADR/security decision |
| Venue reservations | Venue/service booking separate from ticket buyer flow | Mobile v2 and dashboard status pages | Backend/RPC/RLS/auth | `create_venue_reservation_v1/v2`, `decide_venue_reservation_v2` | Split / Needs verification | Revenue-sensitive | Commerce audit; source scan | Keep separate contract |

## 9. Layout / Geometry Contract Assessment

Expected clean contract:

- Layout geometry has one persisted model for sections, rows, seats, standing areas, frames, focal/context areas, split children, and buyer-facing snapshot data.
- Dashboard may author and preview geometry, but saved layout state must be validated by backend authority.
- Mobile should render a published buyer-facing snapshot or raw availability payload without reinterpreting editor internals as purchase truth.

Observed evidence:

- Dashboard visual editor has extensive geometry, template, split, merge, cell, and shape tooling.
- Dashboard buyer snapshot type comments describe the snapshot as the buyer-facing publication output and say `buyer_behavior` is the single source of truth for buyer behavior classification.
- Mobile buyer snapshot adapter strips editor-internal topology and creates synthetic availability sections for map rendering only.
- Mobile raw availability remains documented as the source for eligible keys, seat-picker routing, and purchase flow.
- Hostos shared package has an `admission-model` resolver for none, reservation-only, general admission, exact seat, and future area-based models.

Assessment:

- Venue buyer status: Product-critical.
- Determinism status: Mostly deterministic locally, but split across dashboard, mobile, shared package, and backend provenance.
- Authority owner: Mixed; backend/RPC/RLS/auth must own persisted mutation and purchase validation.
- Recommendation: Preserve buyer snapshot concept, document it as presentation contract only, and verify backend save/publish authority.

## 10. Section / Product Mapping Assessment

Expected clean contract:

- Product-section mapping is stored as backend-owned product configuration.
- `allowed_section_keys = null` should have one accepted meaning.
- Empty `allowed_section_keys` should have one accepted meaning.
- Product restrictions, section price overrides, stale section keys, locked section references, and product-section usage should be backend-enforced.

Observed evidence:

- Dashboard Products page treats `allowed_section_keys = null` as all sellable areas and rejects empty arrays for seating mode.
- Mobile area-select comments treat `null` as unrestricted and empty array as explicitly no sections allowed.
- Mobile buy-ticket uses similar section filtering for selected section keys.
- Mobile seat-picker filtering appears to treat missing or empty `allowed_section_keys` as allowing the section in one observed path.
- Dashboard handles `SECTION_BINDING_CONFLICT` from backend when a section removal conflicts with sold or pending seats.
- `get_event_product_section_usage_v1` is used by dashboard product setup to protect capacity and mapping changes.

Assessment:

- Venue buyer status: Product-critical.
- Determinism status: Split / duplicated.
- Authority owner: Backend/RPC/RLS/auth for final enforcement; dashboard/mobile for UI guidance.
- Risk class: Revenue-sensitive.
- Recommendation: Candidate P1 review for accepted `allowed_section_keys` semantics and backend parity across purchase/order paths.

## 11. Seat / Standing-Area Availability Assessment

Expected clean contract:

- `get_event_seat_availability_v1` or an equivalent backend path must return buyer-safe availability for sections, seats, standing areas, gift-reserved seats, checked-in seats, blocked seats, and remaining capacity.
- Purchase/order RPCs must revalidate seat IDs, section keys, standing allocations, product constraints, capacity, session, event lifecycle, and entitlement at mutation time.
- UI availability is a preview and may be stale by the time purchase occurs.

Observed evidence:

- Mobile availability types include seat statuses: available, sold, checked-in, gift-reserved, and blocked.
- Mobile area-select computes eligible keys from role, template area kind, semantic type, disabled category, capacity/remaining, and product allowed-section intersection.
- Mobile area-select has a snapshot fallback for selectable standing/table/booth sections not present in raw sections.
- Mobile seat-picker supports exact seats and standing allocations, then routes multi-product/basket purchases through `create_commerce_order_v1`.
- Hostos provenance includes exact-seat purchase guard and standing-ticket migrations.
- Commerce audit already identified active purchase/order source-of-truth as unresolved.

Assessment:

- Venue buyer status: Product-critical.
- Determinism status: Split / UI-heavy.
- Authority owner: Backend/RPC/RLS/auth for final mutation; mobile UI for selection.
- Risk class: Revenue-sensitive.
- Recommendation: Verify that purchase/order RPCs enforce all seat/standing/product constraints, especially where mobile uses snapshot-only standing/table sections.

## 12. Dashboard Preview vs Mobile Buyer Assessment

Expected clean contract:

- Dashboard buyer preview should match the mobile buyer map for published layouts, but preview mismatch should not affect backend purchase authority.
- The preview contract should specify which fields are published to buyers, which editor internals are stripped, and which fallback paths are allowed for legacy layouts.

Observed evidence:

- Dashboard preview renders a generated `BuyerPublishedLayout`.
- Mobile buyer snapshot adapter mirrors dashboard snapshot types and supports only version 1.
- Mobile falls back to raw sections if buyer snapshot is missing, invalid, or unsupported.
- Mobile presentation builder says every buyer surface consumes the same builder instead of independently parsing topology metadata, while seat picker retains its own rendering pipeline.
- Dashboard and mobile both carry debug/audit traces for split child, path data, coordinate mode, and preview/render parity.

Assessment:

- Venue buyer status: Active core path for visual venues.
- Determinism status: Mostly deterministic locally, with fallback paths and split renderers.
- Authority owner: Dashboard UI and mobile UI for rendering; backend/RPC for persisted snapshot and purchase validation.
- Risk class: Product correctness and revenue-sensitive if preview implies buyability not accepted by backend.
- Recommendation: Document buyer snapshot as the canonical presentation contract and keep purchase authority separate.

## 13. Backend RPC / RLS Authority Evidence Map

Production evidence from prior reports:

- Production SQL/RPC evidence remains stronger than local source assumptions.
- `venues` exists with RLS enabled in supplied production verification.
- `venue_media` exists with RLS enabled, and venue media storage buckets/policies were confirmed as public-read with constrained write signals.
- `tickets`, `commerce_orders`, and `event_ticket_claims_v1` production evidence makes RPC internal guards critical for revenue-sensitive operations.
- Commerce/order/purchase authority remains linked to the Commerce + Ticketing Contract Audit and is not resolved here.

Local-source and provenance evidence:

- Hostos migration provenance is strongest overall for current venue/media/commerce RPCs.
- Hostos migrations include venue commerce RPC parts, canonical templates, exact-seat guard, blueprint topology patch, storage upload hardening, and standing-ticket support.
- Joinfolk-web migration history includes buyer zones, seat availability, layout persistence, purchase basket seating, and visual layout files.
- Mobile migration history includes venue template scaffolding, seat availability archetype, buyer-flow fixes, and visual topology metadata.
- Dashboard/mobile source references RPC candidates including `get_event_seat_availability_v1`, `get_event_product_section_usage_v1`, `get_event_ticket_products_v1`, `upsert_event_ticket_product_v2`, `get_venue_layout_v1`, and purchase/order RPCs.

Unknown / Needs verification:

- Production RLS/policy status for `venue_layouts`, layout section/seat tables, `event_ticket_products_v1`, and `event_sessions_v1`.
- Whether `get_buyer_venue_zones_v1` is active in the current production path.
- Whether `_validate_exact_seat_purchase_v1` covers all active purchase/order paths.
- Whether standing-area purchases are fully enforced by current production purchase/order RPCs.
- Whether dashboard layout save/publish goes through RPC-only authority or direct table writes in production.
- Whether buyer snapshot persistence is part of the live production layout save path.

## 14. Direct Data Access / RLS Reliance Map

| Data surface | Observed direct access relevance | Prior production authority evidence | Current classification | Recommendation |
| --- | --- | --- | --- | --- |
| `venues` | Venue detail/edit/media surfaces | RLS enabled in production target set | RLS authority likely; policy correctness separate | Preserve and document owner/host contract. |
| `venue_media` / storage buckets | Venue media buyer context and host media mutation | RLS enabled; public buckets and public read confirmed | Storage authority mostly known; ADR needed | Keep ADR/security decision separate from buyer contract. |
| `venue_layouts` | Dashboard layout editor/product setup; mobile layout summary RPC | Not covered in supplied production RLS summary | Unknown / Needs verification | Verify RLS/RPC authority before patching. |
| Layout sections/rows/seats | Buyer map, exact seats, product-section mapping | Not covered in supplied production RLS summary | Unknown / Needs verification | Verify direct/table/RPC ownership. |
| `event_ticket_products_v1` | Dashboard product setup and mobile buyer product list | Not fully summarized in production RLS evidence | Unknown / Needs verification | Confirm product mutation/read authority. |
| `events` | `seat_selection_mode`, `venue_layout_id`, `max_tickets_per_buyer` | RLS enabled and policies exist | RLS authority likely but policy correctness separate | Review revenue-relevant field mutation authority. |
| `tickets` | Session availability direct counts; seat availability/order truth | RLS enabled; zero direct policies in focused output | RLS likely default-deny; RPC guards critical | Avoid treating UI direct counts as final authority. |
| `commerce_orders` | Order creation/issue path | RLS enabled; deny-all style policy | RPC-only reliance likely | Use Commerce + Ticketing findings. |
| `event_sessions_v1` | Mobile session picker direct read | Production RLS not summarized here | Unknown / Needs verification | Verify session read/capacity authority. |
| `venue_reservations` | Venue/service bookings | Production RLS not summarized here | Unknown / Needs verification | Keep separate venue reservation contract. |

## 15. Frontend UI Gate and Helper Map

| UI/helper surface | Observed role | Acceptable as UI guidance? | Risk if treated as authority | Recommendation |
| --- | --- | --- | --- | --- |
| Dashboard VisualVenueEditor | Author layout geometry, split/merge cells, save/publish, preview | Yes | Geometry mutation and publish legality cannot rely on UI only | Verify backend save/publish authority. |
| Dashboard BuyerPreviewPanel | Preview buyer snapshot render | Yes | Preview may imply buyability if backend purchase rejects or accepts different set | Document as presentation preview only. |
| Dashboard buyerSnapshotTypes | Defines `BuyerPublishedLayout` and `buyer_behavior` | Yes | Snapshot classification can drift from backend purchase constraints | Preserve, verify persistence and backend parity. |
| Dashboard Products page | Maps products to sections and protects capacity/stale keys | Yes | UI validation cannot be final capacity/product-section authority | Verify `upsert_event_ticket_product_v2` and usage guard. |
| Mobile buyerSnapshotAdapter | Converts snapshot to synthetic render sections | Yes | Synthetic snapshot sections must not become purchase truth | Preserve explicit rendering-only invariant. |
| Mobile buildVenuePresentationScene | Classifies/render areas and eligible keys | Yes | Classification should not authorize purchase | Keep as UI render contract. |
| Mobile buyerMapTapResolver | Maps taps to section keys | Yes | Hit test cannot authorize product/seat purchase | Preserve as UX-only helper. |
| Mobile area-select eligibility | Filters sellable/eligible section keys | Yes as preview | Product/section constraints may differ from backend | Reconcile semantics with backend. |
| Mobile seat-picker prevalidation | Assigns selected seats to basket products | Yes as preview | Race/stale availability and section assignment need backend revalidation | Verify purchase/order RPC validation. |
| Mobile session-picker availability | Computes session counts from direct reads | Yes as preview | Session capacity cannot be trusted from UI direct counts | Verify session capacity in purchase/order RPC. |

## 16. Duplicated / Split / Legacy Surfaces

| Surface / helper / RPC | Observed role | Current/legacy/unknown | Risk if still active or authoritative | Evidence type | Recommendation |
| --- | --- | --- | --- | --- | --- |
| Dashboard visual editor vs mobile presentation builder | Host authoring vs buyer rendering | Current split | Preview/render mismatch can affect buyer trust | Local source | Document snapshot contract. |
| Buyer snapshot vs raw availability | Presentation payload vs purchase truth | Current split | Snapshot-only sections could be shown without backend purchase support | Local source | Verify backend parity for standing/table/booth sections. |
| Dashboard preview vs mobile buyer map | Preview and actual buyer render | Current split | Visual parity can drift | Local source | Add parity audit later. |
| `allowed_section_keys` null/empty semantics | Product-section restrictions | Current split | UI paths may disagree on unrestricted vs no sections | Local source | Candidate P1 contract review. |
| `purchase_event_ticket_v3` / v4 / v5 / commerce order | Purchase/order authority | Duplicated / Unknown | Final validation may differ by path | Commerce audit; local source | Reconcile active purchase path. |
| `create_venue_layout_v1` / `create_visual_venue_layout_v1` / `save_venue_layout_v1` | Layout lifecycle | Unknown | Draft/live layout authority unclear | Source scan/provenance | Verify active layout path. |
| Template adapters: stadium, arena, rectilinear, radial | Host geometry generation | Current / experimental mix | Template-specific semantics can drift | Dashboard source | Document supported template contract. |
| Shape split engines and split child snapshot handling | Split sections | Current / Needs verification | Product mappings can point to parent/child inconsistently | Dashboard/mobile source | Reconcile split lineage rules. |
| `get_buyer_venue_zones_v1` | Buyer zones candidate | Unknown | Could represent older buyer-zone contract | User-required target list/provenance | Verify whether active. |
| `event_sessions_v1` direct reads | Session picker availability | Unknown / direct-access path | UI availability may bypass backend authority | Mobile source | Verify RLS and purchase enforcement. |
| Venue reservation v1/v2 | Venue/service booking | Split / Needs verification | Could overlap with venue buyer semantics | Commerce audit/source scan | Keep separate contract. |

## 17. Revenue-Sensitive Invariants

The following invariants should be backend-authoritative:

- A buyer can only purchase an active ticket product for the target event and session.
- Product price, currency, capacity, max-per-buyer, giftability, transferability, category, and section restrictions are validated by backend authority.
- `allowed_section_keys` semantics are consistent across dashboard, mobile, and backend.
- Exact seat IDs must belong to the event's linked layout, allowed product section, selected session if applicable, and current availability.
- Standing/table/booth area purchases must enforce section capacity, product capacity, event capacity, and product-section mapping.
- Snapshot-only visual sections cannot authorize a purchase unless backend purchase/order RPCs accept and validate the same section key.
- Product-section mapping cannot remove or invalidate sections that already have sold, pending, allocated, or checked-in tickets unless the backend explicitly allows it.
- Event `seat_selection_mode`, `venue_layout_id`, and `max_tickets_per_buyer` cannot be changed in ways that invalidate existing orders, tickets, or check-ins.
- Dashboard preview and mobile hit testing cannot be the final authority for product eligibility or availability.
- Purchase/order RPCs must revalidate all UI-selected section keys, seat IDs, quantities, sessions, and product IDs.

## 18. Missing / Incomplete Venue Buyer Feature Candidates

These are not accepted gaps or approved feature work. They are candidates requiring product and authority decisions:

- Accepted venue buyer contract for general admission, exact seat, standing area, table/booth, and future area-based modes.
- Canonical `allowed_section_keys` null/empty semantics.
- Canonical buyer snapshot publication and fallback contract.
- Backend authority contract for layout draft, live layout, buyer snapshot, and blueprint save paths.
- Session/showtime buyer contract and backend capacity enforcement.
- Product-section mapping lock behavior after sales, pending claims/orders, allocations, and check-ins.
- Template support matrix for stadium, arena, rectilinear, radial, custom, cinema, theater, concert, table, and standing layouts.
- Split-section lineage contract for parent/child keys and product mapping migration behavior.
- Direct data access/RLS policy review for venue layouts, product tables, event sessions, and layout section/seat tables.

## 19. Venue Buyer Gaps / Risk Register Seeds

### VBF-GAP-001

- Gap ID: VBF-GAP-001
- Domain: Buyer snapshot contract
- Current issue: Dashboard generates buyer snapshots and mobile consumes them, but production persistence/path authority is not verified.
- Expected clean venue buyer contract: Buyer snapshot is documented as the canonical presentation contract, persisted by backend-authorized save/publish path, and separate from purchase authority.
- Risk: Product correctness and revenue-sensitive preview/purchase drift.
- Priority candidate: Candidate P1.
- Blocked by: Production layout save/publish evidence and layout RLS/RPC review.
- Recommended next action: Verify layout save/publish authority and buyer snapshot presence in production payloads.

### VBF-GAP-002

- Gap ID: VBF-GAP-002
- Domain: Product-section semantics
- Current issue: `allowed_section_keys` null/empty handling appears inconsistent across dashboard, area-select, and seat-picker paths.
- Expected clean venue buyer contract: One accepted meaning for null, empty array, and explicit section-key arrays across UI and backend.
- Risk: Revenue-sensitive product eligibility drift.
- Priority candidate: Candidate P1.
- Blocked by: Backend product/purchase body review and product decision.
- Recommended next action: Create a product-section mapping contract matrix.

### VBF-GAP-003

- Gap ID: VBF-GAP-003
- Domain: Exact seat purchase authority
- Current issue: UI prevalidates exact seats, but full production proof that every purchase/order path calls exact-seat validation is incomplete.
- Expected clean venue buyer contract: Backend purchase/order RPCs revalidate event/layout/section/seat/product/session/availability for every exact-seat acquisition.
- Risk: Revenue-sensitive seat assignment conflict.
- Priority candidate: Candidate P1.
- Blocked by: Purchase/order RPC body and overload review.
- Recommended next action: Link this to Commerce + Ticketing purchase source-of-truth review.

### VBF-GAP-004

- Gap ID: VBF-GAP-004
- Domain: Standing area purchase
- Current issue: Mobile supports standing/table/booth area routing and hostos provenance includes standing-ticket migration, but final active production enforcement is not established.
- Expected clean venue buyer contract: Standing/table/booth purchases enforce area capacity and product-section mapping in backend purchase/order RPCs.
- Risk: Revenue-sensitive capacity or product mapping drift.
- Priority candidate: Candidate P1 / Unknown.
- Blocked by: Standing-ticket RPC/body review and production active-path confirmation.
- Recommended next action: Focused standing-ticket purchase audit.

### VBF-GAP-005

- Gap ID: VBF-GAP-005
- Domain: Split sections
- Current issue: Dashboard supports splitting, mobile supports split-child rendering/hit-test signals, but product mapping parent/child lineage contract is not accepted.
- Expected clean venue buyer contract: Split parent/child keys, migration of product mappings, and buyer selectability are deterministic and backend-validated.
- Risk: Product correctness and revenue-sensitive section mismatch.
- Priority candidate: Candidate P1.
- Blocked by: Split lineage product decision and backend layout/product mapping review.
- Recommended next action: Split-section lineage audit.

### VBF-GAP-006

- Gap ID: VBF-GAP-006
- Domain: Dashboard direct event configuration
- Current issue: Dashboard directly mutates event seating fields in local source.
- Expected clean venue buyer contract: Revenue-relevant event seating fields are mutated through accepted backend authority or confirmed RLS policy.
- Risk: Product correctness and revenue-sensitive setup drift.
- Priority candidate: Candidate P2.
- Blocked by: Event RLS policy correctness and event lifecycle contract review.
- Recommended next action: Event Lifecycle Contract Audit.

### VBF-GAP-007

- Gap ID: VBF-GAP-007
- Domain: Session picker availability
- Current issue: Mobile session picker computes availability using direct `event_sessions_v1` and `tickets` reads.
- Expected clean venue buyer contract: Session availability and capacity are backend-authoritative and rechecked in purchase/order RPCs.
- Risk: Revenue-sensitive stale or unauthorized availability display.
- Priority candidate: Candidate P2 / Unknown.
- Blocked by: RLS review for `event_sessions_v1` and purchase `sessionId` enforcement review.
- Recommended next action: Direct Data Access / RLS Reliance Audit.

### VBF-GAP-008

- Gap ID: VBF-GAP-008
- Domain: Layout RLS and table authority
- Current issue: Production RLS evidence did not cover venue layout, section, row, seat, or event session tables.
- Expected clean venue buyer contract: Every direct layout/seat/session data path has RLS authority or RPC authority.
- Risk: Security-sensitive and revenue-sensitive Unknown / Needs verification.
- Priority candidate: Candidate P1 / Unknown.
- Blocked by: Manual production RLS/policy verification.
- Recommended next action: Direct Data Access / RLS Reliance Audit.

### VBF-GAP-009

- Gap ID: VBF-GAP-009
- Domain: Template/admission model
- Current issue: Shared admission resolver exists, but dashboard, mobile, and backend canonical adoption is not proven.
- Expected clean venue buyer contract: Admission model is one accepted contract for none, reservation-only, general admission, exact seat, and future area-based modes.
- Risk: Product correctness and buyer route drift.
- Priority candidate: Candidate P2.
- Blocked by: Product decision and cross-surface adoption review.
- Recommended next action: Event Lifecycle Contract Audit.

### VBF-GAP-010

- Gap ID: VBF-GAP-010
- Domain: Preview/mobile parity
- Current issue: Dashboard preview and mobile map have separate renderers and fallback paths.
- Expected clean venue buyer contract: Published buyer snapshot renders consistently enough across preview and mobile, with documented tolerated differences.
- Risk: UX and product correctness; revenue-sensitive if preview shows buyable areas differently.
- Priority candidate: Candidate P2.
- Blocked by: Visual parity test/audit and published snapshot samples.
- Recommended next action: Dashboard Preview vs Mobile Buyer Parity Audit.

### VBF-GAP-011

- Gap ID: VBF-GAP-011
- Domain: Purchase/order source of truth
- Current issue: Venue buyer flow depends on unresolved commerce purchase/order path and legacy purchase fallback.
- Expected clean venue buyer contract: One accepted purchase/order path validates all venue buyer selections.
- Risk: Revenue-sensitive source-of-truth ambiguity.
- Priority candidate: Candidate P1.
- Blocked by: Commerce + Ticketing purchase audit follow-up.
- Recommended next action: Reconcile with CTC-GAP-002 and CTC-GAP-003.

## 20. Product Decisions Required

- What is the accepted admission model vocabulary for venue buyer flows?
- Is `area_based` still future-only, or should standing/table/booth selection formalize it now?
- What is the accepted meaning of `allowed_section_keys = null`?
- What is the accepted meaning of `allowed_section_keys = []`?
- Is buyer snapshot the canonical presentation contract for all visual venue buyers?
- Which templates are supported for production buyer flow: stadium, arena, theater, cinema, concert, standing, custom, radial, rectilinear, tables, booths?
- Are split child sections first-class purchasable sections, or should products remain mapped to parent areas?
- Which backend path owns layout draft save, live publish, and blueprint save?
- Should session/showtime availability be a first-class backend RPC rather than direct table aggregation in UI?
- Which purchase/order RPC is accepted for exact seat and standing-area flows?
- Are venue reservations part of this buyer flow contract or a separate business-tools reservation contract?

## 21. Recommended Next Audits

1. Event Lifecycle Contract Audit

   Focus on event status, publish readiness, seat-selection mode, venue-layout binding, commerce mode legality, and lifecycle-dependent buyer interactivity.

2. Direct Data Access / RLS Reliance Audit

   Focus on enue_layouts, layout sections/rows/seats, event_ticket_products_v1, event_sessions_v1, events, 	ickets, and venue media access.

3. Public Web / Share Surface Audit

   Focus on public venue/event/share surfaces, public venue detail, claim handoff, public media exposure, and buyer-facing public read contracts.
## 22. Non-Goals

- This audit does not provide SQL, migrations, implementation code, patch instructions, cleanup instructions, or production commands.
- This audit does not authorize modifying any application, dashboard, mobile, web, backend, or Supabase code.
- This audit does not authorize changing ticketing, layout, purchase, storage, RLS, or auth behavior.
- This audit does not connect to production or inspect production directly.
- This audit does not claim final exploitability or production vulnerability.
- This audit does not claim feature removal is safe.

## 23. Open Questions

- Is buyer snapshot present in current production `get_event_seat_availability_v1` responses for visual venue events?
- Which production tables store layout sections, rows, seats, shape params, topology metadata, and buyer snapshots?
- What production RLS policies protect venue layout and layout section/seat tables?
- Does every active purchase/order path call exact-seat and product-section validation?
- Does `create_commerce_order_v1` fully validate `section_key`, `p_seat_ids`, and `p_session_id`?
- Are standing/table/booth section keys accepted by production purchase/order RPCs?
- Is `get_buyer_venue_zones_v1` active or historical?
- Is session availability intended to remain direct table aggregation in UI, or should it be backend-provided?
- Which dashboard visual templates are accepted for buyer-facing production use?
- How should product mappings behave when a section is split after products or tickets exist?
- Should venue reservations overlap ticket buyer UX or remain a separate booking product?

## 24. No-Modification Confirmation

- No application code was modified.
- No dashboard/mobile/web code was modified.
- No Supabase tree was modified.
- No SQL or migration was created.
- No production connection was made.
- Supabase CLI was not run.
- No builds/tests/installs were run.
- No files were staged or committed.
- Only `07_Audits/VenueBuyerFlowContractAudit.md` was created/modified.

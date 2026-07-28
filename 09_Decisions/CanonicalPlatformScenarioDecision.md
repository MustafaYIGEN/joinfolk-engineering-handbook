# Canonical Platform Scenario Decision

## 1. Metadata

- Status: Accepted for JoinFolk V1 scenario alignment
- Version: 1.0
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-28
- Source confidence: owner-accepted evidence packet
- canonical: true
- Implementation status: Partially implemented through closed gates; remaining waves tracked separately
- Production mutation status: This document does not authorize production mutation

## 2. Purpose

This decision records the accepted V1 product and system behavior for the canonical JoinFolk platform scenarios.

It is binding for future engineering work. A technical patch that changes these rules requires a new owner-reviewed decision or a documented supersession.

## 3. Binding Evidence

| Evidence | SHA256 |
|---|---|
| `canonical_platform_scenario_ledger_v1.md` | `4E72E83CDDEEA35A98A249939BCD149FB65229C1D00007A53B097E06A05408DB` |
| `current_behavior_deviation_register_v1.md` | `A1DD392706D255C4CDDE497FBC7CDD61EA59A6C70FB8EE7830BE4D75A028B352` |
| `scenario_alignment_patch_wave_plan_v1.md` | `4CF10FD4F3281AD916C193D51216A78ADE746C2ED323D2EA2F2D0359D04FB089` |
| `handbook_update_plan_v1.md` | `7AA55824FD3BC4AA4AC0EC8EEC42CCBB4E45526C92AFFF26775633F4CC7CC5BA` |
| `canonical_platform_owner_review_packet_v1.md` | `5CDA525F3DAA15562B32F95AD7E87F628D3157350261363DB284749B053A9EE0` |

## 4. Accepted V1 Decisions

| # | Binding rule | Engineering consequence |
|---:|---|---|
| 1 | `create_commerce_order_v1` is the sole authenticated V1 purchase boundary. | Do not revive legacy direct ticket purchase or issuance RPCs as app-facing client paths. Payment confirmation and direct ticket issuance remain server-owned. |
| 2 | Reservations remain separate from tickets. No automatic reservation-to-ticket conversion or check-in entitlement exists in V1. | Reservation reads, creates, approvals, cancellations, ticket wallet, and scanner behavior must remain separate contracts. |
| 3 | Personal events use `friends` or `groups`; host events use `public` or `members`. Audience must never be silently reset. | Persona/audience incompatibility must be explicit and block publish until corrected. |
| 4 | Legacy null-persona events resolve as personal without automatic database backfill. | Compatibility resolution belongs in product/server logic. No bulk rewrite is allowed without a new evidence gate. |
| 5 | Event lifecycle is monotonic for V1. Normal path is `draft -> published -> live -> ended -> archived`; live reversal, reopen, and unarchive are not supported. | Lifecycle actions require server authority, clear terminal states, and preserved history. |
| 6 | The existing versioned visual-editor/RPC document is draft layout authority. Published authority is an immutable snapshot. | Frontend must not query guessed `draft_layout` or `published_layout` columns on `venue_layouts`. Buyer/product mapping surfaces must use the canonical authority. |
| 7 | Owner has full authority; editor may configure drafts; operator may perform assigned day-of operations; staff has narrowly assigned capabilities. | Route access, UI affordances, RPC bodies, and negative authorization tests must align with this role matrix. |
| 8 | Buyer selection requires a published, sellable, product-mapped target. Inventory is constrained by published section capacity. | Visual-only, frame, facility, and unmapped areas are not purchase targets. Capacity must be enforced server-side. |
| 9 | Friends are mutual; follows are discovery-only; groups/members require active membership; blocking overrides every relationship. | Visibility helpers and UI language must not let follow state grant private access. Block checks take precedence. |
| 10 | Only a successful zero-row response is empty. Denied, invalid, offline, and degraded outcomes remain distinct. | UI and client code must not mask ACL, network, schema, or contract failures as empty arrays or empty states. |

## 5. Scenario Domains Governed

These rules govern at minimum:

- Buyer and participant journey
- Host event creation and publish journey
- Host dashboard commerce, tickets, and reservations
- Venue layout and product mapping
- Social graph, profile, friends, followers, groups, and clubs
- Staff scanner and check-in
- Notifications and reminders
- Auth, password recovery, and web-versus-mobile routing
- Persona switching between personal and host
- Event lifecycle from draft through archived

## 6. No Authorization By This Document

This document does not authorize:

- code changes
- migrations
- production SQL
- database schema changes
- RPC body changes
- RLS or policy changes
- ACL grants or revokes
- staging, commit, or push

Any implementation must proceed through a separate scoped gate with evidence, review, validation, and source-of-truth closure.


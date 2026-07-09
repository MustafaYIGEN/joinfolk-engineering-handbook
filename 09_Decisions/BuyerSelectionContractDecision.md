# Buyer Selection Contract Decision

## 1. Status

Locked for JoinFolk v1 release hardening.

This decision defines the buyer-facing selection contract for mobile, web companion, dashboard buyer preview, venue editor preview, ticket products, reservation products, standing areas, sellable areas, visual-only areas, and facility markers.

## 2. Problem

JoinFolk supports visual venue layouts, sellable ticket areas, standing sections, split sections, facility markers, and buyer previews.

Without a strict selection contract, the buyer flow can become non-deterministic:

- visual or facility areas may appear selectable
- standing areas may bypass area selection
- product mapping may be unclear
- buyer preview may differ from mobile buyer behavior
- dashboard editor selection may be confused with buyer purchase selection

The product must distinguish between editor-selectable, visually visible, and buyer-purchase-selectable.

## 3. Core Rule

A venue object can be visible and editor-selectable without being buyer-selectable.

Buyer purchase or reservation selection is allowed only when all required buyer selection conditions pass.

## 4. Selection Classes

| Class | Visible to buyer | Selectable in editor | Selectable by buyer | Purchase/reservation target |
| --- | --- | --- | --- | --- |
| Sellable area | Yes | Yes | Yes, if contract passes | Yes |
| Standing sellable area | Yes | Yes | Yes, if contract passes | Yes |
| Split child sellable area | Yes | Yes | Yes, if contract passes | Yes |
| Visual-only area | Yes | Yes | No | No |
| Facility marker | Yes | Yes | No | No |
| Gate / entrance / exit | Yes | Yes | No | No |
| Walkway / service area | Yes | Yes | No | No |
| Disabled area | Optional | Yes | No | No |
| Structural/chrome area | Optional | Yes | No | No |

## 5. Buyer-Selectable Conditions

A venue object is buyer-selectable only when all of the following are true:

- `buyer_selectable = true`
- area role is `sellable`
- area is not `visual_only`
- area is not a facility marker
- area is not a gate, walkway, service, entrance, exit, toilet, bar, DJ booth, info desk, sponsor marker, or other non-commerce marker
- area has a valid product mapping or valid admission model
- product is active
- product is compatible with current commerce mode
- current flow supports the object type
- object is not disabled by validation
- object has a deterministic area key
- object can be resolved by buyer tap/hit-test resolver

If any condition fails, buyer tap must not create a purchase/reservation selection.

## 6. Editor Selection vs Buyer Selection

Dashboard venue editor may allow selecting and editing visual-only areas and facility markers.

That editor behavior does not imply buyer purchase selection.

Editor selection is for authoring.
Buyer selection is for commerce.

The UI must not blur these meanings.

## 7. Facility Marker Contract

Facility markers are informational objects.

Examples:

- bar
- toilet
- entrance
- exit
- walkway
- DJ booth
- info desk
- service
- sponsor
- stage marker
- terrace marker when non-sellable

Facility markers may appear in buyer map and buyer preview, but must not be purchase/reservation targets.

Buyer tap behavior for facility markers must be deterministic:

- either no-op
- or show non-commerce info
- never select as ticket area
- never attach product
- never proceed to checkout

## 8. Visual-Only Contract

Visual-only areas are layout context.

They may be visible in buyer map and buyer preview.

They must not be buyer-selectable unless explicitly converted to sellable and all buyer-selectable conditions pass.

Visual-only areas must not inherit product mapping from nearby sellable areas.

## 9. Standing Area Contract

Standing areas may be buyer-selectable when they are sellable and product-mapped.

Standing does not mean skip selection.

If a visual layout exists and a standing area is part of the buyer map, the buyer flow should expose area selection when selection is needed for product correctness.

Standing area selection must be deterministic across:

- mobile buyer flow
- dashboard buyer preview
- web companion buyer view, if present

## 10. Product Mapping Contract

Product mapping is the authority for commerce.

A buyer-selectable area must map to the correct ticket or reservation product.

Split parent/child behavior:

- split child sellable areas require valid child mapping or deterministic inherited mapping only if explicitly allowed
- split parent cells must not silently route to wrong child product
- invalid product remap must block publish or buyer selection
- facility and visual-only areas must not receive product mapping as purchase targets

## 11. Buyer Tap Resolver Contract

Buyer tap/hit-test resolver must return one of these deterministic outcomes:

| Outcome | Meaning |
| --- | --- |
| `selectable_area` | valid sellable buyer area |
| `non_selectable_visual` | visible but not buyer-selectable |
| `facility_marker` | visible informational marker |
| `disabled_area` | known but disabled |
| `miss` | no buyer target |

Only `selectable_area` may proceed to area selection, product selection, checkout, reservation request, ticket request, or claim flow.

## 12. Preview Parity

Dashboard buyer preview and mobile buyer map must follow the same buyer selection contract.

A preview may show editor metadata, but it must not imply buyer purchase availability when the buyer contract would deny selection.

## 13. Publish Safety

Publish validation should block or warn when:

- a sellable area has no valid product/admission mapping
- a product references a missing or invalid area key
- a split parent/child mapping is ambiguous
- visual-only/facility objects are accidentally mapped as buyer purchase targets
- buyer preview differs from buyer runtime contract

## 14. Non-Goals

This decision does not require:

- full venue editor redesign
- new commerce model
- new seating engine
- DB schema changes by default
- applying parked corrupted buyer-flow patches
- rewriting the mobile buyer flow wholesale

## 15. Implementation Order

Required implementation order:

1. verify existing dashboard/mobile behavior against this contract
2. harden mobile buyer resolver
3. harden dashboard buyer preview
4. harden publish validation
5. improve venue editor workflow
6. only then consider broader UX polish

## 16. Release Rule

JoinFolk v1 must not claim deterministic buyer commerce readiness unless this contract is honored by mobile buyer flow, dashboard buyer preview, and publish validation.

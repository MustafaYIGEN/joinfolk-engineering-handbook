# Visual Venue Editor

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Prior audit summary + Proposed P0C-C plan
- canonical: false

## 2. Purpose

This document is a platform-level draft specification for JoinFolk visual venue editor concepts.

This is a handbook draft. It is not a code audit and is not an accepted implementation contract. P0C-C Venue Editor Shape, Interaction, and Split Polish is a proposed patch plan, not accepted behavior. All P0C-C behavior in this document is labeled Proposed / Needs verification.

This document does not define exact schema, component ownership, geometry behavior, split math, path data format, hit polygon rules, product-section mapping behavior, buyer preview parity behavior, permission rules, RPC contracts, or RLS policies.

## 3. Visual Venue Editor Definition

JoinFolk has visual venue editor concepts within the broader Venue System and dashboard venue/business tooling.

Known facts:

- JoinFolk has a Venue System.
- JoinFolk has dashboard venue/business tooling.
- JoinFolk has visual venue editor concepts.
- Dashboard UI may expose editor behavior.
- Frontend editor behavior is UX only where security-sensitive.
- Backend/RPC/RLS must enforce security-sensitive behavior.

Unknown / needs verification:

- Exact visual venue editor schema.
- Exact dashboard route/component/service ownership.
- Exact backend/RPC/RLS relationships.
- Exact accepted implementation behavior.

## 4. Authority Model

### What frontend/editor may own

Frontend/editor surfaces may own user experience concerns, subject to backend/RPC/RLS enforcement for security-sensitive behavior.

Frontend/editor-owned behavior may include:

- Canvas presentation.
- Editor interaction UX.
- Selection UX.
- Local draft state.
- Visual preview UX.
- Client-side validation for usability.
- Loading, empty, and error states.
- Dashboard workflow composition.

Unknown / needs verification:

- Exact component ownership.
- Exact route ownership.
- Exact editor state model.
- Exact canvas interaction behavior.
- Exact buyer preview parity behavior.

### What backend/RPC/RLS must enforce

Backend, RPC, and RLS must enforce security-sensitive visual venue editor behavior.

Security-sensitive behavior includes:

- Editor access authority.
- Venue/business access authority.
- Product-section mapping authority.
- Ticket product relationships where security-sensitive.
- Reservation relationships where applicable and security-sensitive.
- Buyer/public preview exposure where applicable and security-sensitive.
- Any behavior that affects product, ticket, reservation, ownership, viewer-role, persona/tier, or venue/business rules.

Unknown / needs verification:

- Exact editor permission model.
- Exact RPC contracts.
- Exact RLS policies.
- Exact backend authority boundaries.
- Exact persistence model.

### What must never be frontend-only

The following must never rely only on frontend/editor checks:

- Security-sensitive product/ticket/reservation/ownership rules.
- Product-section mapping authority.
- Ticket product authority.
- Reservation authority where applicable.
- Buyer/public preview exposure where security-sensitive.
- Viewer-role, persona, or tier authorization.
- Venue/business access control.
- Any backend/RPC/RLS relationship for persisted editor behavior.

Visual editor behavior must not become the sole authority for security-sensitive product, ticket, reservation, or ownership rules.

## 5. Known Visual Venue Concepts Draft

Prior project context mentioned visual venue editor concepts such as:

- Visual templates.
- Visual areas.
- Sellable areas.
- Custom templates.
- Host-added areas.
- Custom overlays.
- Stage/DJ/focal areas.
- Shape rendering.
- Rectangle/circle/arc/path geometry.
- Local path data with transform.
- Hit polygons.
- Split engine.
- Buyer preview.
- `geoHash`/parity.
- Template registry.
- Product-section mapping.

These are known concepts only and are not accepted canonical schema, behavior, or implementation contracts until verified.

## 6. Known Component / Helper / Concept Names Draft

Prior proposed P0C-C plan mentioned component or helper names such as:

- `venueHelpers.ts`
- `VisualVenueCanvas.tsx`
- `VisualVenueEditor.tsx`
- `venueCellSplitEngine.ts`
- `venueCellClassifier.ts`
- `CellEditorPanel.tsx`
- `visualTemplateRegistry.ts`
- `BuyerPreviewPanel.tsx`

These names are known concepts only and must not be treated as accepted canonical file ownership or implementation contracts until verified.

Prior proposed P0C-C plan mentioned functions or concepts such as:

- `canMoveVisualArea`
- `canResizeVisualArea`
- `canMutateVisualGeometry`
- `handleCreateCustomArea`
- `onCreateArea`
- `onDeleteSelected`
- `classifyEditorCell`
- `splitCircle`
- `splitArc`
- `allocateUniqueAreaShortCode`
- `geoHash`

These names are known concepts only and must not be treated as accepted canonical APIs until verified.

## 7. Non-Accepted Visual Venue Editor Areas

The following areas are not accepted yet:

- Exact visual venue editor schema.
- Exact visual area schema.
- Exact template schema.
- Exact shape model.
- Exact geometry model.
- Exact coordinate mode behavior.
- Exact split behavior.
- Exact hit polygon behavior.
- Exact shortCode allocation behavior.
- Exact product-section mapping behavior.
- Exact buyer preview parity behavior.
- Exact template status/release guard behavior.
- Exact editor permission model.
- Exact dashboard route/component/service ownership.
- Exact backend/RPC/RLS relationships.

These areas must remain Unknown / Needs verification until verified through accepted source material.

## 8. Template / Registry Draft

Known concepts:

- Visual templates.
- Custom templates.
- Template registry.

Proposed / Needs verification:

- Template registry may need active/beta/hidden release guard behavior.

Unknown / needs verification:

- Exact template schema.
- Exact template registry behavior.
- Exact custom template behavior.
- Exact template status/release guard behavior.
- Whether template behavior affects buyer preview, product-section mapping, ticket products, reservations, or public sharing.

No exact template or registry behavior is accepted in this draft.

## 9. Visual Area / Geometry Draft

Known concepts:

- Visual areas.
- Sellable areas.
- Host-added areas.
- Stage/DJ/focal areas.
- Shape rendering.
- Rectangle/circle/arc/path geometry.
- Local path data with transform.

Unknown / needs verification:

- Exact visual area schema.
- Exact shape model.
- Exact geometry model.
- Exact coordinate mode behavior.
- Exact pathData format.
- Exact transform behavior.
- Whether visual areas are sellable, visual-only, host-added, template-derived, or another accepted model.
- Whether geometry affects product-section mapping, buyer preview, reservations, ticketing, or public sharing.

No exact visual area or geometry behavior is accepted in this draft.

## 10. Shape Creation / Type Preservation Draft

Known facts:

- Shape creation and type preservation are determinism-sensitive.

Proposed / Needs verification:

- Shape rendering may need normalization, including avoiding unintended rounded-rectangle rendering for circles/arcs.
- Circle/arc creation may need to preserve shape/type as path/arc/circle data instead of falling back to rect.

Unknown / needs verification:

- Exact shape creation behavior.
- Exact type preservation behavior.
- Exact accepted shape types.
- Exact shape rendering behavior.
- Exact path/arc/circle data model.
- Exact fallback behavior, if any.

No exact shape creation or type preservation behavior is accepted in this draft.

## 11. Canvas Interaction Draft

Known facts:

- Dashboard UI may expose editor behavior.
- Frontend editor behavior is UX only where security-sensitive.

Proposed / Needs verification:

- Canvas usability may include Escape to clear selection.
- Canvas usability may include Delete/Backspace for host-added deletion.
- Canvas usability may include Space + drag panning.
- Canvas usability may include hiding resize handles when resize is not allowed.

Unknown / needs verification:

- Exact canvas interaction behavior.
- Exact selection behavior.
- Exact delete behavior.
- Exact panning behavior.
- Exact resize-handle behavior.
- Exact keyboard shortcut behavior.
- Exact permission rules for move, resize, mutation, creation, or deletion.

No exact canvas interaction behavior is accepted in this draft.

## 12. Split Engine Draft

Known facts:

- Geometry/split behavior is determinism-sensitive.

Known concepts:

- Split engine.
- Rectangle/circle/arc/path geometry.

Proposed / Needs verification:

- Non-rect split behavior may include circle pizza/ring split and arc parallel/perpendicular split.
- Splitting SVG arcs and circles may require generating new pathData in local coordinate space.
- Split children may need unique ids and shortCodes.
- Cell editor split controls may need shape-specific options.

Unknown / needs verification:

- Exact split behavior.
- Exact split math.
- Exact split types.
- Exact pathData generation behavior.
- Exact shortCode allocation behavior.
- Exact child id allocation behavior.
- Exact cell editor split controls.
- Whether split behavior affects product-section mapping, ticket products, buyer preview, reservations, or public sharing.

No exact split engine behavior is accepted in this draft.

## 13. Hit Polygon / Selection Draft

Known concepts:

- Hit polygons.
- Shape rendering.
- Rectangle/circle/arc/path geometry.

Proposed / Needs verification:

- Hit polygons may be approximated for curved shapes.

Unknown / needs verification:

- Exact hit polygon behavior.
- Exact selection behavior.
- Exact approximation rules.
- Exact relationship between hit polygons, rendered shapes, split children, buyer preview, and editor interactions.

No exact hit polygon or selection behavior is accepted in this draft.

## 14. Product-Section Mapping Draft

Known facts:

- Product-section mapping is a high-risk area.
- Visual venue editor behavior may interact with product-section mapping.
- Visual editor behavior must not become the sole authority for security-sensitive product/ticket/reservation/ownership rules.

Unknown / needs verification:

- Exact product-section mapping behavior.
- Whether product-section mapping is editor-owned, backend-owned, ticketing-owned, venue-owned, or shared.
- Whether product-section mapping affects ticket products, buyer preview, reservations, commerce, or public sharing.
- Which backend/RPC/RLS behavior enforces product-section mapping.
- Which dashboard/editor behavior displays or edits product-section mapping.

No exact product-section mapping behavior is accepted in this draft.

## 15. Buyer Preview / Parity Draft

Known facts:

- Buyer preview parity is a high-risk area.
- Visual venue editor behavior may interact with buyer/public preview where applicable.

Proposed / Needs verification:

- Buyer preview parity may need to track geometry fields such as width, height, pathData, and rotation.

Unknown / needs verification:

- Exact buyer preview behavior.
- Exact buyer preview parity behavior.
- Exact fields required for parity.
- Whether buyer preview displays sellable areas, visual-only areas, custom overlays, host-added areas, stage/DJ/focal areas, product-section mapping, ticket products, reservations, media/gallery, or public sharing state.
- Which backend/RPC/RLS behavior enforces buyer/public preview exposure where security-sensitive.

No exact buyer preview or parity behavior is accepted in this draft.

## 16. Custom Overlay / Host-Added Area Draft

Known concepts:

- Custom overlays.
- Host-added areas.
- Stage/DJ/focal areas.
- Custom templates.

Proposed / Needs verification:

- Host-added stage, DJ booth, and focal areas may need to remain editable in Custom templates.

Unknown / needs verification:

- Exact custom overlay behavior.
- Exact host-added area behavior.
- Exact stage/DJ/focal area behavior.
- Whether host-added areas are sellable, visual-only, editor-only, exportable, or another accepted model.
- Whether host-added areas appear in buyer preview or public sharing.
- Which backend/RPC/RLS behavior enforces security-sensitive host-added area behavior.

No exact custom overlay or host-added area behavior is accepted in this draft.

## 17. Release Guard Draft

Known concepts:

- Template registry.
- Template status/release guard behavior.

Proposed / Needs verification:

- Template registry may need active/beta/hidden release guard behavior.

Unknown / needs verification:

- Exact release guard behavior.
- Exact template status model.
- Whether release guard behavior affects dashboard editor, buyer preview, Web/Public, Supabase backend, product-section mapping, ticket products, or reservations.
- Which surface or backend contract is authoritative for release guard behavior.

No exact release guard behavior is accepted in this draft.

## 18. Relationship to Product Domains

### Venue/business tools

Known relationship:

- Visual venue editor behavior may interact with venue/business tools.
- JoinFolk has dashboard venue/business tooling.

Unknown / needs verification:

- Exact relationship between visual venue editor behavior and venue/business tools.

### Ticketing

Known relationship:

- Visual venue editor behavior may interact with ticketing.

Unknown / needs verification:

- Exact ticketing relationship.
- Whether visual areas, product-section mapping, split behavior, or buyer preview affect ticketing.

### Ticket products

Known relationship:

- Visual venue editor behavior may interact with ticket products.

Unknown / needs verification:

- Exact ticket product relationship.
- Whether ticket products map to visual areas, sections, templates, split children, or buyer preview.

### Product-section mapping

Known relationship:

- Visual venue editor behavior may interact with product-section mapping.
- Product-section mapping is a high-risk area.

Unknown / needs verification:

- Exact product-section mapping behavior and authority.

### Reservations

Known relationship:

- Visual venue editor behavior may interact with reservations where applicable.

Unknown / needs verification:

- Whether reservations interact with visual areas, sellable areas, split behavior, buyer preview, ticket products, or public sharing.

### Buyer/public preview

Known relationship:

- Visual venue editor behavior may interact with buyer/public preview where applicable.
- Buyer preview parity is a high-risk area.

Unknown / needs verification:

- Exact buyer/public preview relationship and parity requirements.

### Media/gallery

Known relationship:

- Visual venue editor behavior may interact with media/gallery where applicable.

Unknown / needs verification:

- Whether media/gallery affects templates, visual areas, overlays, buyer preview, dashboard editor, or public sharing.

### Event lifecycle

Known relationship:

- Visual venue editor behavior may interact with event lifecycle where applicable.

Unknown / needs verification:

- Whether event lifecycle affects editor access, template use, product-section mapping, ticket products, reservations, buyer preview, or public sharing.

### Viewer roles

Known relationship:

- Visual venue editor behavior may interact with viewer roles.

Unknown / needs verification:

- Exact viewer-role rules for editor access, template use, geometry mutation, split behavior, product-section mapping, buyer preview, and public sharing.

### Personas and tiers

Known relationship:

- Visual venue editor behavior may interact with personas and tiers.

Unknown / needs verification:

- Whether personas or tiers affect editor access, template use, visual areas, product-section mapping, buyer preview, or public sharing.

### Supabase backend

Known relationship:

- Visual venue editor behavior may interact with Supabase backend/RPC/RLS where security-sensitive.
- Backend/RPC/RLS must enforce security-sensitive behavior.

Unknown / needs verification:

- Exact backend/RPC/RLS relationships.
- Exact schema, RPC parameters, return shapes, and RLS policies.

## 19. Cross-Surface Consistency Requirements

### Dashboard editor

Known facts:

- Dashboard UI may expose editor behavior.
- Frontend editor behavior is UX only where security-sensitive.

Unknown / needs verification:

- Exact dashboard editor behavior.
- Exact dashboard route/component/service ownership.
- Exact relationship between dashboard editor state and backend persisted state.

### Buyer preview

Known facts:

- Buyer preview parity is a high-risk area.

Proposed / Needs verification:

- Buyer preview parity may need to track geometry fields such as width, height, pathData, and rotation.

Unknown / needs verification:

- Exact buyer preview parity behavior.
- Exact geometry fields required for parity.
- Exact buyer preview exposure rules.

### Web/Public where applicable

Known relationship:

- Visual venue editor behavior may interact with buyer/public preview where applicable.

Unknown / needs verification:

- Whether Web/Public exposes visual venue editor output.
- Exact public visibility rules.
- Exact public relationship to product-section mapping, ticket products, reservations, media/gallery, and event lifecycle.

### Supabase Backend

Known requirement:

- Backend/RPC/RLS must enforce security-sensitive behavior.

Unknown / needs verification:

- Exact schema.
- Exact RPC contracts.
- Exact RLS policies.
- Exact backend ownership boundaries.
- Exact enforcement model for editor permissions, product-section mapping, buyer preview exposure, ticket products, reservations, and persisted geometry.

## 20. Security Risks

Known risks:

- Product-section mapping is a high-risk area.
- Buyer preview parity is a high-risk area.
- Backend/RPC/RLS must enforce security-sensitive behavior.
- Frontend editor behavior is UX only where security-sensitive.
- Visual editor behavior must not become the sole authority for security-sensitive product/ticket/reservation/ownership rules.

Security risks to verify:

- Unauthorized editor access.
- Unauthorized venue/business editing.
- Unauthorized product-section mapping changes.
- Unauthorized ticket product relationships.
- Unauthorized reservation relationships where applicable.
- Unauthorized buyer/public preview exposure.
- Frontend-only checks being treated as enforcement.
- Visual editor geometry being treated as authoritative for security-sensitive product, ticket, reservation, or ownership behavior.

## 21. Determinism Risks

Known risks:

- Geometry/split behavior is determinism-sensitive.
- Shape creation and type preservation are determinism-sensitive.
- Product-section mapping is a high-risk area.
- Buyer preview parity is a high-risk area.

Determinism risks to verify:

- Shape creation producing different shape types than intended.
- Circle or arc data falling back to rectangle behavior.
- Split behavior producing inconsistent child geometry, ids, or shortCodes.
- Hit polygons diverging from rendered geometry.
- Buyer preview geometry diverging from dashboard editor geometry.
- Product-section mapping diverging from visual areas or split children.
- `geoHash`/parity behavior producing inconsistent comparisons.

## 22. Maintainability Risks

Known risks:

- Exact dashboard route/component/service ownership is not accepted yet.
- Exact backend/RPC/RLS relationships are not accepted yet.
- Prior component/helper/function names are known concepts only, not accepted contracts.

Maintainability risks to verify:

- Component, helper, or function names being treated as canonical before verification.
- Geometry, split, selection, product-section mapping, and buyer preview logic duplicated without accepted ownership.
- Editor UX code encoding security-sensitive rules.
- Proposed P0C-C behavior being mistaken for accepted implementation behavior.
- Template registry and release guard behavior being implemented without accepted ownership.

## 23. Current Known Implementation

Current accepted implementation knowledge is limited to the facts in this draft:

- JoinFolk has a Venue System.
- JoinFolk has dashboard venue/business tooling.
- JoinFolk has visual venue editor concepts.
- Dashboard UI may expose editor behavior.
- Frontend editor behavior is UX only where security-sensitive.
- Backend/RPC/RLS must enforce security-sensitive behavior.
- Product-section mapping and buyer preview parity are high-risk areas.
- Geometry/split behavior and shape creation/type preservation are determinism-sensitive.
- Prior project context mentioned visual venue editor concepts, component/helper names, and function/concept names, but none are accepted canonical contracts.

Unknown / needs verification:

- Exact accepted implementation across dashboard editor, buyer preview, Web/Public where applicable, Supabase backend, venue/business tools, ticketing, ticket products, product-section mapping, reservations, media/gallery, event lifecycle, viewer roles, and personas/tiers.

## 24. Proposed P0C-C Notes

The following notes are Proposed / Needs verification only. They are not accepted behavior:

- Host-added stage, DJ booth, and focal areas may need to remain editable in Custom templates.
- Shape rendering may need normalization, including avoiding unintended rounded-rectangle rendering for circles/arcs.
- Canvas usability may include Escape to clear selection, Delete/Backspace for host-added deletion, Space + drag panning, and hiding resize handles when resize is not allowed.
- Circle/arc creation may need to preserve shape/type as path/arc/circle data instead of falling back to rect.
- Non-rect split behavior may include circle pizza/ring split and arc parallel/perpendicular split.
- Splitting SVG arcs and circles may require generating new pathData in local coordinate space.
- Hit polygons may be approximated for curved shapes.
- Split children may need unique ids and shortCodes.
- Cell editor split controls may need shape-specific options.
- Template registry may need active/beta/hidden release guard behavior.
- Buyer preview parity may need to track geometry fields such as width, height, pathData, and rotation.

Exact behavior for all proposed P0C-C notes is not accepted.

## 25. Unknowns / Needs Verification

The following must be verified before v1.0:

- Exact visual venue editor schema.
- Exact visual area schema.
- Exact template schema.
- Exact shape model.
- Exact geometry model.
- Exact coordinate mode behavior.
- Exact split behavior.
- Exact hit polygon behavior.
- Exact shortCode allocation behavior.
- Exact product-section mapping behavior.
- Exact buyer preview parity behavior.
- Exact template status/release guard behavior.
- Exact editor permission model.
- Exact dashboard route/component/service ownership.
- Exact backend/RPC/RLS relationships.
- Exact component/helper/function ownership.
- Exact relationship to venue/business tools.
- Exact relationship to ticketing.
- Exact relationship to ticket products.
- Exact relationship to reservations where applicable.
- Exact relationship to media/gallery where applicable.
- Exact relationship to event lifecycle where applicable.
- Exact relationship to viewer roles.
- Exact relationship to personas and tiers.

## 26. Acceptance Criteria for v1.0

Visual Venue Editor v1.0 can be accepted only after verification establishes:

- Accepted visual venue editor domain vocabulary.
- Accepted visual venue editor schema.
- Accepted visual area schema.
- Accepted template schema.
- Accepted shape and geometry model.
- Accepted coordinate mode behavior.
- Accepted split behavior.
- Accepted hit polygon and selection behavior.
- Accepted shortCode allocation behavior.
- Accepted product-section mapping behavior and authority.
- Accepted buyer preview parity behavior.
- Accepted custom overlay and host-added area behavior.
- Accepted template status/release guard behavior.
- Accepted editor permission model.
- Accepted dashboard route/component/service ownership.
- Accepted backend/RPC/RLS contracts, including parameters, return shapes, errors, and authorization behavior.
- Accepted cross-surface ownership for dashboard editor, buyer preview, Web/Public where applicable, and Supabase backend.
- Accepted security-sensitive enforcement boundaries.
- Accepted maintainability ownership for editor UX, geometry, split behavior, product-section mapping, buyer preview parity, templates, release guards, and backend contracts.

Until these criteria are met, this document remains non-canonical.

## 27. Open Questions

- What is the accepted visual venue editor schema?
- What is the accepted visual area schema?
- What is the accepted template schema?
- What is the accepted shape model?
- What is the accepted geometry model?
- What is the accepted coordinate mode behavior?
- What is the accepted split behavior?
- What is the accepted hit polygon behavior?
- What is the accepted shortCode allocation behavior?
- What is the accepted product-section mapping behavior?
- What is the accepted buyer preview parity behavior?
- What is the accepted template status/release guard behavior?
- What is the accepted editor permission model?
- What dashboard routes, components, and services own visual venue editor behavior?
- What backend/RPC/RLS relationships are accepted?
- Ring/concentric split default: equal-width rings or equal-area rings?
- Should center disk have a different proportional radius?
- Should Custom template DJ/Stage objects be exportable to buyer preview or remain editor-only?
- If exportable, should they be represented as visual_only?
- How should proposed P0C-C behavior be verified before it becomes accepted handbook content?

# Web Dashboard Buyer Preview Parity Evidence Report

## 1. Status

- Completed

## 2. Scope

- Dashboard buyer preview parity only.
- Aligns dashboard preview interaction with BuyerSelectionContractDecision.
- Does not change mobile renderer.
- Does not change backend, Supabase, RLS, RPCs, migrations, or DB schema.

## 3. Decision Source

- `09_Decisions/BuyerSelectionContractDecision.md`
- Key rule: visible/editor-selectable does not mean buyer-selectable.
- Only valid sellable buyer areas should behave as selectable in buyer preview.
- Visual-only/facility/focal/gate/service/frame/disabled areas must remain visible but not buyer-selectable.

## 4. Repository Evidence

- Repo: `C:\dev\joinfolk-web`
- Branch: `refactor/joinfolk-stabilization-p0`
- Commit: `bf9ea17 fix(dashboard): align buyer preview selection contract`
- Full commit: `bf9ea179784b7f507319ec8cc2cf3997acdd9bd4`
- Tag: `joinfolk-v1-rc2.3-web-dashboard`
- Tag message: `JoinFolk v1 RC-2.3 web dashboard buyer preview parity`
- Branch push: PASS
- Tag push: PASS

## 5. Files Changed

- `dashboard/src/pages/venue/VisualVenueCanvas.tsx`
- `dashboard/src/pages/venue/VisualVenueEditor.tsx`
- `dashboard/src/pages/venue/BuyerPreviewPanel.tsx`

## 6. Implementation Summary

- `VisualVenueCanvas` preview selection now only treats valid sellable buyer areas as preview-selectable.
- Non-sellable roles are no longer selectable/glow-interactive in center canvas preview mode.
- Right `BuyerPreviewPanel` tab was decoupled from center canvas previewMode.
- Top/global Preview button still controls center canvas preview mode.
- Top/global Edit button still returns center canvas to edit mode.
- `BuyerPreviewPanel` custom overlay circle/ellipse render now respects center-origin geometry without affecting frame/path/polygon rendering.

## 7. Validation Evidence

- `git diff --check`: PASS
- Added-line mojibake scan: PASS
- Added-line console/AUDIT/TODO/FIXME scan: PASS
- `npm --prefix dashboard run build`: PASS
- Build modules: 314
- Build duration observed: 2.90s
- Existing chunk-size warning remains non-blocking.

## 8. Runtime QA Evidence

| Scenario | Result |
| --- | --- |
| Dashboard login opens | PASS |
| Venue editor opens | PASS |
| Right Preview tab opens BuyerPreviewPanel without forcing center canvas previewMode | PASS |
| Top Preview button enables center canvas previewMode | PASS |
| Sellable area is selectable in preview | PASS |
| Facility, visual-only, gate, focal, frame, and disabled areas are not selectable in preview | PASS |
| BuyerPreviewPanel custom circle/ellipse renders in the correct position | PASS |
| Frame rendering remains correct | PASS |
| Polygon/path/semicircle/arc rendering remains correct | PASS |
| Rect sellable rendering remains correct | PASS |

## 9. Mobile Impact

- No mobile files changed.
- Dashboard BuyerPreviewPanel DOM is not injected into mobile.
- Mobile continues to consume published layout/snapshot data through its own renderer.

## 10. Remaining Related Work

- B2: publish validation parity.
- Web Companion MVP remains separate.
- No additional source changes are authorized by this report.

## 11. Final Status

- Web dashboard buyer preview parity: CLOSED.
- RC-2.3 web/dashboard checkpoint: EVIDENCE READY.

# Web Dashboard Publish Validation Parity Evidence Report

## 1. Status

- Completed

## 2. Scope

- Dashboard publish/product validation parity only.
- Aligns publish validation with BuyerSelectionContractDecision.
- Does not change mobile renderer.
- Does not change backend, Supabase, RLS, RPCs, migrations, or DB schema.

## 3. Decision Source

- `09_Decisions/BuyerSelectionContractDecision.md`
- Key rule: visible/editor-selectable does not mean buyer-selectable.
- Only valid sellable buyer areas may be product targets and publish-selectable areas.
- Visual-only, facility, gate, focal, service, frame, disabled, and structural areas must not be publish-selectable commerce targets.

## 4. Repository Evidence

- Repo: `C:\dev\joinfolk-web`
- Branch: `refactor/joinfolk-stabilization-p0`
- Commit: `e9c9912 fix(dashboard): align publish validation buyer selection contract`
- Full commit: `e9c991284a7a594899f265defea46709ba7f1da7`
- Tag: `joinfolk-v1-rc2.4-web-dashboard`
- Tag message: `JoinFolk v1 RC-2.4 web dashboard publish validation parity`
- Branch push: PASS
- Tag push: PASS

## 5. Files Changed

- `dashboard/src/pages/venue/venueHelpers.ts`
- `dashboard/src/pages/venue/visualVenueValidationEngine.ts`

## 6. Implementation Summary

- `isBuyerSelectableArea` is now the strict central buyer-selectable contract gate.
- `isProductEligibleArea` now uses `isBuyerSelectableArea`.
- Product mapping validation now treats any non-buyer-selectable area reference as invalid for publish.
- Publish validation selectableAreas now uses `isBuyerSelectableArea`.
- Sellable area product coverage checks now use `isBuyerSelectableArea`.
- Existing split parent remap handling remains before generic non-selectable product reference handling.
- Products with `allowed_section_keys === null` were not changed in this patch.

## 7. Validation Evidence

- `git diff --check`: PASS
- Added-line mojibake scan: PASS
- Added-line console/AUDIT/TODO/FIXME scan: PASS
- `npm --prefix dashboard run build`: PASS
- Build modules: 314
- Build duration observed: 3.00s
- Existing chunk-size warning remains non-blocking.

## 8. Production Deployment Evidence

- Deployment type: Git-backed production deployment
- Commit: `e9c9912`
- Status: Success
- Alias/domain: `app.join-folk.com`

## 9. Runtime QA Evidence

| Scenario | Result |
| --- | --- |
| Existing valid event publish/apply-to-live was not broken | PASS |
| Visual/facility/frame-only layout is blocked from publish | PASS |
| Product-linked area changed to visual_only/gate/service/frame/disabled blocks publish | PASS |
| Valid sellable area with product mapping is not blocked from publish | PASS |

## 10. Mobile Impact

- No mobile files changed.
- Dashboard publish validation now aligns with the buyer selection contract used by dashboard preview and mobile buyer selection hardening.
- Mobile renderer was not changed.

## 11. Remaining Related Work

- Web Companion MVP remains separate.
- No additional source changes are authorized by this report.

## 12. Final Status

- Web dashboard publish validation parity: CLOSED.
- RC-2.4 web/dashboard checkpoint: EVIDENCE READY.

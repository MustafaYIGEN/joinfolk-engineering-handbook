# Event Visual Status Gates

## 1. Metadata

- Status: Accepted
- Version: 1.0
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-13
- canonical: true

## 2. Purpose

This status gate record captures the accepted JoinFolk event-visual product decision and the remaining implementation and release gates for image-or-video parity.

## 3. Status Gates

| Gate | State | Notes |
|---|---|---|
| EVENT_VISUAL_PRODUCT_DECISION | DECIDED | Creator-facing event visuals are IMAGE xor VIDEO by binding decision. |
| FEED_VIDEO_LOOP | OPEN_IMPLEMENTATION | Current feed player initializes `loop = false`; bounded loop-only fix remains open. |
| HOST_PRO_VIDEO_UI | OPEN_IMPLEMENTATION | Current mobile UI exposes video to all host tiers rather than host-plus-pro only. |
| HOST_PRO_VIDEO_BACKEND_ENFORCEMENT | OPEN_P0 | Backend/RPC enforcement for host-plus-pro entitlement is not yet proven. |
| VIDEO_AUTOMATIC_THUMBNAIL | OPEN_P0 | Automatic generated thumbnail is required as a canonical static fallback. |
| PUBLIC_VIDEO_TRANSCODING | OPEN_P0 | No proven active canonical derivative pipeline yet exists for final public launch. |
| RAW_MOV_PUBLIC_PLAYBACK | BLOCKED_FOR_FINAL_RELEASE | Raw MOV / HEVC / HDR input is not accepted as the final public feed source. |
| MOBILE_VIDEO_CREATE | PARTIAL_LEGACY_MODEL | Mobile currently supports poster image plus optional video rather than explicit IMAGE xor VIDEO. |
| DASHBOARD_VIDEO_CREATE | MISSING | Dashboard creation is currently image-only. |
| MOBILE_DASHBOARD_VISUAL_PARITY | OPEN | Mobile and dashboard do not yet support the same canonical visual states. |
| POSTER_VIDEO_DOMAIN_OVERALL | OPEN | The product decision is frozen, but implementation, processing, and parity remain open. |

## 4. Evidence Reference

- [09_Decisions/EventVisualCanonicalImageOrVideoDecision.md](../09_Decisions/EventVisualCanonicalImageOrVideoDecision.md)
- [08_PatchPlans/EventVisualCrossSurfaceClosurePatchPlan.md](../08_PatchPlans/EventVisualCrossSurfaceClosurePatchPlan.md)

## 5. Remaining Open Gates

- bounded feed loop fix;
- host-plus-pro UI restriction;
- host-plus-pro backend enforcement;
- automatic thumbnail generation;
- public canonical derivative processing;
- dashboard video creation parity;
- cross-surface compatibility and UAT.

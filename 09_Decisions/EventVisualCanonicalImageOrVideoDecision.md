# Event Visual Canonical Image-or-Video Decision

## 1. Metadata

- Status: Accepted
- Version: 1.0
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-13
- canonical: true
- Scope: event primary visual mode, feed playback, entitlement, cross-surface parity, and public video processing

## 2. Purpose

This decision records the binding JoinFolk product and architecture rules for event primary visuals across mobile, dashboard, feed playback, and public video processing.

It freezes the accepted product contract as creator-facing IMAGE xor VIDEO, authorizes a bounded feed loop fix, and records the remaining release gates that keep raw public video playback and cross-surface parity open.

## 3. Binding Decision

1. Every event MUST expose one creator-facing primary visual mode: `IMAGE` or `VIDEO`.
2. These modes MUST be mutually exclusive from the creator's perspective.
3. `IMAGE` mode MUST require a manually selected image and MUST NOT leave an active video state.
4. `VIDEO` mode MUST require a source video and MUST NOT require the creator to upload a second manual poster.
5. `VIDEO` mode MUST generate a static thumbnail as an internal derivative. The generated thumbnail is not a second creator-selected visual.
6. Unless a separate schema decision later proves otherwise, a new `visual_kind` database column is NOT authorized by this decision.
7. A visual surface MUST treat the event as video-primary only when `poster_video_status` is in its canonical ready state and `poster_video_path` is present. Otherwise the surface MUST fall back to the static poster/thumbnail path.
8. Event Detail MUST remain static-thumbnail-only for v1.
9. The active and focused feed card MUST autoplay and loop continuously. Video end MUST restart from zero and MUST NOT freeze on the final frame.
10. Inactive or unfocused cards MUST pause and release global playback ownership.
11. Returning to a card after it becomes inactive MUST begin a new playback session from zero.
12. The protected playback internals remain frozen. A bounded loop-only change in `PosterVideoPlayer.tsx` is explicitly authorized.
13. Video creation MUST be available only to host persona plus pro tier.
14. Personal persona combinations MUST be hidden and unauthorized for video creation.
15. Host `user` and host `semi_pro` MUST be hidden and unauthorized for video creation.
16. Backend or RPC enforcement for host-plus-pro entitlement is mandatory. UI-only gating is not sufficient.
17. Mobile and dashboard MUST converge on the same canonical visual contract for create, edit, replace, remove, and publish.
18. A surface MUST NOT publish a visual state that another surface cannot safely read.
19. Public video playback MUST use a canonical derivative: MP4, H.264 AVC, SDR, maximum 1080p, maximum 30 fps, AAC-LC, faststart.
20. A generated JPEG or WebP thumbnail is required for video events.
21. Raw MOV, HEVC, or HDR input MAY be retained as processing input, but it MUST NOT remain the canonical public feed source for final release.

## 4. Canonical Precedence

Creator-facing mode is mutually exclusive, but current storage remains additive.

Until a separate schema justification proves a better model, visual precedence is:

1. `VIDEO` when `poster_video_path` is present and `poster_video_status` is in the ready state accepted by runtime.
2. Static fallback from `poster_snapshot_url` or generated thumbnail when video is absent, not ready, failed, loading, or not allowed on the surface.

This keeps current fields usable without authorizing a schema change merely to represent the product decision.

## 5. Static Fallback Rule

Video events MUST have a generated static thumbnail for:

- feed loading and feed failure states;
- Event Detail;
- reminders;
- tickets and wallet;
- notification cards;
- dashboard lists;
- sharing / Open Graph;
- review and admin surfaces.

## 6. Actual Runtime Evidence Basis

The decision is grounded in the current verified implementation state:

- Mobile creation currently uses image plus optional video.
- Dashboard creation is currently image-only and requires a poster image.
- Current event/runtime fields are:
  - `events.poster_snapshot_url`
  - `events.poster_video_path`
  - `events.poster_video_thumb_url`
  - `events.poster_video_duration_ms`
  - `events.poster_video_status`
- Current video attach RPC is `attach_poster_video_v1`.
- Current storage bucket is `event-videos`.
- Current feed defect is explained directly by `PosterVideoPlayer` initializing `loop = false`.
- Current backend proves event ownership only; it does not prove host-plus-pro video entitlement.
- Raw MOV / QuickTime input is currently accepted.
- No active transcode worker or server-generated thumbnail pipeline is currently proven.

Current persisted status evidence proves the runtime distinguishes at least:

- no video conceptually;
- `processing`;
- `ready`;
- `failed`.

This document does not invent additional persisted enum values beyond the evidence already recorded.

## 7. Publish Contract

### IMAGE

- Image asset MUST exist.
- Active video state MUST NOT exist.

### VIDEO

- Source video upload MUST exist.
- Generated static thumbnail MUST exist before public rendering is considered complete.
- Canonical derivative readiness policy MUST be explicit in implementation.
- Raw source alone MUST NOT satisfy final public-launch PASS.

Processing and failed states MUST have deterministic UI, retry, and rollback behavior.

## 8. Compatibility Posture

Current implementation is transitional:

- mobile already supports additive poster image plus optional video;
- dashboard remains image-only;
- runtime can read video-backed feed items only on a subset of surfaces;
- public-launch-safe processing is not closed.

This decision therefore authorizes controlled convergence, not broad refactor.

## 9. Evidence Basis

This decision is based on the verified read-only audit recorded in:

- `EVENT-CREATION-VISUAL-01: Cross-Surface Image/Video Creation and Playback Audit`

Related handbook references:

- [08_PatchPlans/EventVisualCrossSurfaceClosurePatchPlan.md](../08_PatchPlans/EventVisualCrossSurfaceClosurePatchPlan.md)
- [10_Status/EventVisualStatusGates.md](../10_Status/EventVisualStatusGates.md)
- [01_Product/EventLifecycle.md](../01_Product/EventLifecycle.md)
- [01_Product/PersonasAndTiers.md](../01_Product/PersonasAndTiers.md)
- [03_Database/StorageModel.md](../03_Database/StorageModel.md)

## 10. Release Boundary

Final-release PASS is blocked until:

- the loop fix is implemented;
- host-plus-pro entitlement is enforced in UI and backend;
- automatic thumbnail generation exists;
- public video transcoding is implemented;
- mobile/dashboard parity is closed to the accepted level;
- raw public MOV playback is removed from the final public contract.

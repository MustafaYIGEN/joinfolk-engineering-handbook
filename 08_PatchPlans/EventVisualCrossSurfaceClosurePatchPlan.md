# Event Visual Cross-Surface Closure Patch Plan

## 1. Metadata

- Status: Draft / Controlled closure plan
- Version: 1.0
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-13
- canonical: true
- Scope: event image-or-video contract closure across mobile, dashboard, feed playback, entitlement, and public processing

## 2. Purpose

This patch plan records the controlled implementation waves required to close the accepted event-visual decision without broad feed-player or schema churn.

It does not authorize production SQL, migrations, deployment, or source changes by itself.

## 3. Controlled Waves

### PV-01 — Handbook decision and gates

- Record the accepted event visual product contract.
- Freeze the release gates and rollout dependencies.

### PV-02 — Bounded feed loop fix

- Implement the bounded loop-only change in `PosterVideoPlayer.tsx`.
- Preserve global owner lock, mute ownership, feed patching, lifecycle contract, and session dedupe.
- Verify active + focused cards loop continuously and inactive cards still pause and release ownership.

### PV-03 — Host-plus-pro UI and backend enforcement

- Restrict video controls to host persona plus pro tier.
- Hide video UI for personal persona, host `user`, and host `semi_pro`.
- Add backend or RPC enforcement so bypassed clients cannot attach video outside the accepted entitlement.

### PV-04 — Processing and thumbnail architecture

- Define and implement the canonical derivative pipeline.
- Generate thumbnail derivatives automatically.
- Make public feed playback consume only the accepted derivative source.
- Preserve raw upload only as processing input or retry material where needed.

### PV-05 — Mobile IMAGE xor VIDEO create/edit contract

- Convert mobile create/edit from additive poster-plus-video behavior into explicit IMAGE xor VIDEO creator flows.
- Keep static fallback generation automatic in video mode.
- Remove the creator requirement to supply a second manual poster in video mode.

### PV-06 — Dashboard IMAGE xor VIDEO parity

- Add dashboard support for the same create/edit/replace/remove/publish visual states as mobile.
- Prevent dashboard from remaining an image-only publisher if mobile can produce video-primary events.

### PV-07 — Cross-surface migration and compatibility validation

- Verify mobile, dashboard, and read surfaces can safely read the same visual states.
- Verify transitional data does not break Event Detail, reminders, tickets/wallet, notification cards, review/admin surfaces, or sharing surfaces.

### PV-08 — TestFlight, dashboard, and production UAT

- Run device and browser UAT for feed loop, entitlement, video create/edit, thumbnail fallback, dashboard parity, and public processing behavior.

### PV-09 — Final handbook closure

- Update final status gates only after implementation and UAT evidence is complete.

## 4. Prerequisites

- Event visual decision accepted.
- Read-only audit evidence preserved.
- Protected playback internals remain frozen except for the bounded loop fix.
- Entitlement contract is frozen as host persona plus pro tier only.
- Public-launch derivative contract is frozen as MP4 / H.264 / SDR / 1080p max / 30 fps max / AAC-LC / faststart.

## 5. Rollout Order

1. Freeze handbook decision and status gates.
2. Implement and validate the bounded feed loop fix.
3. Implement host-plus-pro UI gating and backend enforcement.
4. Implement automatic thumbnail and canonical derivative processing.
5. Convert mobile create/edit to explicit IMAGE xor VIDEO flows.
6. Add dashboard parity for the same visual states.
7. Run cross-surface compatibility validation.
8. Run TestFlight, dashboard, and production UAT.
9. Close handbook status only after the accepted gates pass.

## 6. Rollback

- If the loop fix regresses playback ownership, pause at PV-02 and keep the frozen internals intact.
- If entitlement enforcement blocks legitimate host-pro flows, keep the product contract but pause rollout until UI and backend agree.
- If processing/transcoding is incomplete or unstable, keep raw video out of final public-release PASS and do not broaden unsupported fallback.
- If dashboard parity lags behind mobile, treat parity as open and do not mark the domain closed.

## 7. Verification Categories

- Feed loop behavior.
- Active/inactive playback ownership.
- Host-plus-pro entitlement enforcement.
- Mobile IMAGE xor VIDEO create/edit behavior.
- Dashboard IMAGE xor VIDEO parity.
- Automatic thumbnail generation.
- Canonical derivative processing and playback.
- Transitional compatibility across non-feed surfaces.
- Device/browser UAT.

## 8. Acceptance Criteria

This patch plan is complete only when:

- feed video loops without final-frame freeze;
- video creation is restricted to host persona plus pro tier in UI and backend;
- video events receive automatic static thumbnails;
- public playback uses the accepted derivative contract;
- mobile and dashboard can create, edit, replace, remove, and publish under the same visual contract;
- transitional data remains safe on all static-thumbnail surfaces;
- raw public MOV playback is no longer part of the final release contract.

## 9. Prohibited Actions

- No broad feed-player rewrite.
- No protected playback-internal changes beyond the bounded loop fix unless a separate deterministic defect is proven.
- No reintroduction of a second required manual poster in video mode.
- No schema change such as `visual_kind` without separate justification.
- No final-release PASS while raw public MOV playback remains in the public path.

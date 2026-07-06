# Production Migration Provenance Report

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Local read-only migration provenance check
- canonical: false

## 2. Purpose

This document is a draft production migration provenance report for JoinFolk Supabase Database Functions / RPCs.

It is not a canonical source-of-truth decision, not a cleanup plan, not a patch plan, and not a migration plan. It does not authorize modifying any Supabase tree and does not authorize proof check-in hardening implementation.

This report separates production Database Functions / RPC evidence from Edge Functions, and separates local migration provenance from production deployment-path confirmation. Production SQL evidence remains valid separately from local source-path ambiguity.

## 3. Provenance Scope

The operator viewed a Supabase Database Functions / RPC list. The supplied list is Database Functions / RPCs, not Supabase Edge Functions.

A read-only local provenance check compared 55 production RPC/function names against three local migration trees:

1. `C:\dev\hostos\supabase\migrations`
2. `C:\dev\joinfolk-web\supabase\migrations`
3. `C:\dev\hostos\apps\mobile\supabase\migrations`

Scope constraints:

- The check did not inspect production directly.
- The check did not inspect Edge Function deployment.
- The check did not authorize migration or patch implementation.
- No final canonical source-of-truth is claimed.

## 4. Production Function Evidence Type

The supplied Supabase UI list is Database Functions / RPCs, not Edge Functions.

Implications:

- RPC provenance can be compared against local SQL migration histories.
- RPC provenance does not prove Supabase Edge Function deployment.
- Edge Function folders such as `push-dispatch`, `send-test-push`, `snapshot`, `transactional-email`, `notify-transfer-invite`, and `notify-transfer-review` remain separate from this RPC provenance check.
- Production SQL evidence remains stronger than local source assumptions for production-state claims.

## 5. Migration Tree Inventory

### C:\dev\hostos\supabase\migrations

- Count: 186
- Notes: broadest/current backend-looking tree.
- Latest examples:
  - `20260606_blueprint_topology_meta_patch.sql`
  - `20260606_commerce_eligibility_guard.sql`
  - `20260606_event_product_section_usage_v1.sql`
  - `20260606_exact_seat_purchase_guard.sql`
  - `20260606_publish_readiness_guard.sql`
  - `20260606_ticket_section_context.sql`
  - `20260607_ticket_product_upsert_null_clear_semantics.sql`
  - `20260618_p0_cross_entitlement_guard_v1.sql`
  - `20260619123535_fix_venue_media_storage_upload_policy.sql`
  - `20260626_commerce_standing_tickets_v1.sql`

### C:\dev\joinfolk-web\supabase\migrations

- Count: 54
- Notes: strong host-transfer/ops signal; includes temp file.
- Latest examples:
  - `20260326_spatial_foundation_v1.sql`
  - `20260410_purchase_zone_intent_v1.sql`
  - `20260411_buyer_zones_leaf_only.sql`
  - `20260412_buyer_v4_priority_v1.sql`
  - `20260412_persistence_truth_repair_v1.sql`
  - `20260412_restore_buyer_v4_containers.sql`
  - `20260627_ops_media_drafts_v1.sql`
  - `20260628_host_identity_transfer_v1.sql`
  - `20260628_host_identity_transfer_v1_1_persona.sql`
  - `temp_rpc.sql`

### C:\dev\hostos\apps\mobile\supabase\migrations

- Count: 103
- Notes: mobile/component-specific and proof/transfer-heavy signal.
- Latest examples:
  - `20260503_host_transfer_phase2.sql`
  - `20260503_host_transfer_phase3.sql`
  - `20260503_host_transfer_phase4.sql`
  - `20260503_tier_mutation_lockdown.sql`
  - `20260509_photo_feed_city_events_patch.sql`
  - `20260510_allow_event_audience_share_group_kind.sql`
  - `20260512_app_diagnostics.sql`
  - `20260512_build19_app_diagnostics_anon_policy.sql`
  - `20260611_frame_slot_guard_fix_v1.sql`
  - `20260611_visual_topology_meta_v1.sql`

## 6. Function Match Summary

Path-level match summary:

- Searched function count: 55
- Matched in `hostos`: 35
- Matched in `joinfolk-web`: 16
- Matched in mobile: 28
- Unique to `hostos`: 18
- Unique to `joinfolk-web`: 8
- Unique to mobile: 9
- Unmatched everywhere: `public_verify_checkin`

Overlap patterns:

- `hostos` only: 18
- `joinfolk-web` only: 8
- Mobile only: 9
- `hostos` + mobile: 11
- `joinfolk-web` + mobile: 2
- All three: 6

Interpretation:

- `hostos\supabase\migrations` is the strongest overall migration-source candidate for the current production RPC surface.
- Split-source evidence exists.
- The match summary does not prove final canonical deployment source.

## 7. Domain-Level Provenance Assessment

### Transfer / Ops

- Strongest source tree: split between `joinfolk-web` and mobile.
- Supporting files:
  - `joinfolk-web`: `20260627_ops_media_drafts_v1.sql`, `20260628_host_identity_transfer_v1.sql`, `20260628_host_identity_transfer_v1_1_persona.sql`
  - Mobile: `20260503_host_transfer_phase1.sql` through `20260503_host_transfer_phase4.sql`
- Confidence: Medium-High.
- Caveat: split-source history, not canonical deployment source.

Interpretation: `joinfolk-web` strongly implicates host identity transfer / ops migration history.

### Commerce / Entitlement

- Strongest source tree: `hostos`.
- Supporting files:
  - `20260606_exact_seat_purchase_guard.sql`
  - `20260618_p0_cross_entitlement_guard_v1.sql`
  - `20260626_commerce_standing_tickets_v1.sql`
  - `20260605` / `20260606` commerce guard files
- Confidence: High for current production-style commerce/standing-ticket provenance.

### Check-In / Proof

- Strongest source tree: split.
- Mobile strongest for proof-specific helpers.
- `hostos` strongest for older/core check-in.
- Supporting files:
  - `hostos`: `20260216_checkin_proofs.sql`, `20260210` / `20260211` check-in files, `20260312_staff_checkin_rpc.sql`
  - Mobile: `20260330_phase6_checkin_proof_normalization*.sql`, `20260330_phase7_proof_readback_error_mapping.sql`
- Confidence: Medium.
- Caveat: `public_verify_checkin` was not found in any local tree.

Interpretation: proof-specific helpers strongly implicate mobile migration history, while core check-in provenance also points to `hostos`.

### Venue / Media / Visual

- Strongest source tree: split, but `hostos` best explains current venue/media/commerce RPCs.
- Supporting files:
  - `hostos`: `20260505_venue_commerce_v1_rpcs_part*.sql`, `20260515_canonical_templates_v1.sql`, `20260606_blueprint_topology_meta_patch.sql`, `20260619123535_fix_venue_media_storage_upload_policy.sql`
  - `joinfolk-web`: buyer zones, seat availability, layout persistence files
  - Mobile: topology and `create_event_media_v2` files
- Confidence: Medium.

### Notification / Push

- Strongest source tree: `hostos`.
- Supporting files:
  - `20260419_push_tokens_v1.sql`
  - `20260419_notification_settings_v1.sql`
  - `20260504_notifications_v2_foundation.sql`
  - `20260512_build20b_push_eligibility.sql`
  - `20260512_build20b_fix_push_function_grants.sql`
- Confidence: High.

## 8. Classification Update

### hostos\supabase\migrations

- Classification: strongest overall migration-source candidate for current production RPC surface.
- Confidence: Medium-High.
- Evidence: most total matches, all notification/push DB functions, strongest current commerce/standing-ticket and venue/media matches.
- Caveat: does not explain host identity transfer/ops or proof helpers alone.
- Canonical status: not final canonical.

### joinfolk-web\supabase\migrations

- Classification: active in-progress or split-source host-transfer/visual source.
- Confidence: Medium.
- Evidence: strongest match for `admin_execute_host_identity_transfer_v1`, ops transfer functions, buyer-zone/seat functions.
- Caveat: partial tree, includes `temp_rpc.sql`, not enough to call canonical.

### mobile\supabase\migrations

- Classification: component-specific or historical mobile-coupled source with real production-RPC overlap.
- Confidence: Medium.
- Evidence: strong transfer phase files, proof normalization/readback functions, some visual/topology functions.
- Caveat: nested repo; may be stale or component-specific rather than canonical.

## 9. Impact on Existing Handbook Docs

| Document | Impact | Updated note |
| --- | --- | --- |
| `RepositorySourceMap.md` | Strengthened | “hostos is primary plausible, not canonical” remains correct. |
| `SupabaseSourceOfTruthClassification.md` | Strengthened | Keep split-source caveat. `hostos` is strongest overall RPC provenance candidate, but not sole source. |
| `ManualDeploymentPathConfirmationReport.md` | Still needed | RPC provenance cannot replace deployment path confirmation. |
| `SupabaseBackendGapReport` | Medium for local provenance | Local `hostos\supabase` evidence is strengthened overall, but split-source caveats remain. |
| `SupabaseProductionParityVerificationReport` | Low for production SQL | Production SQL evidence remains separate and valid. |
| `SupabaseFocusedBackendFollowUpReport` | Low to Medium | Focused SQL findings remain mostly unaffected; proof/check-in source provenance remains split. |
| `SupabaseProofCheckinRpcHardeningPlan.md` | Medium | Target readiness improves only slightly. `hostos` can be named as likely overall migration target, but proof-specific helpers strongly implicate mobile. Implementation remains blocked until operator accepts target path. |

## 10. Updated Working Rule

- Treat `C:\dev\hostos\supabase\migrations` as the strongest migration-source candidate for current production RPC surface.
- Do not call it final canonical source.
- Recognize split-source evidence across `hostos`, `joinfolk-web`, and mobile migration trees.
- Proof check-in patch planning may say “likely target: `hostos\supabase`, pending operator confirmation,” but must not proceed to implementation.
- Migration implementation remains blocked until the operator accepts the target path and resolves split-source evidence.
- Production SQL evidence remains valid and stronger than local source assumptions for production-state claims.

## 11. Remaining Unknowns

- Whether production migration history was assembled manually from multiple local trees.
- Why host-transfer production RPCs align more with `joinfolk-web`/mobile than `hostos`.
- Why proof-specific helpers align more with mobile than `hostos`.
- Where `public_verify_checkin` originated.
- Whether deployed Edge Functions use any of these trees; this check did not inspect Edge Functions.
- Which source tree should be treated as the deployment/migration source for future accepted changes.

## 12. Non-Goals

- This report does not claim final canonical source.
- This report does not include SQL.
- This report does not include cleanup instructions.
- This report does not include migration instructions.
- This report does not authorize modifications.
- This report does not authorize proof check-in hardening implementation.
- This report does not inspect production directly.
- This report does not inspect Edge Function deployment.

## 13. Open Questions

- Was production migration history assembled from more than one local migration tree?
- Is `C:\dev\hostos\supabase\migrations` the accepted future target for new backend migrations?
- Should host identity transfer / ops migrations from `joinfolk-web` be reconciled into the primary backend source?
- Should proof-specific mobile migration history be reconciled before proof check-in hardening work proceeds?
- Where did `public_verify_checkin` originate?
- Are Edge Functions deployed from any of these local trees, from another tree, or not deployed?
- What operator decision is required to unblock future migration implementation?

## 14. No-Modification Confirmation

For this production migration provenance report:

- No application code files were modified. This report was created from supplied read-only migration provenance output and handbook sources.
- No production connection was made.
- Supabase CLI was not run.
- No migrations were run or generated.
- No builds were run.
- No tests were run.
- No cleanup plan was created.
- No patch plan was created.
- No Supabase tree modification was authorized.
- No proof check-in hardening implementation was authorized.
- No files were staged or committed.

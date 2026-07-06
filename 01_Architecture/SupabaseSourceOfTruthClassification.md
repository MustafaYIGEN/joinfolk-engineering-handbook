# Supabase Source of Truth Classification

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: User-stated + Local read-only Supabase source classification audit
- canonical: false

## 2. Purpose

This document is a draft Supabase source-of-truth classification for local JoinFolk repository paths.

It is not a canonical decision, not a cleanup plan, not a patch plan, and not a migration plan. It separates confirmed local facts from interpretation and keeps local source classification separate from production SQL evidence. Production SQL evidence remains valid separately from local source-path ambiguity.

## 3. Classification Scope

Competing Supabase paths reviewed in the supplied classification evidence:

1. `C:\dev\hostos\supabase`
2. `C:\dev\joinfolk-web\supabase`
3. `C:\dev\hostos\apps\mobile\supabase`

This document classifies the local source-path evidence only. It does not establish production deployment source of truth.

## 4. Supabase Path Inventory

| Path | Parent repo | Branch | Remote | Status | Migrations | Functions | Config / seed | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `C:\dev\hostos\supabase` | `joinfolk-platform.git` | `refactor/joinfolk-stabilization-p0` | `MustafaYIGEN/joinfolk-platform.git` | Clean | 186 | `push-dispatch`, `send-test-push`, `snapshot`, `transactional-email` | No `config.toml`, no `seed.sql` | Largest and broadest backend-looking tree. |
| `C:\dev\joinfolk-web\supabase` | `joinfolk-web.git` | `refactor/joinfolk-stabilization-p0` | `MustafaYIGEN/joinfolk-web.git` | Modified dashboard files; untracked migration | 54 | None | No `config.toml`, no `seed.sql`, `.temp` exists | Partial-looking; includes untracked `20260628_host_identity_transfer_v1_1_persona.sql`. |
| `C:\dev\hostos\apps\mobile\supabase` | Nested `joinfolk-web.git` | `release/ios-v17-media-performance` | `MustafaYIGEN/joinfolk-web.git` | Modified mobile UI files | 103 | `notify-transfer-invite`, `notify-transfer-review` | No `config.toml`, no `seed.sql`, `.temp` exists | Component-specific/mobile-looking tree. |

Earliest and latest migration observations:

- `C:\dev\hostos\supabase`: earliest `20260116212500_command_center_tables.sql`; latest `20260626_commerce_standing_tickets_v1.sql`.
- `C:\dev\joinfolk-web\supabase`: earliest `20260304_add_ticket_sales_module_key.sql`; latest `temp_rpc.sql`.
- `C:\dev\hostos\apps\mobile\supabase`: earliest `_audit_profile_bootstrap.sql`; latest `20260611_visual_topology_meta_v1.sql`.

## 5. Migration History Comparison

Filename overlap:

- `hostos` vs `joinfolk-web`: 0 filename overlaps.
- `hostos` vs `mobile`: 1 overlap, `20260512_app_diagnostics.sql`.
- `joinfolk-web` vs `mobile`: 21 overlaps, mostly early March ticket/profile/gift/notification migrations.

Hostos latest unique examples:

- `20260606_commerce_eligibility_guard.sql`
- `20260607_ticket_product_upsert_null_clear_semantics.sql`
- `20260618_p0_cross_entitlement_guard_v1.sql`
- `20260619123535_fix_venue_media_storage_upload_policy.sql`
- `20260626_commerce_standing_tickets_v1.sql`

Joinfolk-web latest unique examples:

- `20260627_ops_media_drafts_v1.sql`
- `20260628_host_identity_transfer_v1.sql`
- `20260628_host_identity_transfer_v1_1_persona.sql`
- `temp_rpc.sql`

Mobile latest unique examples:

- `20260503_host_transfer_phase*.sql`
- `20260512_build19_app_diagnostics_anon_policy.sql`
- `20260611_frame_slot_guard_fix_v1.sql`
- `20260611_visual_topology_meta_v1.sql`

Interpretation:

- The histories look forked/partial rather than copied wholesale.
- `joinfolk-web` and mobile share an early base.
- `hostos` is largely independent and broader.
- Migration history alone does not prove canonical deployment source.

## 6. Function Source Comparison

Confirmed local function directories:

- `C:\dev\hostos\supabase\functions`: `push-dispatch`, `send-test-push`, `snapshot`, `transactional-email`.
- `C:\dev\joinfolk-web\supabase\functions`: missing.
- `C:\dev\hostos\apps\mobile\supabase\functions`: `notify-transfer-invite`, `notify-transfer-review`.

No `deno.json`, import map, README, or obvious config file was observed in the supplied function-directory evidence.

Interpretation:

- `hostos\supabase` has the broadest backend function set and includes `push-dispatch`.
- Mobile has component-looking notification functions.
- Function evidence supports `hostos\supabase` as the primary plausible backend source, but does not prove canonical deployment source.

## 7. Local Reference Signals

Confirmed local reference signals:

- No inspected `package.json` scripts referenced Supabase migrations, functions, deploy, or database operations.
- Hostos docs reference `supabase/migrations` paths and `push-dispatch`.
- `01_Architecture/RepositorySourceMap.md` already states `C:\dev\hostos\supabase` is primary plausible but not canonical.
- No safe docs/scripts conclusively name `joinfolk-web\supabase` or mobile `supabase` as canonical deployment source.

Interpretation:

- Local references point most strongly toward `hostos\supabase`.
- Deployment source remains Unknown / Needs verification.

## 8. Classification Results

### C:\dev\hostos\supabase

- Classification: Primary plausible active backend source; canonical status still Needs verification.
- Confidence: Medium-High.
- Confirmed local evidence: largest migration count, broad backend domains, recent commerce/ticket/storage migrations, Edge Functions including `push-dispatch`, parent repo `joinfolk-platform`.
- Risks: no `config.toml`, near-zero overlap with other trees, not proven deployment source.

### C:\dev\joinfolk-web\supabase

- Classification: Partial / possibly dashboard-web-adjacent migration history; possible experimental or in-progress tree.
- Confidence: Medium.
- Confirmed local evidence: 54 migrations, no functions, `.temp`, `temp_rpc.sql`, untracked latest migration, overlaps mobile early history but not `hostos`.
- Risks: could contain current draft work such as host identity transfer; easy to mistake for backend canonical path.

### C:\dev\hostos\apps\mobile\supabase

- Classification: Active component-specific backend source or stale mobile-coupled fork; Needs verification.
- Confidence: Medium.
- Confirmed local evidence: nested mobile repo, mobile release branch, mobile-specific notify-transfer functions, 103 migrations, overlaps `joinfolk-web` early history.
- Risks: nested repo means parent `hostos` status does not cover it; migrations may be stale or mobile-specific.

## 9. Recommended Working Rule

- Treat `C:\dev\hostos\supabase` as the primary plausible backend source for documentation and local-code audits, with explicit caveat: not confirmed canonical.
- Do not modify any Supabase tree until source-of-truth is confirmed.
- Block future migration creation or patch implementation that assumes a canonical path until the operator confirms the deployment path or deployment config is verified read-only.
- Future reports must name the exact source path used and separate local-source findings from production SQL findings.
- Production SQL evidence outranks local source assumptions for production-state claims.

## 10. Impact on Existing Reports and Patch Plans

| Document | Impact | Reason | Recheck |
| --- | --- | --- | --- |
| `SupabaseBackendGapReport` | Medium | Relies on local `hostos\supabase` findings, especially `push-dispatch`; path is plausible but not proven canonical. | Recheck any local-code finding against confirmed source path. |
| `SupabaseProductionParityVerificationReport` | Low for SQL; Medium for local-code carryover | Production SQL evidence is separate; `push-dispatch` deployment posture remains local-source dependent. | Recheck local-code carryover after backend source path is confirmed. |
| `SupabaseFocusedBackendFollowUpReport` | Low to Medium | Focused SQL findings are mostly safe; dormant/local-code `push-dispatch` language depends on path. | Recheck only local-source assumptions if used later. |
| `SupabaseProofCheckinRpcHardeningPlan` | Medium | Patch plan should not name target migration path until source-of-truth is confirmed. | Confirm target Supabase tree before implementation. |

The proof check-in patch plan cannot be implementation-ready until the target Supabase tree is confirmed.

## 11. Migration / Patch Blocking Rule

No Supabase tree should be modified until source-of-truth is confirmed.

Future migration creation, patch implementation, or path-specific remediation must remain blocked until:

- The deployment/migration source path is named.
- Competing Supabase trees are classified as active, stale, partial, or component-specific.
- The target Supabase source tree is confirmed for the specific change type.
- Local-source evidence is separated from production SQL evidence in any approving document.

This is a blocking documentation rule, not a cleanup instruction and not a migration instruction.

## 12. Remaining Unknowns

- Which Supabase path is used by the real deployment/migration process.
- Whether `joinfolk-web\supabase` is draft, stale, dashboard-specific, or active.
- Whether mobile `supabase` functions are deployed or only historical/mobile-coupled.
- Why no inspected Supabase tree has `config.toml`.
- Whether `temp_rpc.sql` is scratch work or accepted source material.
- Whether `hostos\supabase` and production are intentionally decoupled.

## 13. Required Manual Confirmation

Before treating any Supabase source tree as canonical, manually confirm:

- Which path feeds production migrations.
- Which path feeds Edge Function deployment.
- Whether `hostos\supabase`, `joinfolk-web\supabase`, and mobile `supabase` have distinct ownership.
- Whether `joinfolk-web\supabase` contains active in-progress work, stale history, or dashboard/web-specific database work.
- Whether mobile `supabase` is active component-owned source or historical fork.
- Whether deployment configuration exists outside the inspected paths.

## 14. Non-Goals

- This document does not make a canonical source-of-truth decision.
- This document does not provide cleanup instructions.
- This document does not provide migration instructions.
- This document does not include SQL.
- This document does not include production commands.
- This document does not authorize modifying any Supabase tree.
- This document does not supersede production SQL evidence.

## 15. Open Questions

- Which Supabase directory is canonical for production migrations?
- Which Supabase directory is canonical for Edge Functions?
- Is `C:\dev\joinfolk-web\supabase` active, stale, partial, dashboard-specific, or experimental?
- Is `C:\dev\hostos\apps\mobile\supabase` active mobile-owned backend source or stale?
- Why do the inspected Supabase trees lack `config.toml`?
- Is `temp_rpc.sql` scratch work, active draft work, or accepted source material?
- Are `hostos\supabase` and production intentionally decoupled?
- Which source tree should receive any future accepted proof check-in RPC hardening work?

## 16. No-Modification Confirmation

For this source-of-truth classification draft:

- No application code files were modified. This draft was created from supplied read-only classification evidence and handbook sources.
- No production connection was made.
- Supabase CLI was not run.
- No migrations were run or generated.
- No builds were run.
- No tests were run.
- No cleanup plan was created.
- No patch plan was created.
- No files were staged or committed.

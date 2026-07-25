# JoinFolk Platform Evidence Registry

## 1. Metadata

- Status: Active Registry
- Version: 1.0
- Owner: Mustafa / JoinFolk
- Last reviewed: 2026-07-25
- Purpose: Central registry tracking all verified production gates, platform commits, migration versions, evidence file paths, and gate results.

---

## 2. Production Gate Evidence Table

| Gate Identifier | Terminal Marker | Platform Commit | Handbook Commit | Migration Version | Migration SHA256 | Evidence Path | Final Evidence File | Result | Next Gate |
| :--- | :--- | :---: | :---: | :---: | :---: | :--- | :--- | :---: | :--- |
| **`P0_ANON_RPC_CONTAINMENT_PRODUCTION_CLOSED`** | `JOINFOLK_P0_ANON_RPC_CONTAINMENT_PRODUCTION_CLOSED` | `d8828f5a` | `095081e` | `20260725140000` | `B004199B0383B92AC5FED89A5E7C058162C827B7B00A3D6B071D74EDFE4E4524` | `C:\dev\joinfolk-evidence\db-surface-audit\p0-target-only-apply-v1\` | `target_only_apply_final_evidence.json` | **CLOSED / PASSED** | `P1_AUTHENTICATED_ONLY_RPC_SURFACE_REVIEW` |

---

## 3. Detailed Evidence Specifications

### Gate: `P0_ANON_RPC_CONTAINMENT_PRODUCTION_CLOSED`
- **Platform Commit**: `d8828f5a` (`refactor/joinfolk-stabilization-p0`)
- **Handbook Closeout Commit**: `095081e`
- **Target Migration File**: `20260725140000_p0_anon_rpc_surface_containment.sql`
- **Containment Mode**: Mode B Strict Containment
- **Target Counts**:
  - `p0_signatures_count`: 21
  - `p0_anon_execute_count`: 0
  - `p0_service_role_execute_count`: 21
  - `authenticated_direct_execute_count`: 4
  - `public_allowlist_anon_execute_count`: 12
  - `phase_a_hashes_unchanged`: true
  - `tracking_row_inserted`: true
  - `tracking_row_verified`: true
- **Evidence Files**:
  - `C:\dev\joinfolk-evidence\db-surface-audit\p0-target-only-apply-v1\apply_20260725140000_target_only.sql`
  - `C:\dev\joinfolk-evidence\db-surface-audit\p0-target-only-apply-v1\post_apply_verify_20260725140000.sql`
  - `C:\dev\joinfolk-evidence\db-surface-audit\p0-target-only-apply-v1\target_only_apply.log`
  - `C:\dev\joinfolk-evidence\db-surface-audit\p0-target-only-apply-v1\target_only_post_apply_verify.log`
  - `C:\dev\joinfolk-evidence\db-surface-audit\p0-target-only-apply-v1\target_only_apply_final_evidence.json`

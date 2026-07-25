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
| **`P1_AUTHENTICATED_ONLY_RPC_SURFACE_REVIEW_PRODUCTION_CLOSED`** | `JOINFOLK_P1_AUTHENTICATED_ONLY_RPC_SURFACE_REVIEW_PRODUCTION_CLOSED` | `c6a80d10` | `68e2062` | `20260725193000` | `AB11549675A6CD90CDD0A8F809E2A068C217210B2395CDF89D622B1D63882751` | `C:\dev\joinfolk-evidence\db-surface-audit\p1-authenticated-only-rpc-surface-v1\phase7-target-only-apply-v1\` | `p1_phase7_target_only_apply_final_evidence.json` | **CLOSED / PASSED** | `TBD` |

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

### Gate: `P1_AUTHENTICATED_ONLY_RPC_SURFACE_REVIEW_PRODUCTION_CLOSED`
- **Platform Commit**: `c6a80d10` (`refactor/joinfolk-stabilization-p0`)
- **Target Migration File**: `20260725193000_p1_authenticated_rpc_surface_containment.sql`
- **Migration SHA256**: `AB11549675A6CD90CDD0A8F809E2A068C217210B2395CDF89D622B1D63882751`
- **Target Counts**:
  - `target_signature_count`: 13
  - `authenticated_execute_count`: 0
  - `anon_execute_count`: 0
  - `public_execute_count`: 0
  - `service_role_execute_count`: 13
  - `tracking_row_inserted`: true
  - `tracking_row_verified`: true
- **Evidence Files & Hashes**:
  - `C:\dev\joinfolk-evidence\db-surface-audit\p1-authenticated-only-rpc-surface-v1\phase7-target-only-apply-v1\apply_20260725193000_p1_target_only.sql`
  - `C:\dev\joinfolk-evidence\db-surface-audit\p1-authenticated-only-rpc-surface-v1\phase7-target-only-apply-v1\post_apply_verify_20260725193000_p1.sql`
  - `C:\dev\joinfolk-evidence\db-surface-audit\p1-authenticated-only-rpc-surface-v1\phase7-target-only-apply-v1\p1_phase7_target_only_apply.log` (`A5396A0917B498AAB5E32553167FD27E9D2F672165BD1DEA4B393856D3A6F446`)
  - `C:\dev\joinfolk-evidence\db-surface-audit\p1-authenticated-only-rpc-surface-v1\phase7-target-only-apply-v1\p1_phase7_target_only_post_apply_verify.log` (`B60379F672D8EA6890EF927DD32238EB28E9BEC9D6DA3FE31AFE01560F7F3162`)
  - `C:\dev\joinfolk-evidence\db-surface-audit\p1-authenticated-only-rpc-surface-v1\phase7-target-only-apply-v1\p1_phase7_target_only_apply_final_evidence.json` (`2C7D011E4CA141043E084FE826BDFA97B6BCD2FF49F6A58E3B4BE69978527F8D`)


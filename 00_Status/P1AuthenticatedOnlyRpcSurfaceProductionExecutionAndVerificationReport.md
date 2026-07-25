# P1 Authenticated-Only RPC Surface Production Execution and Verification Report

## 1. Metadata

- **Gate Identifier**: `P1_AUTHENTICATED_ONLY_RPC_SURFACE_REVIEW`
- **Final Classification**: `PRODUCTION_CLOSED`
- **Platform Commit**: `c6a80d10` (`fix(db): contain P1 authenticated RPC surface`)
- **Migration Version**: `20260725193000`
- **Migration File**: `C:\dev\hostos\supabase\migrations\20260725193000_p1_authenticated_rpc_surface_containment.sql`
- **Migration SHA256**: `AB11549675A6CD90CDD0A8F809E2A068C217210B2395CDF89D622B1D63882751`
- **Terminal Marker**: `JOINFOLK_P1_AUTHENTICATED_ONLY_RPC_SURFACE_REVIEW_PRODUCTION_CLOSED`
- **Evidence Root**: `C:\dev\joinfolk-evidence\db-surface-audit\p1-authenticated-only-rpc-surface-v1\`

---

## 2. Executive Summary

This report documents the completion and formal production closure of the **P1 Authenticated-Only RPC Surface Review** (`P1_AUTHENTICATED_ONLY_RPC_SURFACE_REVIEW_PRODUCTION_CLOSED`).

The migration `20260725193000_p1_authenticated_rpc_surface_containment.sql` was applied to production PostgreSQL via the target-only apply wrapper (`apply_20260725193000_p1_target_only.sql`), revoking `PUBLIC`, `anon`, and `authenticated` EXECUTE privileges from **13 target RPC functions** while retaining `service_role` EXECUTE.

Post-apply verification confirmed that **0 external authenticated calls** can execute any of the 13 contained functions, **53 excluded domain RPCs** retain authenticated EXECUTE access without application disruption, and the migration tracking row for version `20260725193000` was inserted and verified in `supabase_migrations.schema_migrations`.

---

## 3. Workstream Execution History

1. **Phase 1 (Inventory & Classification)**: Cataloged **66 total authenticated-only functions** (`authenticated_execute = true`, `anon_execute = false`).
2. **Phase 2 (Static Caller Trace)**: Scanned 1,527 source files across `C:\dev\hostos` and `C:\dev\joinfolk-web`. Identified 13 `service_role` containment candidates and 45 retain-authenticated candidates.
3. **Phase 3 (Body Extraction)**: Extracted production definitions for 6 target functions and isolated `admin_execute_host_identity_transfer_v1(uuid)` for explicit owner decision.
4. **Owner Technical Decision**: Owner rendered decision `public.admin_execute_host_identity_transfer_v1(uuid) = CONTAIN_TO_SERVICE_ROLE`.
5. **Phase 4 (Patch Plan Scoping & Migration Draft)**: Scoped 13 containment targets and 53 retain-authenticated exclusions. Drafted migration `20260725193000_p1_authenticated_rpc_surface_containment.sql`.
6. **Phase 5 (Rollback-Only Dry-Run)**: Executed in-transaction dry-run (`BEGIN ... ROLLBACK`). Verified post-rollback ACL state restoration.
7. **Phase 6 (Commit & Push)**: Committed (`c6a80d10`) and pushed migration draft to `origin/refactor/joinfolk-stabilization-p0`.
8. **Phase 7 (Target-Only Apply & Post-Apply Verification)**: Executed `apply_20260725193000_p1_target_only.sql` and verified post-apply ACL states and tracking row presence.

---

## 4. Production Apply Evidence Hashes

- **Evidence Directory**: `C:\dev\joinfolk-evidence\db-surface-audit\p1-authenticated-only-rpc-surface-v1\phase7-target-only-apply-v1\`
- **Production Apply Log**: `p1_phase7_target_only_apply.log`
  - **SHA256**: `A5396A0917B498AAB5E32553167FD27E9D2F672165BD1DEA4B393856D3A6F446`
- **Post-Apply Verification Log**: `p1_phase7_target_only_post_apply_verify.log`
  - **SHA256**: `B60379F672D8EA6890EF927DD32238EB28E9BEC9D6DA3FE31AFE01560F7F3162`
- **Final Evidence Summary JSON**: `p1_phase7_target_only_apply_final_evidence.json`
  - **SHA256**: `2C7D011E4CA141043E084FE826BDFA97B6BCD2FF49F6A58E3B4BE69978527F8D`

---

## 5. Final Production ACL State

```json
{
  "gate": "P1_AUTHENTICATED_ONLY_RPC_SURFACE_REVIEW",
  "status": "PRODUCTION_CLOSED",
  "commit": "c6a80d10",
  "migration_version": "20260725193000",
  "target_signature_count": 13,
  "authenticated_execute_count": 0,
  "anon_execute_count": 0,
  "public_execute_count": 0,
  "service_role_execute_count": 13,
  "tracking_row_inserted": true,
  "tracking_row_verified": true,
  "target_only_apply_success": true,
  "post_apply_verify_success": true,
  "issue_codes": []
}
```

---

## 6. Exact 13 Contained RPC Targets

1. `public._can_mutate_venue_media_v1(uuid)`
2. `public._can_view_event_content(uuid)`
3. `public._check_poll_access(uuid)`
4. `public._check_vote_read_access(uuid)`
5. `public._is_share_group_member(uuid,uuid)`
6. `public._is_share_group_owner(uuid,uuid)`
7. `public._transfer_audit(uuid,text,uuid,jsonb)`
8. `public.admin_execute_host_identity_transfer_v1(uuid)`
9. `public.assert_checkin_open(uuid)`
10. `public.debug_auth_ctx_v2(uuid)`
11. `public.get_control_pending_claims(integer)`
12. `public.mute_target_v1(text,uuid)`
13. `public.unmute_target_v1(text,uuid)`

---

## 7. Explicit Safety Claims

- **Zero Function Body Changes**: No `CREATE OR REPLACE FUNCTION` DDL was executed (`pg_get_functiondef` hashes unchanged).
- **Zero Table DDL**: No `ALTER TABLE` or table mutations were performed.
- **Zero RLS Changes**: Row Level Security policies were unchanged.
- **Zero Persona/Lifecycle Changes**: User personas, host tier logic, and subscription mechanics were untouched.
- **No Supabase DB Push**: `supabase db push` was not executed. Single-target apply wrapper was used to prevent bulk migration side effects.
- **Tracking Row Insertion Verified**: Single row for version `20260725193000` was inserted and verified in `supabase_migrations.schema_migrations`.
- **Post-Apply Verification Passed**: All post-apply ACL assertions passed cleanly.

---

## 8. Final Gate Marker

JOINFOLK_P1_AUTHENTICATED_ONLY_RPC_SURFACE_REVIEW_PRODUCTION_CLOSED

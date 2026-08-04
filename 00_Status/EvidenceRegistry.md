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
| **`A01_UNEXPECTED_ANON_PUBLIC_RPC_CONTAINMENT_PRODUCTION_CLOSED`** | `JOINFOLK_A01_UNEXPECTED_ANON_PUBLIC_RPC_CONTAINMENT_REPO_COMMITTED_AND_PUSHED` | `e7b099e4` | `TBD` | `20260725213000` | `7E252D9A3DB7BF7C846768618F4A08A60A088FD39CC1605189467654A63998BF` | `C:\dev\joinfolk-evidence\db-surface-audit\unexpected-anon-public-execute-containment-v1\phase6r-batch-a01-target-only-production-apply\` | `phase6r_a01_target_only_production_apply_final_evidence.json` | **CLOSED / PASSED** | `A02 planning or product smoke validation` |

| **`C07_RPC_ACL_POST_APPLY_PRODUCTION_VERIFY_CLOSED`** | `JOINFOLK_C07_RPC_ACL_POST_APPLY_PRODUCTION_VERIFY_V1_PASS` | `local-source-capture` | `local-handbook-candidate` | `20260803185000` | `LOCAL_SOURCE_CAPTURE` | `C:\dev\joinfolk-evidence\broad-launch-readiness-audit-v1\c07-rpc-acl-post-apply-production-verify-20260803_195129\` | `c07_final_result.json` | **CLOSED / PASSED** | `C07R Relic RPC Lockdown` |
| **`C07R_RELIC_RPC_LOCKDOWN_POST_APPLY_PRODUCTION_VERIFY_CLOSED`** | `JOINFOLK_C07R_RELIC_RPC_LOCKDOWN_POST_APPLY_PRODUCTION_VERIFY_V1_PASS` | `local-source-capture` | `local-handbook-candidate` | `20260803185000` | `LOCAL_SOURCE_CAPTURE` | `C:\dev\joinfolk-evidence\broad-launch-readiness-audit-v1\c07r-relic-rpc-lockdown-post-apply-production-verify-20260803_203047\` | `c07r_final_result.json` | **CLOSED / PASSED** | `C08 Closeout Binding` |

| **`C23_CONFIRM_ORDER_PAYMENT_SOURCE_CAPTURE_CLOSED`** | `JOINFOLK_C23_CONFIRM_ORDER_PAYMENT_SOURCE_CAPTURE_AND_HANDBOOK_LOCAL_WRITE_V1_READY_FOR_DIFF_REVIEW` | `local-source-capture` | `local-handbook-update` | `20260804100000` | `LOCAL_SOURCE_CAPTURE` | `C:\dev\joinfolk-evidence\broad-launch-readiness-audit-v1\c23-confirm-order-payment-source-capture-and-handbook-local-write-20260804_101338\` | `c23_final_result.json` | **ACCEPTED / CAPTURED** | `C24 Diff Review` |

### A01 Migration History Reconciliation

- Gate: `A01_SUPPORTED_MIGRATION_HISTORY_REPAIR_EXECUTION_V1`
- Version: `20260725213000`
- Method: `SUPPORTED_SUPABASE_MIGRATION_REPAIR`
- Scope: A01 only
- Before classification: `A01_MANUAL_EFFECT_CONFIRMED_NOT_HISTORY_TRACKED`
- After classification: `A01_HISTORY_TRACKED_AND_EFFECT_CONFIRMED`
- Platform commit: `e7b099e4`
- Migration: `supabase/migrations/20260725213000_unexpected_anon_public_execute_batch_a01.sql`
- Migration SHA256: `7E252D9A3DB7BF7C846768618F4A08A60A088FD39CC1605189467654A63998BF`
- Execution summary SHA256: `BB4499A7205B6AB2CDA271D12CB25E73E799796C78338299397D54C096853A77`
- Execution report SHA256: `808801AE9B378BBAA7A12523DB9B9F6019B2B6CAB21D8194016C2399BF4A7D05`
- Repair execution log SHA256: `61F17499DFD67E2D691CA589317911C356881E81FFE6FF7A0CFFF31BBE04E0B2`
- Post-repair history log SHA256: `28BB5A626B334C1840E01D2C853C441BE26799CDCB3D951FB0A0C6461C5D8694`
- Post-repair effect log SHA256: `99AD459D6B43E33C5B8B2D49E985E70667218B34F97CBD9C401C98E02429A8F1`
- Status: **CLOSED / PASSED**
- Notes: History count increased from 9 to 10; migration SQL was not rerun and product smoke remains separate.

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

### Gate: `A02_HISTORY_TRACKED_AND_EFFECT_CONFIRMED`
- Platform commit: `3df55c0f`
- Migration: `20260725223000_unexpected_anon_public_execute_batch_a02.sql`
- Migration SHA256: `FBAD756FCF79A48DA77849C2D9BAA55239CC7BCF0E36EB2B634063D47A116887`
- Apply log SHA256: `47984DB3AE7F57F0060B95D7F86C4C2163BD12CD104C1A58A5EF681040EA9F25`
- Post-commit verify log SHA256: `6142A2DED2A9B19D02872CC177DF1BE2A671B4DB4E40CF01DDFD66406541B152`
- Final evidence SHA256: `E9C3F0FDF0AF27A6E41A7BBBCC6E947903D24759054AB2F28250C6E04879A236`
- History row: `20260725223000|unexpected_anon_public_execute_batch_a02`
- Repair: supported Supabase migration repair, A02-only, executed once
- Status: production applied, repo committed, migration history tracked, effect confirmed
- Notes: no db push, migration SQL rerun, or manual history insert; product smoke remains separate.

### Gate: `A03_FULLY_CLOSED`
- Platform commit: `297ec584`
- Migration: `20260725233000_unexpected_anon_public_execute_batch_a03.sql`
- Migration SHA256: `4139DC356E35580EE5E891EA0F653F24C8DD784763711109E20582F2C725167F`
- Production effect: target/resolved `40/40`; anon/public/authenticated `0/0/0`; service_role `40`; allowlist `2/2`
- Recovery final evidence SHA256: `D1488B6E4D5378279CE34F3930E6F55FB312ED14234AB9FCA187E54A45380296`
- Apply log SHA256: `DC04B6596A5620435F2B1B391878A38A459A8F9C8FFC8B0410DE52E458D91082`
- Recovery post-commit verify log SHA256: `C3A5E527078A678307952FAB95AD34143D28F7175C9754E26470A20E4BF8B540`
- History: `20260725233000|unexpected_anon_public_execute_batch_a03`; local equals remote
- Closeout document: `00_Status/A03UnexpectedAnonPublicRpcContainmentProductionCloseout.md`
- Status: production applied, repo committed, migration history tracked, effect confirmed; recovery used after post-COMMIT wrapper evidence failure
- Next gate: `B01_UNEXPECTED_ANON_PUBLIC_RPC_CONTAINMENT_PHASE4_MIGRATION_DRAFT_V1`

### Gate: `B01_FULLY_CLOSED`
- Platform commit: `4dadb310`
- Migration: `20260726003000_unexpected_anon_public_execute_batch_b01.sql`
- Migration SHA256: `6DC3F3520FDF6C6354433FDBE220F09D67A057D569710A154262D5AEA428D91E`
- Production effect: target/resolved `40/40`; anon/public/authenticated after `0/0/0`; service_role `40`; allowlist `2/2`
- Production classification: `B01_PHASE6R_PRODUCTION_EFFECT_CLOSED_VIA_POST_COMMIT_RECOVERY`
- Recovery apply log SHA256: `7B7A99556BD7F6C17F3088BCD9F5B783BDE9C2A0964807C1C4D92852F3BE9A67`
- Recovered verifier log SHA256: `B0258C2137FFAB295CC954703FB77C0E342F5F50866FC521754A11F01C19363B`
- Final evidence SHA256: `C1A27FBF3CAAC6B3416429797FE5FC36057B624DCE13FB63D8FD15128C0330A9`
- Migration history: `20260726003000` Local/Remote applied
- Closeout document: `00_Status/B01UnexpectedAnonPublicRpcContainmentProductionCloseout.md`
- Status: production effect confirmed via post-commit recovery verifier; wrapper caveat documented, not an open production blocker

### Gate: `B02_FULLY_CLOSED`
- Platform commit: `af6511e7`
- Migration: `20260726013000_unexpected_anon_public_execute_batch_b02.sql`
- Migration SHA256: `6992D8A46EC7CAE3F13D72B715F72CEF46BE08706CA2F7AD608D7EFD5826D040`
- Production classification: `B02_PHASE6R_PRODUCTION_EFFECT_CLOSED_VIA_READONLY_POST_COMMIT_RECOVERY`
- Production effect: target/resolved `40/40`; anon/public/authenticated after `0/0/0`; service_role `40`; allowlist `2/2`
- Phase6R recovered final evidence SHA256: `CFE055B0A0B10F70352C72D20191CD5A1275A0D1EBEA1177777C25F0DC504673`
- Recovered post-commit log SHA256: `76E97308EBBA47DF6C6B175EC551CA2716FFA5A6AFB559F697A48481DAD415B4`
- Migration history: `20260726013000` Local/Remote applied
- Closeout document: `00_Status/B02UnexpectedAnonPublicRpcContainmentProductionCloseout.md`
- Status: production effect confirmed via read-only post-commit recovery evidence; wrapper path caveat documented, not a production blocker


<!-- C39_ISSUE_TICKETS_FROM_ORDER_BODY_ALIGNMENT_EVIDENCE_START -->
## ISSUE_TICKETS_FROM_ORDER_BODY_ALIGNMENT evidence chain

Generated: 2026-08-04T10:11:13.9228956Z

Target RPC: public._issue_tickets_from_order_v1(uuid)

Evidence classification chain:
- C35: JOINFOLK_C35_ISSUE_TICKETS_FROM_ORDER_PRODUCTION_APPLY_V1_PASS_READY_FOR_POST_APPLY_READONLY_VERIFY
- C36: JOINFOLK_C36_ISSUE_TICKETS_FROM_ORDER_POST_APPLY_READONLY_VERIFY_V1_PASS_READY_FOR_SOURCE_ALIGNMENT_AND_CLOSEOUT
- C37R: JOINFOLK_C37R_ISSUE_TICKETS_FROM_ORDER_LOCAL_SOURCE_CAPTURE_REPAIR_V1_READY_FOR_DIFF_REVIEW
- C38: JOINFOLK_C38_ISSUE_TICKETS_FROM_ORDER_DIFF_AND_UNTRACKED_REVIEW_V1_READY_FOR_SCOPED_GIT_ADD_AND_HANDBOOK_EDIT_AUTHORIZATION_WITH_OTHER_UNTRACKED_EXCLUDED
- C39F: JOINFOLK_C39F_ISSUE_TICKETS_FROM_ORDER_SCOPED_ADD_FALSE_NEGATIVE_FORENSIC_V1_BLOCKED_MISMATCH
- C39R: handbook block repair

Production result:
- Expected/current post md5: c1e54217ce48a1c3ce21fee2d96327be
- C36 post_apply_current_state_ok: true
- Strong body guard: true
- ACL: anon=false, authenticated=false, public=false, service_role=true

Source capture:
- File: supabase/migrations/20260804115000_issue_tickets_from_order_body_guard_source_capture_v1.sql
- SHA256: CFA66A795628F4C40CAC924FCA1A50633317FD49594C013B1FF489B66D4C7C37
- Other HostOS untracked files are explicitly excluded from scoped add.

Artifact paths:
- C35 final: C:\dev\joinfolk-evidence\broad-launch-readiness-audit-v1\c35-issue-tickets-from-order-production-apply-20260804_113719\c35_final_result.json
- C36 final: C:\dev\joinfolk-evidence\broad-launch-readiness-audit-v1\c36-issue-tickets-from-order-post-apply-readonly-verify-20260804_114321\c36_final_result.json
- C37R final: C:\dev\joinfolk-evidence\broad-launch-readiness-audit-v1\c37r-issue-tickets-from-order-local-source-capture-repair-20260804_115251\c37r_final_result.json
- C38 final: C:\dev\joinfolk-evidence\broad-launch-readiness-audit-v1\c38-issue-tickets-from-order-diff-and-untracked-review-20260804_115649\c38_final_result.json
- C39 final: C:\dev\joinfolk-evidence\broad-launch-readiness-audit-v1\c39-issue-tickets-from-order-scoped-git-add-handbook-edit-20260804_120110\c39_final_result.json
- C39F final: C:\dev\joinfolk-evidence\broad-launch-readiness-audit-v1\c39f-issue-tickets-from-order-scoped-add-false-negative-forensic-20260804_120819\c39f_final_result.json

Restrictions preserved:
- No production SQL in C39
- No production SQL in C39R
- No DB push
- No migration apply
- No git add in C39R
- No git commit
- No git push
- No global launch-safe claim
<!-- C39_ISSUE_TICKETS_FROM_ORDER_BODY_ALIGNMENT_EVIDENCE_END -->

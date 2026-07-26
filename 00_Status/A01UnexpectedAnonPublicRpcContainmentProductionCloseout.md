# A01 Unexpected Anon/Public RPC Containment - Production Closed

- Gate: `A01_UNEXPECTED_ANON_PUBLIC_RPC_CONTAINMENT_PRODUCTION_CLOSED`
- Platform commit: `e7b099e4` (`fix(db): contain unexpected anon public RPC batch A01`)
- Migration: `supabase/migrations/20260725213000_unexpected_anon_public_execute_batch_a01.sql`
- Migration SHA256: `7E252D9A3DB7BF7C846768618F4A08A60A088FD39CC1605189467654A63998BF`
- Batch: `A01`; target count: `40`; production_apply: `true`; commit_observed: `true`; post_commit_verify_success: `true`; issue_codes: `[]`

## Verified post-commit ACL state

- anon/PUBLIC/authenticated EXECUTE: `0/0/0`
- service_role EXECUTE: `40`
- Allowlist unchanged: `2` anon / `2` public

## Evidence

- Apply log SHA256: `05D7D5499FAD9433F38C4A9C1924B8F1E603B54D5B01DD3470AA5B83CD03E455`
- Post-commit verify log SHA256: `9DFA24AF21803284F5C856BA50F7CA8EEDA6E02EC48A306A5C8F8F14D1DB99DD`
- Final evidence JSON SHA256: `1670A88E925044CD440FE0F66089AF0FDD4299C1D88E00BDC76BC502E22229C9`

The post-commit verifier initially failed after COMMIT because of a read-only aggregation defect. Recovery validated the existing committed apply log and ran only the repaired verifier; production apply was not rerun.

Current status: DB ACL containment is production closed; the platform migration is committed and pushed. Product smoke validation remains separate and is not claimed complete.

Next gate: A02 unexpected anon/public RPC containment planning/dry-run, or product smoke validation if launch-risk validation is prioritized.

## A01 Migration History Reconciliation - Closed

- Previous state: `A01_MANUAL_EFFECT_CONFIRMED_NOT_HISTORY_TRACKED`
- Final state: `A01_HISTORY_TRACKED_AND_EFFECT_CONFIRMED`
- Repair method: `SUPPORTED_SUPABASE_MIGRATION_REPAIR`
- Command executed once: `npx --no-install supabase migration repair --linked --status applied 20260725213000`
- Scope: A01 only; migration SQL was not executed, `supabase db push` was not used, and no manual history insert was used.
- Post-repair history row: `20260725213000|unexpected_anon_public_execute_batch_a01`
- Post-repair history count: `10`
- Post-repair ACL effect: anon/PUBLIC/authenticated `0/0/0`; service_role `40`; allowlist `2` anon / `2` public; `issue_codes=[]`
- Execution summary SHA256: `BB4499A7205B6AB2CDA271D12CB25E73E799796C78338299397D54C096853A77`
- Execution report SHA256: `808801AE9B378BBAA7A12523DB9B9F6019B2B6CAB21D8194016C2399BF4A7D05`
- Repair execution log SHA256: `61F17499DFD67E2D691CA589317911C356881E81FFE6FF7A0CFFF31BBE04E0B2`
- Post-repair history log SHA256: `28BB5A626B334C1840E01D2C853C441BE26799CDCB3D951FB0A0C6461C5D8694`
- Post-repair effect log SHA256: `99AD459D6B43E33C5B8B2D49E985E70667218B34F97CBD9C401C98E02429A8F1`

Current A01 status: production ACL containment closed, repo-tracked, and Supabase migration-history tracked.

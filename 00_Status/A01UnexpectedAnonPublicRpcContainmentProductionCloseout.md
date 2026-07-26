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

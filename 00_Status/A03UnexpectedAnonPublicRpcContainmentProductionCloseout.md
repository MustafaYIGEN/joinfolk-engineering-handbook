# A03 Unexpected Anon/Public RPC Containment - Production Closeout

- Gate: `A03` unexpected anon/public RPC containment
- Final classification: `A03_FULLY_CLOSED`
- Production apply: effect confirmed via recovery post-commit verifier
- Platform commit: `297ec584`
- Migration: `20260725233000_unexpected_anon_public_execute_batch_a03.sql`
- Migration SHA256: `4139DC356E35580EE5E891EA0F653F24C8DD784763711109E20582F2C725167F`

## Production effect

- target_count/resolved_count: `40/40`
- anon/public/authenticated execute: `0/0/0`
- service_role execute: `40`
- allowlist anon/public execute: `2/2`
- issue_codes: `[]`

## Evidence

- Recovery final evidence SHA256: `D1488B6E4D5378279CE34F3930E6F55FB312ED14234AB9FCA187E54A45380296`
- Production apply log SHA256: `DC04B6596A5620435F2B1B391878A38A459A8F9C8FFC8B0410DE52E458D91082`
- Recovery post-commit verifier SQL SHA256: `651B4748C562003D9CFAA944379240ED3A7EC18ACD956F6ACA23B17C31D1560C`
- Recovery post-commit verifier log SHA256: `C3A5E527078A678307952FAB95AD34143D28F7175C9754E26470A20E4BF8B540`
- Apply SQL SHA256: `515BFED89A7196212DCEC3AA0A9296DF6FFAAB7BF408347FE708C5424D38D6FC`
- Wrapper SHA256: `6BDDF528BEBEA38C5CCF521EF188BD26346396FBAC724B405BA0B335095576BB`
- Original post-commit verifier SQL SHA256: `455AF555FA4B4F5BE4F5BCB4A47A1B8588801CCDF2D1B730715BD036A8BAB5B0`

## Migration-history reconciliation

- Result: `Repaired migration history: [20260725233000] => applied`
- Verification: `Local 20260725233000 equals Remote 20260725233000`
- Migration history is repaired and verified.
- Recovery was used because the wrapper failed after COMMIT before normal final evidence was written.
- Apply rerun is not authorized and is not needed.

## Scope boundaries

This closeout makes no business-flow E2E, UI smoke, A04/B/C batch, broad DB-security, or product-readiness claim.

Next recommended gate: `B01_UNEXPECTED_ANON_PUBLIC_RPC_CONTAINMENT_PHASE4_MIGRATION_DRAFT_V1`.

# B01 Unexpected Anon/Public RPC Containment - Production Closeout

- Gate: `B01` unexpected anon/public RPC containment
- Final classification: `B01_FULLY_CLOSED`
- Production effect: `B01_PHASE6R_PRODUCTION_EFFECT_CLOSED_VIA_POST_COMMIT_RECOVERY`
- Platform commit: `4dadb310` - fix(db): contain unexpected anon public RPC batch B01
- Migration: `20260726003000_unexpected_anon_public_execute_batch_b01.sql`
- Migration SHA256: `6DC3F3520FDF6C6354433FDBE220F09D67A057D569710A154262D5AEA428D91E`

## Production effect

- target_count/resolved_count: `40/40`
- anon/public/authenticated execute after: `0/0/0`
- service_role execute after: `40`
- allowlist anon/public execute: `2/2`

## Evidence

- Recovery apply log SHA256: `7B7A99556BD7F6C17F3088BCD9F5B783BDE9C2A0964807C1C4D92852F3BE9A67`
- Read-only recovered post-commit verifier SQL SHA256: `1CCE734B53DA2386B1499FDEFC1F71ABBC4DF3A85BBD498D0DB0F88077ACFF1D`
- Read-only recovered post-commit verifier log SHA256: `B0258C2137FFAB295CC954703FB77C0E342F5F50866FC521754A11F01C19363B`
- Final recovered evidence JSON SHA256: `C1A27FBF3CAAC6B3416429797FE5FC36057B624DCE13FB63D8FD15128C0330A9`
- Failed first apply log SHA256: `EC13439ACCB05BC23CF478C4C3010ED09FD27CF5DC00C10B95043651BC6E8EDB`
- Post-failure diagnostic log SHA256: `E88A3FEEE3BF290E1EE80757AEE0D80E84ECA810AC911BD0638F5E491087F424`

## Migration history

- `20260726003000` repaired and verified as Local/Remote applied.
- No additional production apply is authorized or needed.

## Recovery notes

- Initial apply failed before COMMIT because of verifier typo `authenticated_exec=0U`.
- Read-only post-failure diagnostics confirmed rollback/open state.
- Corrected recovery V2 apply committed successfully.
- The recovery wrapper had a non-fatal Assert-Hash definition defect; apply evidence remains valid because precheck, in-tx marker, COMMIT, and read-only post-commit recovery verification all passed.
- Final state is verified by the read-only post-commit recovery verifier.

## Scope boundaries

This closeout makes no business-flow E2E, UI smoke, broad DB-security, or product-readiness claim.

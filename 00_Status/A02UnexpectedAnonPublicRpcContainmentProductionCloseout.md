# A02 Unexpected Anon/Public RPC Containment — Production Closeout

- Gate: `A02` unexpected anon/public RPC containment
- Final classification: `A02_HISTORY_TRACKED_AND_EFFECT_CONFIRMED`
- Production apply: `PHASE6R_A02_TARGET_ONLY_PRODUCTION_APPLY_CLOSED`
- Platform commit: `3df55c0f` — fix(db): contain unexpected anon public RPC batch A02
- Migration: `20260725223000_unexpected_anon_public_execute_batch_a02.sql`
- Migration SHA256: `FBAD756FCF79A48DA77849C2D9BAA55239CC7BCF0E36EB2B634063D47A116887`

## Production effect

- target_count/resolved_count: `40/40`
- anon/public/authenticated execute: `0/0/0`
- service_role execute: `40`
- allowlist anon/public execute: `2/2`
- issue_codes: `[]`

Post-commit marker:

`JF_A02_PROD_APPLY_POST_COMMIT_VERIFY|status=SUCCESS|issue_codes=[]|target_count=40|resolved_count=40|anon_exec=0|public_exec=0|authenticated_exec=0|service_role_exec=40|allowlist_anon_exec=2|allowlist_public_exec=2`

## Evidence

- Apply log SHA256: `47984DB3AE7F57F0060B95D7F86C4C2163BD12CD104C1A58A5EF681040EA9F25`
- Post-commit verify log SHA256: `6142A2DED2A9B19D02872CC177DF1BE2A671B4DB4E40CF01DDFD66406541B152`
- Final evidence JSON SHA256: `E9C3F0FDF0AF27A6E41A7BBBCC6E947903D24759054AB2F28250C6E04879A236`

## Migration-history reconciliation

- Method: `SUPPORTED_SUPABASE_MIGRATION_REPAIR`
- Command executed exactly once: `npx --no-install supabase migration repair --linked --status applied 20260725223000`
- Migration SQL was not rerun; `supabase db push` was not used; no manual history insert was used.
- History row: `20260725223000|unexpected_anon_public_execute_batch_a02`
- History log SHA256: `4E99681A17B8D7C7AD709590A14FE20DC2C7DED3181167796C643AC3721A541B`
- Post-repair effect marker: `JF_A02_EFFECT_READONLY_VERIFY|status=SUCCESS|issue_codes=[]|target_count=40|resolved_count=40|anon_exec=0|public_exec=0|authenticated_exec=0|service_role_exec=40|allowlist_anon_exec=2|allowlist_public_exec=2`
- Effect log SHA256: `F485BD598A216F5A0B82F7F1B82418AB758F34C0A79C1DD0AC072890ED4556B4`
- Corrected verifier SQL SHA256: `00BEE733E317DD0E6F939E82228CBB5CEA12F6FF44D3433900A44A26F26FC2EB`

Phase8 artifacts: summary `83AEFCEA8D799B76A55BB780121EBDCC07D9547E847763C29F0F7BAC9EF009C3`; report `5F5A3E341598BA01D770BC99738348C2CF9F60F5F42C33F4EB8FE09851469E08`; transcript `65C48F9CD181AD5C249447049CFAE9E7481CA24AE4343F766032B9A5F7449E14`.

## Scope boundaries

This closeout makes no business-flow E2E or UI smoke claim, does not close A03/A04/B/C batches, does not claim broad DB-security completion, and does not claim product readiness.

Next recommended gate: `A03_UNEXPECTED_ANON_PUBLIC_RPC_CONTAINMENT_PHASE4_MIGRATION_DRAFT_V1`.

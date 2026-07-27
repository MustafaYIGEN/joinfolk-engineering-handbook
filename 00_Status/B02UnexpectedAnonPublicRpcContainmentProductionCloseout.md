# B02 Unexpected Anon/Public RPC Containment - Production Closeout

- Gate: `B02` unexpected anon/public RPC containment
- Final classification: `B02_FULLY_CLOSED`
- Production effect: `B02_PHASE6R_PRODUCTION_EFFECT_CLOSED_VIA_READONLY_POST_COMMIT_RECOVERY`
- Platform commit: `af6511e7` - fix(db): contain unexpected anon public RPC batch B02
- Migration: `20260726013000_unexpected_anon_public_execute_batch_b02.sql`
- Migration SHA256: `6992D8A46EC7CAE3F13D72B715F72CEF46BE08706CA2F7AD608D7EFD5826D040`

## Production effect

- target_count/resolved_count: `40/40`
- anon/public/authenticated execute after: `0/0/0`
- service_role execute after: `40`
- allowlist anon/public execute: `2/2`

## Phase5R baseline

- Classification: `B02_PHASE5R_ROLLBACK_ONLY_DRY_RUN_CLOSED_VIA_BASELINE_ADJUSTED_POST_ROLLBACK_RECOVERY`
- target/resolved: `40/40`
- anon/public/authenticated: `40/38/40`
- service_role: `40`
- baseline_non_public_count: `2`
- allowlist anon/public: `2/2`
- Baseline signatures: `public.get_event_share(uuid)`, `public.get_followed_host_ids()`

## Evidence

- Phase5R dry-run SQL SHA256: `75101A59631D982FB88D25CFA6DB3269EDA8BB35D3963809F337E0814D2AB201`
- Phase5R original post-rollback SQL SHA256: `382CD13990185F93EDFCCAA6FAF592474CA3D2FB0D55E52783D5DCD545724657`
- Phase5R wrapper SHA256: `D6B15B82182C3495AFC571EB269F3F76E0D1F145240B2EC1C87A430421C4D86B`
- Phase5R dry-run log SHA256: `A69AEBFA8CFC8DA1C212E8D2909B5F719AFEDEDBC6949054E5FB782BADE94AB7`
- Prior metric diagnostic log SHA256: `BE49788BAAB6C4A9209D63727105ADD4B30F871AFBC12B2EDF16E1F3DCB4EBAA`
- Fixed baseline diagnostic SQL SHA256: `0B74C0A2F0A6C0A3557C00EACBE1811087C8045EB9BF73332D0BCBB073C3BBB1`
- Fixed baseline diagnostic log SHA256: `1B7655F28C55319394FBA8EB4A6F9954B2742A7CDEF85DDE99BB3DB03B93BCC7`
- Phase5R final evidence SHA256: `546E304403F39942B737AD45DDA0511B3DA9738B6636F0A76FEF34DCBA321A9D`
- Phase6R apply SQL SHA256: `F3C4430F302CFB044A4D2A947D448EC8AB2F3862AC0D9A7D2F9924BD07F10DF1`
- Phase6R post-commit SQL SHA256: `0C66829D227E07694C319D972F9A7B4EA7A18B6520DF3CDFFA1AE4F9BF955B5D`
- Phase6R Recovery V2 wrapper SHA256: `A8505D85A657DEE7849BD73C5FE7D6E5902683D615EA97C99D252853FC27A093`
- Phase6R recovered post-commit log SHA256: `76E97308EBBA47DF6C6B175EC551CA2716FFA5A6AFB559F697A48481DAD415B4`
- Phase6R recovered final evidence SHA256: `CFE055B0A0B10F70352C72D20191CD5A1275A0D1EBEA1177777C25F0DC504673`

## Migration history

- `20260726013000` repaired and verified as Local/Remote applied.

## Recovery notes

- Initial wrapper failed closed before psql because it referenced a nonexistent metric diagnostic path.
- Recovery V2 corrected the Phase5R metric diagnostic path binding.
- Recovery V2 reached precheck, in-tx verification, COMMIT, and terminal post-commit verification.
- Recovery V2 did not persist apply/post-commit logs or final evidence; current state was reverified using read-only post-commit recovery evidence.
- No further B02 production apply is authorized.

## Scope boundaries

This closeout makes no business-flow E2E, UI smoke, broad DB-security, or product-readiness claim.

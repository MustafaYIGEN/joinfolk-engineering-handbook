# Supabase Migration Target Decision

## 1. Metadata

- Status: Draft
- Version: 0.2
- Owner: Mustafa / JoinFolk
- Last reviewed: TBD
- Source confidence: Operator decision + handbook audit/provenance reports
- canonical: false

## 2. Decision Summary

Decision: future accepted Supabase backend migration work should use `C:\dev\hostos\supabase\migrations` as the default working target, unless a later decision record supersedes this.

Decision status: accepted as future working target by operator.

This is a decision record for future accepted migration target selection. It is not final proof that `C:\dev\hostos\supabase` was the only historical canonical source, and it does not authorize modifying any Supabase tree.

## 3. Decision Scope

This decision applies to future accepted backend migration drafting only.

It does not apply to:

- Historical canonical-source claims.
- Cleanup or reconciliation of split-source migration histories.
- Production deployment path confirmation.
- Edge Function deployment.
- Proof check-in hardening implementation.
- Any production mutation.

Production SQL evidence remains stronger than local source assumptions for production-state claims.

## 4. Accepted Future Working Target

Accepted future working target:

- `C:\dev\hostos\supabase\migrations`

This means:

- New accepted backend migrations should be prepared under `C:\dev\hostos\supabase\migrations` unless a later decision supersedes this.
- `C:\dev\hostos\supabase` is the strongest overall migration-source candidate for the current production RPC surface.
- `C:\dev\hostos\supabase` is the working target for future accepted backend migration work.

This does not mean implementation is authorized. Future migrations must not be written or applied unless the scope is explicitly approved.

## 5. Explicit Non-Claims

This decision does not mean:

- `C:\dev\hostos\supabase` is proven to be the only historical canonical source.
- Split-source migration history is resolved.
- `C:\dev\joinfolk-web\supabase` is stale or safe to delete.
- `C:\dev\hostos\apps\mobile\supabase` is stale or safe to delete.
- Proof check-in hardening can be implemented immediately.
- Any migration can be applied to production.
- Any Supabase tree can be modified without explicit patch approval.
- Any split-source files may be deleted, moved, reconciled, or normalized.

## 6. Evidence Basis

Repository source-map evidence:

- Multiple Supabase trees exist:
  - `C:\dev\hostos\supabase`
  - `C:\dev\joinfolk-web\supabase`
  - `C:\dev\hostos\apps\mobile\supabase`
- `C:\dev\hostos\supabase` was identified as the primary plausible backend source, not canonical.

Manual deployment confirmation evidence:

- No GitHub Actions workflow evidence was found.
- No local `.github/workflows` evidence was found.
- No deployed Supabase Edge Functions were visible in the Supabase Dashboard.
- GitHub `joinfolk-platform` contains `supabase/migrations` and `supabase/functions` matching the shape of `C:\dev\hostos\supabase`.
- Production migration deployment path remains Unknown / Needs verification.

Production migration provenance evidence:

- 55 production Database Function / RPC names were compared against three local migration trees.
- `hostos` matched 35.
- `joinfolk-web` matched 16.
- Mobile matched 28.
- `hostos` had 18 unique matches.
- `joinfolk-web` had 8 unique matches.
- Mobile had 9 unique matches.
- `hostos` was the strongest overall migration-source candidate.
- Split-source evidence remains.

Domain evidence:

- Commerce / entitlement: strongest source tree is `hostos`.
- Notification / push database functions: strongest source tree is `hostos`.
- Venue / media / visual: split, but `hostos` best explains current venue/media/commerce RPCs.
- Transfer / ops: split between `joinfolk-web` and mobile.
- Check-in / proof: split; mobile strongest for proof-specific helpers, `hostos` strongest for older/core check-in.

Therefore, `hostos` is suitable as the future working target while historical canonical source remains unresolved.

## 7. Split-Source Caveat

Split-source evidence remains unresolved.

- `joinfolk-web\supabase` strongly implicates host identity transfer / ops migration history.
- `hostos\apps\mobile\supabase` strongly implicates proof-specific helpers and some transfer/visual history.
- `hostos\supabase` is strongest overall, especially for current commerce, entitlement, notification, push database functions, and venue/media/commerce RPCs.

This decision chooses a future working target. It does not reconcile historical source trees.

## 8. Operational Rule

For future accepted backend migration work, prepare migration drafts under `C:\dev\hostos\supabase\migrations` by default.

Before any migration is created, require:

1. Explicit patch scope approval.
2. Target path confirmation.
3. Production SQL/RPC evidence review.
4. Rollback/safety note.
5. No changes to `joinfolk-web` or mobile Supabase trees unless separately approved.

This decision can be superseded only by a later decision record with stronger deployment evidence.

## 9. Impact on Patch Planning

- Proof check-in patch planning may name `C:\dev\hostos\supabase\migrations` as the likely future target.
- Proof check-in hardening implementation still requires explicit patch approval.
- The proof check-in plan must continue to acknowledge split-source evidence, especially mobile proof-specific helper provenance.
- No migration drafting, production application, or Supabase tree modification is authorized by this decision.

## 10. Impact on Existing Handbook Docs

| Document | Impact |
| --- | --- |
| `RepositorySourceMap.md` | Working target decision strengthens the prior “primary plausible” classification for `hostos`, but does not make a historical canonical claim. |
| `SupabaseSourceOfTruthClassification.md` | Future target is now operator-accepted as `hostos\supabase\migrations`; historical source-of-truth unknowns remain. |
| `ManualDeploymentPathConfirmationReport.md` | Still valid: production deployment path remains Unknown / Needs verification, no GitHub Actions workflow evidence was found, and no deployed Supabase Edge Functions were visible. |
| `ProductionMigrationProvenanceReport.md` | Supports `hostos` as strongest overall migration-source candidate while preserving split-source caveat. |
| `SupabaseBackendGapReport` | Local-code findings using `hostos\supabase` are strengthened for future audit reference but still separate from production SQL evidence. |
| `SupabaseProductionParityVerificationReport` | Production SQL conclusions remain valid and stronger than local source assumptions. |
| `SupabaseFocusedBackendFollowUpReport` | Focused SQL findings remain mostly unaffected; check-in/proof source history remains split. |
| `SupabaseProofCheckinRpcHardeningPlan.md` | The plan may name `hostos\supabase\migrations` as likely target, but implementation remains blocked pending explicit patch approval. |

## 11. Required Guardrails Before Any Migration

Before any future migration is written or applied:

- Explicit patch scope must be approved.
- Target path must be reconfirmed as `C:\dev\hostos\supabase\migrations` or superseded by a later decision.
- Production SQL/RPC evidence must be reviewed for the specific change.
- Rollback/safety note must be documented.
- `joinfolk-web\supabase` and mobile `supabase` trees must not be changed unless separately approved.
- Production deployment path remains Unknown / Needs verification unless separately confirmed.

## 12. Remaining Unknowns

- Whether `C:\dev\hostos\supabase` was the only historical canonical source.
- Whether production migration history was assembled from multiple local trees.
- Why host identity transfer / ops migration history aligns strongly with `joinfolk-web` and mobile.
- Why proof-specific helper history aligns strongly with mobile.
- Whether `C:\dev\joinfolk-web\supabase` is active, stale, partial, or component-specific.
- Whether `C:\dev\hostos\apps\mobile\supabase` is active, stale, partial, or component-specific.
- The production migration deployment path.
- The deployment state and deployment source of Supabase Edge Functions.

## 13. Supersession Rule

This decision can be superseded only by a later decision record with stronger deployment evidence.

Examples of stronger evidence may include:

- Confirmed deployment/migration process documentation.
- Confirmed deployment configuration.
- Operator-approved repository ownership decision.
- Production provenance evidence that materially changes the target-path conclusion.

Until superseded, `C:\dev\hostos\supabase\migrations` is the default future working target for accepted backend migration drafts.

## 14. Non-Goals

- This document does not claim historical sole canonical source.
- This document does not include SQL.
- This document does not include cleanup instructions.
- This document does not include migration instructions.
- This document does not authorize modifications.
- This document does not authorize deleting, moving, reconciling, or normalizing split-source migration files.
- This document does not authorize proof check-in hardening implementation.
- This document does not authorize production changes.

## 15. Open Questions

- What is the actual production migration deployment path?
- Was production migration history assembled from multiple local trees?
- Should host identity transfer / ops history be reconciled into the future working target later?
- Should proof-specific mobile history be reconciled into the future working target later?
- Are `joinfolk-web\supabase` and mobile `supabase` active component-owned sources, historical forks, or stale?
- What later evidence would be sufficient to supersede this working target decision?

## 16. No-Modification Confirmation

For this decision record:

- No application code files were modified. This decision record was created from handbook audit/provenance reports and operator decision.
- No production connection was made.
- Supabase CLI was not run.
- No migrations were run or generated.
- No builds were run.
- No tests were run.
- No cleanup plan was created.
- No patch plan was created.
- No migration plan was created.
- No Supabase tree modification was authorized.
- No proof check-in hardening implementation was authorized.
- No files were staged or committed.
